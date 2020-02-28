## Deploying Container Images to GKE using Kubectl

> This article is a based on [Kubernetes on GKE : Series Intro](https://fullstackgcp.com/kubernetes-on-gke-series-intro-5969ae700e99) , which covers some basics of Google Kubernetes Engine (GKE) and how to setup a cluster.

Kubectl is a command line tool for controlling Kubernetes clusters, which allows you to run commands against Kubernetes clusters to deploy applications, inspect and manage cluster resources, or view logs.
We’ll use the kubectl tool to deploy a container image to the Google Kubernetes Engine Cluster.

GKE uses Kubernetes objects to create and manage your cluster’s resources. Kubernetes provides the [Deployment](https://cloud.google.com/kubernetes-engine/docs/concepts/deployment) object for deploying stateless applications like web servers. [Service](https://cloud.google.com/kubernetes-engine/docs/concepts/service) objects define rules and load balancing for accessing your application from the internet.

To install Kubectl, follow steps described [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

## Configure kubectl

After creating your cluster, you need to get authentication credentials to interact with the cluster, you can do with a command line that has both _kubectl_ and [_gcloud_](https://cloud.google.com/sdk) installed or simply use the [Google Cloud Shell](https://cloud.google.com/shell).

*   Login to Gcloud using:


```
gcloud auth login
```


_This would open a browser window for you to login. You could also set a default project if you haven’t._

*   To configure kubectl to manage your GKE cluster, execute:


```
gcloud container clusters get-credentials my-cluster
```


_Where_ **_my-cluster_** _is the name of the cluster you created earlier._

## Creating a deployment

In this step, you would create deployments using the _kubectl_ commands and **not** by writing YAML configurations.

_To create a new deployment on your GKE cluster, run the following command:_


```
kubectl create deployment hello-server — image=gcr.io/google-samples/hello-app:1.0
```


_The command creates a Deployment named_ **_hello-server_** _on your configured cluster. The Deployment’s Pod runs the_ **_hello-app_** _container image._ Inspect the running Pods by running:


```
kubectl get pods
```


## Exposing the Deployment

You need to expose it to the internet so that users can access it. You can expose your application by creating a Service, which is a Kubernetes resource that exposes your application to external traffic.

_To create a new service on your GKE cluster, run the following command:_


```
kubectl expose deployment hello-server — type=LoadBalancer — port 8080
```


Inspect the `hello-server` Service by running:


```
kubectl get service hello-server
```


_It might take some minutes for an external IP address to be generated. Run the above command again if the EXTERNAL-IP column is in “pending” status._

To learn more about the Kubectl, check out [Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

## Other Articles in this series
- [Kubernetes on GKE : Series Intro](https://fullstackgcp.com/kubernetes-on-gke-series-intro-5969ae700e99)
- [Deploying Ready Apps to GKE using Cloud Marketplace](https://fullstackgcp.com/deploying-ready-apps-to-gke-using-cloud-marketplace-4353ea499546)
- [Managing Apps on GKE using Helm Charts](#)
