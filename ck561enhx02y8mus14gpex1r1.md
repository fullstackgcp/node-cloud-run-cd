## A Developer's Guide to Building Serverless Applications on the Cloud - Part 1 (Deployment)

In this series, I'll discuss few topics crucial to building serverless applications while using Google Cloud, they would span across deployment, CI/CD, tooling, backend-as-a-service and more.

> In this article, I will discuss how Developers at DevShopZ *easily* moved from local development to the Cloud. This covers simple functions, web-based application, and containerized applications including code samples.

If you are new to Serverless, check out [this post](https://fullstackgcp.com/journey-to-serverless-on-google-cloud-platform-ck101zpb2005ek7s1rcsgqnug) 


DevShopZ recently started a new project for a client, her engineers had made lots of progress in building the project, however, the project manager wants it deployed so the client can have an idea of what he's getting and also provide feedback to the team.

Developers at DevShopZ started the project by building simple API endpoints, then moved on to integrating the frontend and other tools such as Docker.
We shall see how the deployment was done at each stage of the project.


## Before you begin:
-  The developers made use of Google Cloud account for free, details [here](cloud.google.com/free) 
-  They created a new GCP project, you can also do the same [here](https://console.cloud.google.com/project)  
-  The developers used the [Google Cloud Shell terminal](https://cloud.google.com/shell/) to execute the deployment commands.


# *Deploying Functions*

The Engineers built the first simple endpoint which is a Python function that returns a JSON message.
_Tip: You could use any language of choice_


%[https://gist.github.com/Timtech4u/c6b701830cc4ab2bef09d8f0f4d52766]


_The endpoint function was deployed to Cloud Functions_


> [Google Cloud Functions](https://cloud.google.com/functions/) is a lightweight compute solution for developers to create single-purpose, stand-alone functions that respond to cloud events without the need to manage a server or runtime environment. 


```
gcloud functions deploy hello_world --runtime python37 --trigger-http
```


Execute the command above on Cloud Shell after cloning the sample codes from the [Gist](https://gist.github.com/Timtech4u/c6b701830cc4ab2bef09d8f0f4d52766) or setting up your own codes.



![Screenshot from 2020-01-09 00-59-14.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1578527996651/66LCVaKKK.png)
_Deployed Function_


# *Deploying Web Applications (without containers)*

The project has grown into a full Web Application built using the Flask Web framework and now includes static files.
_Tip: You could use any framework of choice_

%[https://gist.github.com/Timtech4u/7b420b74d83cf9e42ab0d7bfce60c8b6]

_The Web Application was deployed to App Engine_

>  [App Engine](https://cloud.google.com/appengine/)  is a fully managed, serverless platform for developing and hosting web applications at scale. You can choose from several popular languages, libraries, and frameworks to develop your apps, then let App Engine take care of provisioning servers and scaling your app instances based on demand.

```
gcloud app deploy
```

Execute the command above on Cloud Shell after cloning the sample codes from the [Gist](https://gist.github.com/Timtech4u/7b420b74d83cf9e42ab0d7bfce60c8b6) or setting up your own codes 


![Screenshot from 2020-01-09 00-51-57.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1578527636802/A4E5gRny6.png)
_Deployed Web Application_


# *Deploying containers*

The team decided to adopt containerization technologies by Dockerizing the existing Web Application. The source codes were updated to include a *Dockerfile* that was used to build the container image.


%[https://gist.github.com/Timtech4u/fda016845505878fe412e8c1881f804b]


_There are multiple options for deploying container images on Google Cloud,  [see this short video](https://www.youtube.com/watch?v=jh0fPT-AWwM)_

>  [Cloud Run](https://cloud.google.com/run/) is a managed compute platform that automatically deploys and scales your stateless containers. Cloud Run is serverless; it abstracts away all infrastructure management, so you can focus on what matters most — building great applications.

```
gcloud builds submit --config cloudbuild.yaml
```

The **cloudbuild.yaml** file is a configuration file for  [Google Cloud Build](https://cloud.google.com/cloud-build/)  which contains two steps:
one to build the container image & upload to  [Google Container Registry](https://cloud.google.com/container-registry); step two then deploys the image to Cloud Run.

Execute the command above on Cloud Shell after cloning the sample codes from the [Gist](https://gist.github.com/Timtech4u/fda016845505878fe412e8c1881f804b) or setting up your own codes 


![Screenshot from 2020-01-09 01-38-21.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1578530328612/oM88yjeJU.png)
_Deployed Containerized Web App_


# Useful Links
-  [Google Cloud - Choosing a Serverless Option](https://cloud.google.com/serverless-options/)
-  [Cloud Function - Functions Framework](https://cloud.google.com/functions/docs/functions-framework)
-  [App Engine -  Choosing an App Engine environment](https://cloud.google.com/appengine/docs/the-appengine-environments)
-  [Ahmetb - Cloud Run FAQ](https://github.com/ahmetb/cloud-run-faq)
-  [Unofficial - Google Cloud Compute Options Guide](https://github.com/Timtech4u/gcp_compute_options_guide)


> Stay tuned for the next article where I will discuss a more exciting serverless topic.