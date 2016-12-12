# Spring-Boot CXF JAXRS QuickStart

This example demonstrates how you can use Apache CXF with Spring Boot.

The quickstart uses Spring Boot to configure a little application that includes a CXF JAXRS endpoint with Swagger enabled.


### Building

The example can be built with

    mvn clean install


### Running the example locally

The example can be run locally using the following Maven goal:

    mvn spring-boot:run


### Running the example in OpenShift

It is assumed a running OpenShift platform is already running. 

Assuming your current shell is connected to OpenShift so that you can type a command like

```
oc get pods
```

Then the following command will package your app and run it on OpenShift:

```
mvn fabric8:run
```
The output log will give the URL to access the endpoint, something like
```
[INFO] F8:[SVC] spring-boot-cxf-jaxrs: http://192.168.64.7:32225
```

You will need to append the context-path to access the JAX-RS service so the url is something like

http://192.168.64.7:32225/services/helloservice/sayHello/

To list all the running pods:

    oc get pods

Then find the name of the pod that runs this quickstart, and output the logs from the running pods with:

    oc logs <name of pod>


### Running via an S2I Application Template

Applicaiton templates allow you deploy applications to OpenShift by filling out a form in the OpenShift console that allows you to adjust deployment parameters.  This template uses an S2I source build so that it handle building and deploying the application for you.

First, import the Fuse image streams:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/fis-image-streams.json

Then create the quickstart template:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/quickstarts/spring-boot-cxf-jaxrs-template.json

Now when you use "Add to Project" button in the OpenShift console, you should see a template for this quickstart. 

### Accessing the Endpoints    

To access the endpoint, use the host and port from the output log when run mvn fabric8:run

http://192.168.64.7:32225/services/helloservice/sayHello/FIS
will display "Hello FIS, Welcome to CXF RS Spring Boot World!!!"


http://192.168.64.7:32225/services/helloservice/swagger.json will return a Swagger JSON
description of services.
