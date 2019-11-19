## Scheduling periodic jobs with Cloud Scheduler ⏰

> Cloud Scheduler is a fully managed enterprise-grade cron job scheduler. It allows you to schedule virtually any job, including batch, big data jobs, cloud infrastructure operations, and more.   

![enterprise-grade-scheduler.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1574150170177/-bVnPh7xH.png)


Automating tasks that re-occur is definitely at the heart of every software developer. Google Cloud Platform has a tool that allows users to schedule jobs while maintaining the usual unix-cron format.  
Cloud Scheduler can be referred to as *Cronjob as a Service* tool, it is fully managed by Google Cloud Platform so you don't need to manage the scheduler's underlying infrastructure.

Cloud Scheduler can be used for multiple use cases such as making requests to an HTTP/S Endpoint, invoking a Pub/Sub topic, making database updates and push notifications, triggering CI/CD pipelines, scheduling tasks such as image uploads and sending an email, or even invoking Cloud Functions.  
In this article, we would simply use Cloud Scheduler to make simple requests to an HTTP Endpoint.  


## Setting up Cloud Scheduler  

Visit [Cloud Scheduler](https://console.cloud.google.com/cloudscheduler) and click on * ➕ Create Job*

- Enter job name
- Set *Frequency* : every 24 hours
- *Target* : Select HTTP
- Set *URL* :  (Use your Application or Functions URL)
- *HTTP Method* : Select GET

*You can change values above to fit your use case.*

![Cloud-Scheduler.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1574150947725/wE9sLzr_F.png)

I wrote a tool that utilizes Cloud Scheduler to keep  [Serverlesss](https://fullstackgcp.com/journey-to-serverless-on-google-cloud-platform-ck101zpb2005ek7s1rcsgqnug) services warm.  

%[https://twitter.com/NeoPrimeAws/status/1196223053794926592]

If you face problems with HTTP headers or other issues while using Cloud Scheduler, feel free to check out  [John Hanley's](https://twitter.com/NeoPrimeAws)  answers on  [Stack Overflow.](https://stackoverflow.com/search?q=user:8016720+[google-cloud-scheduler) 

Cloud Scheduler can also be used with Firebase Cloud Functions which automatically configures Cloud Scheduler, along with a Pub/Sub topic, that invokes the function that you define using the Cloud Functions for Firebase SDK.  [Read more
](https://firebase.google.com/docs/functions/schedule-functions)

In addition, Stackdriver integrates with  Cloud Scheduler providing *powerful logging* for greater transparency into job execution and performance.

### Pricing

Cloud Scheduler is simple and pay-for-use; where you pay for the number of jobs you consume per month.  Google Cloud generously also allows you create 3 free jobs per month while you only per for others at $0.10/job/month

### Awesome Resource
📖  [Cloud Scheduler Documentation](https://cloud.google.com/scheduler/docs/quickstart)  
🔥  [Scheduled Cloud Functions from Fireship ](https://fireship.io/lessons/cloud-functions-scheduled-time-trigger/)  
🕶️ [Awesome List for Google Cloud Platform](https://github.com/GoogleCloudPlatform/awesome-google-cloud)