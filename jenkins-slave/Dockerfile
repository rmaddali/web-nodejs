FROM ubuntu:18.04

# Setup JNLP for jenkins slave
RUN apt-get update && \
    apt-get install -y apt-transport-https curl init openssh-server openssh-client openjdk-8-jdk sudo git-core


RUN curl -sSL https://get.docker.com/ | sh

RUN groupadd -g 10000 jenkins && \
    useradd -d $HOME -u 10000 -g jenkins jenkins && \
    adduser jenkins sudo
RUN usermod -aG docker jenkins
RUN usermod -aG sudo jenkins

RUN curl --create-dirs -sSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/3.40/remoting-3.40.jar && \
    chmod 755 /usr/share/jenkins && \
    chmod 644 /usr/share/jenkins/slave.jar

RUN curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose \
        && chmod +x /usr/local/bin/docker-compose

COPY jenkins-slave /usr/local/bin/jenkins-slave
RUN chmod 777 /usr/local/bin/jenkins-slave && \
    chown jenkins:jenkins /usr/local/bin/jenkins-slave

RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL'| sudo EDITOR='tee -a' visudo



RUN mkdir -p /home/jenkins/.jenkins && \
    mkdir -p /home/jenkins/agent && \
    mkdir -p /home/jenkins/.kube && \
    chown -R jenkins:jenkins /home/jenkins
RUN sudo apt-get update
RUN sudo apt-get install -y python-pip
RUN sudo pip install awscli
RUN sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.4/bin/linux/amd64/kubectl
RUN sudo chmod u+x ./kubectl
RUN sudo mv ./kubectl /usr/local/bin/kubectl
COPY config /home/jenkins/.kube/config
COPY config /root/.kube/config

VOLUME /home/jenkins/.jenkins
VOLUME /home/jenkins/agent

WORKDIR /home/jenkins
USER jenkins

ENTRYPOINT ["/usr/local/bin/jenkins-slave"]