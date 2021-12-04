FROM jenkins/jenkins:lts

USER root
RUN apt-get -qq update \
   && apt-get -qq -y install \
   curl

RUN curl -sSL https://get.docker.com/ | sh

#RUN chmod 777 /var/run/docker.sock

RUN usermod -a -G staff jenkins

USER jenkins
#USER root
