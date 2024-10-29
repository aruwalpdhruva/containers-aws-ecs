# Use the official Ubuntu 20.04 as the base image
FROM ubuntu:20.04

# Set environment variables to non-interactive to avoid prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update the package list and install necessary packages
RUN apt-get update && \
    apt-get install -y software-properties-common wget curl unzip && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.11 python3.11-venv python3.11-dev python3-pip && \
    apt-get clean

# Set Python 3.11 as the default python and pip3
#RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1 && \
#    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

# Install additional tools
RUN apt-get update && \
    apt-get install -y tree vim git && \
    apt-get install -y python3.11

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

# Download and install Terraform
RUN wget https://releases.hashicorp.com/terraform/1.0.11/terraform_1.0.11_linux_amd64.zip && \
    unzip terraform_1.0.11_linux_amd64.zip && \
    mv terraform /usr/local/bin/ && \
    rm terraform_1.0.11_linux_amd64.zip

# Install AWS CLI, IPython, and Boto3
RUN pip install awscli ipython boto3

# Set environment variables for AWS credentials
ENV AWS_ACCESS_KEY_ID=MY_ACCESS_KEY 
ENV AWS_SECRET_ACCESS_KEY=MY_SECRET_KEY 
ENV AWS_DEFAULT_REGION=us-east-1

# Configure AWS CLI with the provided credentials
RUN mkdir -p ~/.aws && \
    echo "[default]" > ~/.aws/credentials && \
    echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >> ~/.aws/credentials && \
    echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >> ~/.aws/credentials && \
    echo "[default]" > ~/.aws/config && \
    echo "region = ${AWS_DEFAULT_REGION}" >> ~/.aws/config

# Set the working directory
WORKDIR /workspace

# Command to run Terraform initialization and apply
CMD ["bash"]
