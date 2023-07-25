# syntax=docker/dockerfile:1
FROM nvidia/cuda:11.2.2-cudnn8-runtime-ubuntu20.04

ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"

# Update the APT-GET
RUN apt-get update --fix-missing
RUN apt-get install apt-transport-https
RUN apt-get install ca-certificates
RUN update-ca-certificates

RUN apt-get install -y wget && \
  rm -rf /var/lib/apt/lists/*

# necessary for java-bridge
RUN apt-get update && \
  apt-get install -y openjdk-8-jdk 

# Install Miniconda
RUN wget \
  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
  && mkdir /root/.conda \
  && bash Miniconda3-latest-Linux-x86_64.sh -b \
  && rm -f Miniconda3-latest-Linux-x86_64.sh 

# create conda environment
RUN conda create -n {{cookiecutter.project_name}} python={{cookiecutter.python_version}} -y

# activate conda environment
RUN conda init bash
RUN echo "conda activate {{cookiecutter.project_name}}" >> ~/.bashrc

USER root

# Install required packages
WORKDIR /setup
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

WORKDIR /workspace/{{cookiecutter.project_name}}

