FROM ubuntu:16.04

LABEL maintainer="Sylvain Desroziers <sylvain.desroziers@gmail.com>"

RUN apt-get update && \
    apt-get install -y python python-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN pip install keras tensorflow

RUN mkdir /srv/python
COPY ./mnist.py /srv/python/mnist.py

CMD python /srv/python/mnist.py