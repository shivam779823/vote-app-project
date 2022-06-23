Vote App CI/CD Using Cloud Buid 
=========

A simple distributed application running across multiple Google Kubernetes Engine containers.

Getting started
---------------



The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).


Run the app in Kubernetes
-------------------------

The folder k8s contains the yaml specifications of the Voting App's services.


Run the following command to create the deployments and services objects: (manually)

```

$ kubectl create -f k8s/
```

deployment "db" created
service "db" created
deployment "redis" created
service "redis" created
deployment "result" created
service "result" created
deployment "vote" created
service "vote" created
deployment "worker" created



The vote interface is then available on port 31000 on each host of the cluster, the result one is available on port 31001.

CI/CD 
----

Git -> Github/SCM -> Cloud Build -> Google kubernetes Engine 
```
1) gcloud container clusters create vote-cluster --zone us-central1-a
2) Create trigger on cloud Buid for cloudbuild.yaml file
3) git push code 
4) Kubectl get svc 
5) vist External Ip of vote-app
6) vist External IP of Result app in (new tab)

``` 

Architecture
-----

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) or [ASP.NET Core](/vote/dotnet) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) or [NATS](https://hub.docker.com/_/nats/) queue which collects new votes
* A [.NET Core](/worker/src/Worker), [Java](/worker/src/main) or [.NET Core 2.1](/worker/dotnet) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) or [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) database backed by a Docker volume
* A [Node.js](/result) or [ASP.NET Core SignalR](/result/dotnet) webapp which shows the results of the voting in real time


Notes
-----

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple 
example of app used for CI/CD on  google cloud using cloud build 
