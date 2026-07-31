FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && apt-get install -y \
    openjdk-17-jdk \
    git \
    curl \
    wget \
    unzip \
    maven \
    openssh-server \
    docker.io \
    sudo \
 && rm -rf /var/lib/apt/lists/*

# Create SSH runtime directory
RUN mkdir /var/run/sshd

# Create Jenkins user
RUN useradd -m -s /bin/bash jenkins

# Add Jenkins user to sudo and docker groups
RUN usermod -aG sudo,docker jenkins

# # Create SSH directory
# RUN mkdir -p /home/jenkins/.ssh

# # Copy Jenkins public key
# COPY jenkins-agent.pub /home/jenkins/.ssh/authorized_keys

# # Correct permissions
# RUN chown -R jenkins:jenkins /home/jenkins/.ssh && \
#     chmod 700 /home/jenkins/.ssh && \
#     chmod 600 /home/jenkins/.ssh/authorized_keys

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
