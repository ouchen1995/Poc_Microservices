
# Proof of Concept Java Microservice

## What's a Microservice architecture 
An architectural style that structures an application as a collection of loosely coupled services, which implement business capabilities.
 ### Enables
 the continuous delivery/deployment of large.
 complex applications. 
 enables an organization to evolve its technology stack.
Services communicate using either synchronous protocols such as HTTP/REST or asynchronous protocols such as AMQP. 
Services can be developed and deployed independently of one another. 
Each service has its own database in order to be decoupled from other services. Data consistency between services is maintained using the Saga pattern
## The Scale Cube

            
X-axis scaling consists of running multiple copies of an application behind a load balancer. If there are N copies then each copy handles 1/N of the load. This is a simple, commonly used approach of scaling an application.
Y-axis scaling
Unlike X-axis and Z-axis, which consist of running multiple, identical copies of the application, Y-axis axis scaling splits the application into multiple, different services. Each service is responsible for one or more closely related functions.
Z-axis scaling
When using Z-axis scaling each server runs an identical copy of the code. In this respect, it’s similar to X-axis scaling. The big difference is that each server is responsible for only a subset of the data.

## Pattern: Saga
Each service has its own database. Some business transactions, however, span multiple service so you need a mechanism to ensure data consistency across services. 
Implement each business transaction that spans multiple services as a saga.
A saga is a sequence of local transactions. 
Each local transaction updates the database and publishes a message or event to trigger the next local transaction in the saga.
If a local transaction fails because it violates a business rule then the saga executes a series of compensating transactions that undo the changes that were made by the preceding local transactions.
Choreography - each local transaction publishes domain events that trigger local transactions in other services
Orchestration - an orchestrator (object) tells the participants what local transactions to execute

## Pattern: API Composition
  it is no longer straightforward to implement queries that join data from multiple services.    
Implement a query by defining an API Composer, which invoking the services that own the data and performs an in-memory join of the results.
 
 
## Pattern: API Gateway / Backend for Front-End

# Prerequisite 

Instal java 8 
First, update the package index.
sudo apt-get update


Next, install Java. Specifically, this command will install the Java Runtime Environment (JRE).
sudo apt-get install default-jre


There is another default Java installation called the JDK (Java Development Kit). The JDK is usually only needed if you are going to compile Java programs or if the software that will use Java specifically requires it.
The JDK does contain the JRE, so there are no disadvantages if you install the JDK instead of the JRE, except for the larger file size.
You can install the JDK with the following command:
sudo apt-get install default-jdk


Installing the Oracle JDK
If you want to install the Oracle JDK, which is the official version distributed by Oracle, you will need to follow a few more steps.
First, add Oracle's PPA, then update your package repository.
sudo add-apt-repository ppa:webupd8team/java


sudo apt-get update


Then, depending on the version you want to install, execute one of the following commands:
Oracle JDK 8
This is the latest stable version of Java at time of writing, and the recommended version to install. You can do so using the following command:
sudo apt-get install oracle-java8-installer

Instal docker 
The Docker installation package available in the official Ubuntu 16.04 repository may not be the latest version. To get the latest and greatest version, install Docker from the official Docker repository. This section shows you how to do just that.
First, add the GPG key for the official Docker repository to the system:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


Add the Docker repository to APT sources:
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"


Next, update the package database with the Docker packages from the newly added repo:
sudo apt-get update


Make sure you are about to install from the Docker repo instead of the default Ubuntu 16.04 repo:
apt-cache policy docker-ce


You should see output similar to the follow:
Output of apt-cache policy docker-ce
docker-ce:
  Installed: (none)
  Candidate: 17.03.1~ce-0~ubuntu-xenial
  Version table:
     17.03.1~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     17.03.0~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages



Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 16.04. The docker-ce version number might be different.
Finally, install Docker:
sudo apt-get install -y docker-ce


Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running:
sudo systemctl status docker


The output should be similar to the following, showing that the service is active and running:
Output
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2016-05-01 06:53:52 CDT; 1 weeks 3 days ago
     Docs: https://docs.docker.com
 Main PID: 749 (docker)


Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. We'll explore how to use the docker command later in this tutorial.
Install docker-compose
Although we can install Docker Compose from the official Ubuntu repositories, it is several minor version behind the latest release, so we'll install Docker Compose from the Docker's GitHub repository. The command below is slightly different than the one you'll find on the Releases page. By using the -o flag to specify the output file first rather than redirecting the output, this syntax avoids running into a permission denied error caused when using sudo.
We'll check the current release and if necessary, update it in the command below:
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose


Next we'll set the permissions:
sudo chmod +x /usr/local/bin/docker-compose


Then we'll verify that the installation was successful by checking the version:
docker-compose --version


This will print out the version we installed:
Output
docker-compose version 1.18.0, build 8dd22a9


Now that we have Docker Compose installed, we're ready to run a "Hello World" example
Install git
By far the easiest way of getting git installed and ready to use is by using Ubuntu's default repositories. This is the fastest method, but the version may be older than the newest version. If you need the latest release, consider following the steps to compile git from source.
You can use the apt package management tools to update your local package index. Afterwards, you can download and install the program:
sudo apt-get update


sudo apt-get install git


This will download and install git to your system. You will still have to complete the configuration steps that we cover in the "setup" section, so feel free to skip to that section now.
Piggy Metrics
 

Clone from git 
Git clone https://github.com/vaquarkhan/PiggyMetrics-microservice-poc.git

Export system variable 
export CONFIG_SERVICE_PASSWORD=password
export NOTIFICATION_SERVICE_PASSWORD=password
export STATISTICS_SERVICE_PASSWORD=password
export ACCOUNT_SERVICE_PASSWORD=password
export MONGODB_PASSWORD=password
docker-compose 
Docker-compose up


CI/CD
Why 
How 




