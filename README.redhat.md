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
mvn fabric8:deploy
```

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

To access the endpoint you first need to create an OpenShift route for the service so that it can be exposed externally.  You can use the 'oc create route' command to create the route and the 'oc get routes' to get the host name for
the route that you created.  Example:


    $ oc create route edge example1 --service=spring-boot-cxf-jaxrs
    route "example1" created
    
    $ oc get routes example1
    NAME       HOST/PORT                                PATH      SERVICES                PORT      TERMINATION
    example1   example1-myproject.192.168.64.2.xip.io             spring-boot-cxf-jaxrs   http      edge

You can then use the host report to access the service. 

`https://example1-myproject.192.168.64.2.xip.io/services/helloservice/sayHello/FIS`
will display "Hello FIS, Welcome to CXF RS Spring Boot World!!!"


`http://example1-myproject.192.168.64.2.xip.io/services/helloservice/swagger.json` will return a Swagger JSON
description of services.


### Integration Testing

The example includes a [fabric8 arquillian](https://github.com/fabric8io/fabric8/tree/v2.2.170.redhat/components/fabric8-arquillian) OpenShift Integration Test. 
Once the container image has been built and deployed in OpenShift, the integration test can be run with:

    mvn test -Dtest=*KT

The test is disabled by default and has to be enabled using `-Dtest`. Open Source Community documentation at [Integration Testing](https://fabric8.io/guide/testing.html) and [Fabric8 Arquillian Extension](https://fabric8.io/guide/arquillian.html) provide more information on writing full fledged black box integration tests for OpenShift. 

