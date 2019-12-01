## Building Artifacts on the Cloud


![aerial-greyscale-photography-of-new-york-city-700974.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1575211599042/pha-9Dzuk.jpeg)

Software developers often build artifacts (Docker containers, WAR files, APK bundles etc.) from codes.

This often requires reliable internet connection, high processing power, sufficient storage space, constant upgrade of build tools among others.

Built artifacts could be published e.g. deploying containers to the cloud or hosting APK files on Play store.
However, this also includes steps that might be difficult when done manually or on a local machine.

> In this tutorial, you will leverage [Cloud Build](https://cloud.google.com/cloud-build/) to build artifacts extremely fast on the Cloud using the following examples: 
- Building container artifacts and storing on Container Registry.  
- Building non-container artifacts and storing on Cloud Storage.  
- Automating builds from repositories using Cloud Build Triggers.  

## Cloud Build

 [Cloud Build](https://cloud.google.com/cloud-build)  is a service that executes your builds on Google Cloud Platform infrastructure.  

You can write build configurations to provide instructions to Cloud Build on what tasks to perform and can either use the build step/builders provided by  [Cloud Build](https://github.com/GoogleCloudPlatform/cloud-builders)  ,  [Cloud Build community](https://github.com/GoogleCloudPlatform/cloud-builders-community)  or write your own  [custom build steps/builders](https://cloud.google.com/cloud-build/docs/create-custom-build-steps) .

Cloud Build offers the first 120 build minutes per day for free.
You can simple get started by  [enabling Cloud Build](https://console.cloud.google.com/cloud-build/builds)  for your  [GCP project](https://console.cloud.google.com/project) .  

Before you begin, ensure you have setup Cloud [SDK](https://cloud.google.com/sdk/docs/) or  [Cloud Shell](https://cloud.google.com/shell). 

## Building Docker containers to Container Registry 

Cloud Build allows you to build Docker images and push the built images to Container Registry.

You can get my sample source codes for a *Containerized Flask Application* [here](https://gist.github.com/Timtech4u/6639a92b4197ea831ba9b975c9b34a76)  and *Dockerfile* below:

```
FROM python:3.7-stretch
RUN apt-get update -y
RUN apt-get install -y python-pip python-dev build-essential
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["app.py"]
```

There should be a *Dockerfile* on your project root directory and then you can run the following command to Build on the Cloud which also publishes your Docker container to Container Registry.
```
    gcloud builds submit --tag gcr.io/[PROJECT_ID]/my-image .
```

You've just built a Docker image named **my-image** using a Dockerfile and pushed the image to Container Registry.

You can also build on the Cloud using build configuration file `cloudbuild.yaml` which is created on the project root directory with content as follows:

```
steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/[PROJECT_ID]/my-image', '.' ]
images:
- 'gcr.io/[PROJECT_ID]/my-image'
```

Then trigger the build using the following command:

```
gcloud builds submit --config cloudbuild.yaml .
```

Note that:
- `[PROJECT_ID]` should be replaced with your actual project ID.
- `.` is used to the current working directory for build time.
- You can specify other tags for your image using `:` e.g. `my-image:dev` and also pass a longer timeout for larger images e.g. `--timeout=600s`


## Building non-containers artifacts to Cloud Storage

Cloud Build supports [builders ](https://cloud.google.com/cloud-build/docs/cloud-builders) that you can reference to execute specific tasks such as *npm, go, gradle, gradle etc*. 

However, I would use a [custom gradle builder](https://gcr.io/fullstackgcp/gradle) to build an Android project APK and then enable Cloud Build to store the artifacts to a Cloud Storage Bucket.

You can get my sample source codes for the Android application [here](https://github.com/Timtech4u/gcb-android-tutorial) and modified build configuration file `cloudbuild.yaml` below:

```
steps:
# Set a persistent volume according to https://cloud.google.com/cloud-build/docs/build-config (search for volumes)
- name: 'ubuntu'
  volumes:
  - name: 'vol1'
    path: '/persistent_volume'
  args: ['cp', '-a', '.', '/persistent_volume']
  
# Build APK with Gradle Image from mounted /persistent_volume using name: vol1
- name: 'gcr.io/cloud-builders/docker'
  volumes:
  - name: 'vol1'
    path: '/persistent_volume'
  args: ['run', '-v', 'vol1:/home/app', '--rm', 'gcr.io/fullstackgcp/gradle', '/bin/sh', '-c', 'cd /home/app && ./gradlew clean assembleDebug']

# Push the APK file from vol1 to your GCS Bucket.
artifacts:
  objects:
    location: 'gs://[STORAGE_LOCATION]/'
    paths: ['*.apk']


timeout: 1200s

```

Then trigger the build using the following command:
```
gcloud builds submit --config cloudbuild.yaml .
```

Note that:
- [STORAGE_LOCATION] represents the path to your Cloud Storage bucket.
- `gcr.io/fullstackgcp/gradle` is the gradle builder
- [Cloud Build service account](https://cloud.google.com/cloud-build/docs/securing-builds/configure-access-control)  should have access to write to Cloud Storage OR that the Bucket is publicly open.


## Automating builds

Cloud Build can import source code from Google Cloud Storage, Cloud Source Repositories, GitHub, or Bitbucket, execute a build to your specifications, and produce artifacts.

We’ll proceed to create a trigger that listens to changes on a particular branch on our source codes and performs the operation in our cloudbuild.yaml configuration file.

- Open the  [Cloud Build Triggers page](https://console.cloud.google.com/cloud-build/triggers)
- Click on **Connect repository** 
- Select your **Source Code option** and **Continue**.
- **Authenticate** and Create **Push Trigger** with required inputs.


![cb.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1575214414652/lQr19DXak.png)
Above is a sample Cloud Build Trigger page.


You can monitor Build Triggers on the  [History page](https://console.cloud.google.com/cloud-build/builds) 

> If you would like some insights on where to deploy your artifacts, check out  [Google Cloud Compute Options Guide](https://github.com/Timtech4u/gcp_compute_options_guide) .


**Additional Resources on Cloud Build**

-  [Cloud Build Documentation](https://cloud.google.com/cloud-build/docs/) 
-  [Official Cloud Builder](https://github.com/GoogleCloudPlatform/cloud-builders)
-  [Community Cloud Builders](https://github.com/GoogleCloudPlatform/cloud-builders-community)
- [Awesome Cloud Build](https://github.com/Timtech4u/awesome-cloudbuild)
-  [Google Cloud Platform Awesome List](https://github.com/GoogleCloudPlatform/awesome-google-cloud)