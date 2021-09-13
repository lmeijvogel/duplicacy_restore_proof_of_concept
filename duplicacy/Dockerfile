FROM ubuntu:18.04

RUN apt-get update
RUN apt-get -y install wget python python-pip

COPY duplicacy_linux_x64_2.7.2 /usr/bin/duplicacy
RUN chmod +x /usr/bin/duplicacy

WORKDIR /app

RUN mkdir /app/.duplicacy

CMD /usr/bin/duplicacy list
