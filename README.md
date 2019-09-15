# README

This project deploys an app, which features a microservice architecture, on a Kubernetes Cluster on AWS.

The app itself was built by [@grutt](https://github.com/scheele) and [@scheeles](https://github.com/scheeles) in partnership with Udacity. 
It is comprised of the following microservices:
* frontend service -- udacity-c3-frontend/
* nginx proxy service -- udacity-c3-deployment/
* a backend service to handle user data -- udacity-c3-restapi-user/
* another backend service to handle feed data -- udacity-c3-restapi-feed/

In addition, this app also makes use of a Postgres Database Server also deployed on AWS.


## Notes


### Jenkinsfile
The app is deployed à la DevOps, and we use Jenkins as our CI/CD tool of choice. 
Our Jenkins server monitors the GitHub repo for changes; it initiates the build process pipeline whenever it detects a change: build the image, push the image to Image Repository, run the containers on pods.


## App Update Mechanism

This [domain]((http://testdevops.riyanchristy.net)) name has a weighted routing linked to two A records, each pointing to the Load Balancer representing a Kubernetes Cluster, where a version of the app lives.

If we want to launch an updated app, we will place the updates on one of the Kubernetes clusters: the [Green](http://a5029f627cb2e11e9a39f0a24a18a98b-990378143.us-east-2.elb.amazonaws.com) cluster. 

If it goes well in the green cluster, we will apply the changes in the other cluster, [Blue](http://aa6c96850d75711e9a2cd06bfda61de7-1210280241.us-east-2.elb.amazonaws.com:8100) cluster.

***Currently (at the time of this writing, Sep 16, 2019), we are featuring a simulation of an app update***: the Green Cluster contains the updated app being rolled out (represented by a screen showing the text 'Deployment Successful'.
