# helloworld: Helloworld Example
Pete Muir

The helloworld quickstart demonstrates the use of CDI and Servlet 3 and is a good starting point to verify JBoss EAP is configured correctly.

## What is it?

The helloworld quickstart demonstrates the use of CDI and Servlet 3 in Red Hat JBoss Enterprise Application Platform 7.4.

## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.4 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See Configure Maven to Build and Deploy the Quickstarts to make sure you are configured correctly for testing the quickstarts.

## Use of the EAP_HOME and QUICKSTART_HOME Variables

In the following instructions, replace EAP_HOME with the actual path to your JBoss EAP installation. The installation path is described in detail here: Use of EAP_HOME and JBOSS_HOME Variables.

When you see the replaceable variable QUICKSTART_HOME, replace it with the path to the root directory of all of the quickstarts.

## Start the JBoss EAP Standalone Server

Open a terminal and navigate to the root of the JBoss EAP directory.

Start the JBoss EAP server with the default profile by typing the following command.

```$ EAP_HOME/bin/standalone.sh```

**NOTE**

For Windows, use the `EAP_HOME\bin\standalone.bat` script.

## Build and Deploy the Quickstart

Make sure you start the JBoss EAP server as described above.

Open a terminal and navigate to the root directory of this quickstart.

Type the following command to build the artifacts.

```$ mvn clean package wildfly:deploy```

This deploys the helloworld/target/helloworld.war to the running instance of the server.

You should see a message in the server log indicating that the archive deployed successfully.

## Access the Application

The application will be running at the following URL: http://localhost:8080/helloworld/.

## Undeploy the Quickstart

When you are finished testing the quickstart, follow these steps to undeploy the archive.

Make sure you start the JBoss EAP server as described above.

Open a terminal and navigate to the root directory of this quickstart.

Type this command to undeploy the archive:

```$ mvn wildfly:undeploy```

## Run the Quickstart in Red Hat CodeReady Studio or Eclipse

You can also start the server and deploy the quickstarts or run the Arquillian tests in Red Hat CodeReady Studio or from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use Red Hat CodeReady Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.adoc#use_red_hat_jboss_developer_studio_or_eclipse_to_run_the_quickstarts).

## Debug the Application

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

```$ mvn dependency:sources```

## Getting Started with JBoss EAP for OpenShift

This document contains the basic instructions to build and deploy this quickstart to JBoss EAP for OpenShift or JBoss EAP for OpenShift Online.

See Getting Started with JBoss EAP for OpenShift Container Platform for more detailed information about building and running applications on JBoss EAP for OpenShift.

See Getting Started with JBoss EAP for OpenShift Online for more detailed information about building and running applications on JBoss EAP for OpenShift Online.

## Prepare OpenShift for Quickstart Deployment

Log in to your OpenShift instance using the oc login command.

Create a new project for the quickstart in OpenShift. You can create a project in OpenShift using the following command.

```$ oc new-project helloworld-project```

Before you can import and use the OpenShift image for JBoss EAP for OpenShift , you must configure authentication to the Red Hat Container Registry.

Create an authentication token using a registry service account to configure access to the Red Hat Container Registry. You need not use or store your Red Hat account’s username and password in your OpenShift configuration when you use an authentication token.

### Procedure

Follow the instructions on Red Hat Customer Portal to create an authentication token using a registry service account.

Download the YAML file containing the OpenShift secret for the token.

You can download the YAML file from the OpenShift Secret tab on your token’s Token Information page.

Create the authentication token secret for your OpenShift project using the YAML file that you downloaded:

```oc create -f 1234567_myserviceaccount-secret.yaml```

Configure the secret for your OpenShift project using the following commands, replacing the secret name below with the name of your secret created in the previous step.

```oc secrets link default 1234567-myserviceaccount-pull-secret --for=pull```
```oc secrets link builder 1234567-myserviceaccount-pull-secret --for=pull```

#### Additional resources

