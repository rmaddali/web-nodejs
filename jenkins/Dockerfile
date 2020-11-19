FROM jenkins/jenkins:lts

USER root
RUN apt-get update
RUN apt-get install -y sudo
RUN rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN sudo apt-get update && sudo apt-get install -y maven
RUN /usr/local/bin/install-plugins.sh  < /usr/share/jenkins/ref/plugins.txt
RUN sudo apt-get update
RUN sudo apt-get install -y python-pip
RUN sudo pip install awscli
RUN sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.4/bin/linux/amd64/kubectl
RUN sudo chmod u+x ./kubectl
RUN sudo mv ./kubectl /usr/local/bin/kubectl
COPY config /root/.kube/config
RUN sudo curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN sudo apt-get install -y nodejs
RUN /usr/local/bin/install-plugins.sh ssh-slaves
USER root
RUN apt-get update
RUN groupadd docker && gpasswd -a jenkins docker
USER jenkins