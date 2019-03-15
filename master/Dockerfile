
# CF https://code-maze.com/ci-jenkins-docker/
# Starting off with the Jenkins base Image
FROM jenkins/jenkins:latest

# Installing the plugins we need using the in-built install-plugins.sh script
RUN /usr/local/bin/install-plugins.sh git matrix-auth workflow-aggregator docker-workflow blueocean credentials-binding

# Install Docker from official repo
USER root
RUN apt-get update -qq && \
    apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    apt-key fingerprint 0EBFCD88 && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && \
    apt-get update -qq && \
    apt-get install -qqy docker-ce && \
    usermod -aG docker jenkins && \
    chown -R jenkins:jenkins $JENKINS_HOME/

USER jenkins

# Setting up environment variables for Jenkins admin user
ENV JENKINS_USER admin
ENV JENKINS_PASS admin

# Skip the initial setup wizard
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false

# Start-up scripts to set number of executors and creating the admin user
COPY executors.groovy /usr/share/jenkins/ref/init.groovy.d/
COPY default-user.groovy /usr/share/jenkins/ref/init.groovy.d/

VOLUME /var/jenkins_home2