[Create an authentication token using a registry service account](https://access.redhat.com/RegistryAuthentication#registry-service-accounts-for-shared-environments-4)

[Configuring access to secured registries](https://access.redhat.com/documentation/en-us/openshift_container_platform/3.11/html/developer_guide/dev-guide-managing-images#allowing-pods-to-reference-images-from-other-secured-registries)

[Configuring authentication to the Red Hat Container Registry](https://access.redhat.com/RegistryAuthentication)

## Import the Latest JBoss EAP for OpenShift Imagestreams and Templates

**IMPORTANT**

If you are building and deploying this quickstart on JBoss EAP for OpenShift, you must configure authentication to the Red Hat Container Registry before you import the image streams and templates into your namespace. Getting Started with JBoss EAP for OpenShift Container Platform provides an example of one way to configure authentication to the registry. For additional information, see Red Hat Container Registry Authentication on the Red Hat Customer Portal.

Configuration of authentication to the registry is not necessary if you are building and deploying this quickstart on JBoss EAP for OpenShift Online.

### Procedure
Use the following commands to import the latest JDK 8 and JDK 11 image streams and templates for the OpenShift image for JBoss EAP for OpenShift, into your OpenShift project’s namespace.

Import JDK 8 image streams:

```oc replace --force -f https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap74/eap74-openjdk8-image-stream.json```

This command imports the following imagestreams:

The JDK 8 builder imagestream: jboss-eap74-openjdk8-openshift

The JDK 8 runtime imagestream: jboss-eap74-openjdk8-runtime-openshift

Import JDK 11 image stream:

```oc replace --force -f https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap74/eap74-openjdk11-image-stream.json```

This command imports the following imagestreams:

* The JDK 11 builder imagestream: jboss-eap74-openjdk11-openshift
* The JDK 11 runtime imagestream: jboss-eap74-openjdk11-runtime-openshift

Import the JDK 8 and JDK 11 templates:

```
for resource in \
  eap74-amq-persistent-s2i.json \
  eap74-amq-s2i.json \
  eap74-basic-s2i.json \
  eap74-https-s2i.json \
  eap74-sso-s2i.json \
  eap74-starter-s2i.json \
  eap74-third-party-db-s2i.json \
  eap74-tx-recovery-s2i.json
do
  oc replace --force -f \
https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap74/templates/${resource}
done
```

**NOTE**

The JBoss EAP image streams and templates imported using the above command are only available within that OpenShift project.

If you have administrative access to the general openshift namespace and want the image streams and templates to be accessible by all projects, add -n openshift to the oc replace line of the command. For example:

```
oc replace -n openshift --force -f \
```

## Deploy the JBoss EAP Source-to-Image (S2I) Quickstart to OpenShift

### Procedure

Create a new OpenShift application using the JBoss EAP for OpenShift image and the quickstart’s source code. Use the following command to use the eap74-basic-s2i template with the JDK 8 images and the helloworld source code on GitHub.

```$ oc new-app --template=eap74-basic-s2i \
 -p EAP_IMAGE_NAME=jboss-eap74-openjdk8-openshift:latest \
 -p EAP_RUNTIME_IMAGE_NAME=jboss-eap74-openjdk8-runtime-openshift:latest \
 -p IMAGE_STREAM_NAMESPACE="helloworld-project" \
 -p SOURCE_REPOSITORY_URL="https://github.com/jboss-developer/jboss-eap-quickstarts" \
 -p SOURCE_REPOSITORY_REF="7.4.x" \
 -p CONTEXT_DIR="helloworld"
 ```

* `--template` The template to use.
* `-p IMAGE_STREAM_NAMESPACE` The latest images streams and templates were imported into the project’s namespace, so you must specify the namespace of where to find the image stream. This is usually the OpenShift project’s name.
* `-p SOURCE_REPOSITORY_URL` The URL to the repository containing the application source code.
* `-p SOURCE_REPOSITORY_REF` The Git repository reference to use for the source code. This can be a Git branch or tag reference.
* `-p CONTEXT_DIR` The directory within the source repository to build.

Alternatively, to create the quickstart application using the JDK 11 images enter the following command.

```$ oc new-app --template=eap74-basic-s2i \
 -p EAP_IMAGE_NAME=jboss-eap74-openjdk11-openshift:latest \
 -p EAP_RUNTIME_IMAGE_NAME=jboss-eap74-openjdk11-runtime-openshift:latest \
 -p IMAGE_STREAM_NAMESPACE="helloworld-project" \
 -p SOURCE_REPOSITORY_URL="https://github.com/jboss-developer/jboss-eap-quickstarts" \
 -p SOURCE_REPOSITORY_REF="7.4.x" \
 -p CONTEXT_DIR="helloworld"
 ```

* `--template` The template to use.
* `-p IMAGE_STREAM_NAMESPACE` The latest images streams and templates were imported into the project’s namespace, so you must specify the namespace of where to find the image stream. This is usually the OpenShift project’s name.
* `-p SOURCE_REPOSITORY_URL` The URL to the repository containing the application source code.
* `-p SOURCE_REPOSITORY_REF` The Git repository reference to use for the source code. This can be a Git branch or tag reference.
* `-p CONTEXT_DIR` The directory within the source repository to build.

**NOTE**

A template can specify default values for many template parameters, and you might have to override some, or all, of the defaults. To see template information, including a list of parameters and any default values, use the command oc describe template TEMPLATE_NAME.

**TIP**

It is possible to trim down the JBoss EAP for OpenShift image that will be used to run this quickstart. To do so, please add the -p GALLEON_PROVISION_LAYERS=<galleon layers> argument when creating the new application. Please refer to the JBoss EAP documentation for the list of supported galleon layers.
Retrieve the name of the build configuration.

```$ oc get bc -o name```

Use the name of the build configurations from the previous step to view the Maven progress of the builds.

```
$ oc logs -f bc/${APPLICATION_NAME}-build-artifacts

Push successful
$ oc logs -f bc/${APPLICATION_NAME}

Push successful
```

For example, for the previously created application, the following command shows the progress of the Maven builds.

```
$ oc logs -f bc/eap74-basic-app-build-artifacts

Push successful
$ oc logs -f bc/eap74-basic-app

Push successful
```

## OpenShift Post Deployment Tasks

Depending on your application, you might need to complete some tasks after your OpenShift application has been built and deployed.

Examples of post-deployment tasks include the following:

Exposing a service so that the application is viewable from outside of OpenShift.

Scaling your application to a specific number of replicas.

### Procedure

Get the service name of your application using the following command.

```$ oc get service```

Optional: Expose the main service as a route so you can access your application from outside of OpenShift. For example, for the previously created application, use the following command to expose the required service and port.

**NOTE** If you used a template to create the application, the route might already exist. If it does, continue on to the next step.

```$ oc expose service/eap74-basic-app --port=8080```

Get the URL of the route.

```$ oc get route```

Access the application in your web browser using the URL. The URL is the value of the HOST/PORT field from previous command’s output.

**NOTE** For example, to interact with this quickstart , the root of its application URLs should be https://HOST_PORT_Value/.

Optionally, you can scale up the application instance by running the following command. This command increases the number of replicas to 3.

```$ oc scale deploymentconfig DEPLOYMENTCONFIG_NAME --replicas=3```

For example, for this` quickstart, use the following command to scale up the application.

```$ oc scale deploymentconfig/eap74-basic-app --replicas=3```