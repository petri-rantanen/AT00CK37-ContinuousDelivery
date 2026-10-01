# Installation Instructions #

You'll need the following requirements installed for this task:
- Java 21+
- Maven
- Docker

If you have these installed, you can skip over the next section, and continue with the actual task.

## Installing Java ##

You can follow Oracle's instruction if you want, but OpenJDK is generally available on all Linux distributions by default.

You can check your current version with the command:

java --version

If it shows version 21 or higher (e.g., openjdk 25 2025-09-16), you are good to go!

If NOT, then you must install Java. First, check which versions you have available:

apt-cache search openjdk|grep -e -jdk

You are looking for package named "openjdk-VERSIONNUMBER-jdk". Any version higher or equal to 21 will work, install it with:

sudo apt install openjdk-VERSIONNUMBER-jdk

Re-check your Java version with:

java --version

It should now show your updated Java version.

## Installing Maven ##

Maven should be available on most Linux distributions. You can install it with:

sudo apt install maven

You can check your Maven version with:

mvn --version

Any relatively recent version (3.0.0+) should work.

## Installing Docker ##

If you don't have Docker installed, you must install it now. You can use the following command to check your current Docker version:

docker --version

Any relatively recent version should work (20.0.0+).

You can follow the instruction at dockerdocs to install Docker: https://docs.docker.com/engine/install/

If you are unsure whether you have a broken or working version already installed, it might be worthwhile to follow the installation for uninstalling old/conflicting versions. Otherwise, you can skip those over.

For Debian/Ubuntu, going through steps 1 through 3 shoudl be enough (in section "Install using the apt repository").

# The Task #

When you have the required tools (listed above) installed, you can start your actual task.

You should follow the instructions below, and while doing it, try to figure out what is happening (you might be asked about it later :)

## Step 1 - Create an Example Application ##

Create a directory anywhere you like, and change to directory. You can freely choose the name of your directory.

In your newly created directory generate a new Quarkus application:

mvn io.quarkus.platform:quarkus-maven-plugin:3.18.2:create -DprojectGroupId=com.example -DprojectArtifactId=example-app -Dextensions=resteasy

If you want to read more about Quarkus, you can check their web page (https://quarkus.io/), but for now, it is not required.

## Step 2 - Verify the Example Application ##

To run you example application, you must have port 8080 open.

Hint: if you are using the same virtual machine as last week, make sure that GitLab is not running port 8080 - access http://localhost:8080 with web browser. If it is, you can stop it with:

sudo gitlab-ctl stop

And disable automatic startup with:

sudo systemctl disable gitlab-runsvdir.service

After making sure that port 8080 is open, test that your new example application works:

cd example-app && mvn quarkus:dev

Access the application using the web browser first at http://localhost:8080 and then at http://localhost:8080/hello

You can stop the application from terminal with ctrl+c (Remember, SIGTERM!)

## Step 3 - Compile Native Binaries for the Example Application ##

Next, you should compile native binaries for your example application:

mvn package -Pnative

Note that the command must be executed by a user that has access to docker command (if you installed docker with default settings, sudo should work).

## Step 4 - Create Docker Image ##

Create file called "Dockerfile" in the application root directory with content:

```
FROM quay.io/quarkus/quarkus-distroless-image:2.0
WORKDIR /app
COPY target/*-runner /app/app
CMD ["/app/app"]
EXPOSE 8080
```

What do you think these configuration options might do?

After pondering the meaning of the configuration file for a while, create the image using the command:

docker build -t example-app .

Remember to run docker with a user that has access to it. And do NOT forget the period at the end of the command :)

## Step 5 - Run your new Docker Image ##

You can run your new Docker image with:

docker run -p 8080:8080 example-app

This will not detach the process, so you can stop it with ctrl+c.

Test you application with a web browser first at: http://localhost:8080 and then at http://localhost:8080/hello

What did you find out, and why?

## Step 6 - What happened, and why? ##

After testing your application in both paths listed below, consider what you have just achieved.

What did you find out, what happened, and why?

How would the process you have used help you to implement continuous delivery?
