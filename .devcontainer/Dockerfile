# Use the ROCm PyTorch image as the base image
FROM rocm/pytorch:rocm6.1.3_ubuntu22.04_py3.10_pytorch_release-2.1.2

# Set the working directory
WORKDIR /workspace

# Install additional dependencies if required
RUN apt-get update && apt-get install -y \
    vim \
    git \
    && rm -rf /var/lib/apt/lists/*

# Create the .ssh directory
RUN mkdir -p ~/.ssh

# Add GitHub to the known hosts
RUN ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts

# Install Docker CE CLI
RUN apt-get update \
    && apt-get install -y apt-transport-https ca-certificates curl gnupg2 lsb-release \
    && curl -fsSL https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]')/gpg | apt-key add - 2>/dev/null \
    && echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]') $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y docker-ce-cli

# Install Docker Compose
RUN LATEST_COMPOSE_VERSION=$(curl -sSL "https://api.github.com/repos/docker/compose/releases/latest" | grep -o -P '(?<="tag_name": ").+(?=")') \
    && curl -sSL "https://github.com/docker/compose/releases/download/${LATEST_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose


RUN apt-get update \
    && apt-get install -y wget

# Copy the current directory contents into the container at /workspace
COPY . /workspace

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

EXPOSE 8080
EXPOSE 8888

# Run a shell by default
CMD ["bash"]
