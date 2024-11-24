# Use a imagem base do Jenkins
FROM jenkins/jenkins:lts

# Instale dependências necessárias
USER root
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    git \
    mailutils \
    && rm -rf /var/lib/apt/lists/*

# Defina a versão do Go que você deseja instalar
ENV GO_VERSION 1.23.2

# Baixe e instale o Go
RUN wget https://golang.org/dl/go$GO_VERSION.linux-amd64.tar.gz \
    && tar -C /usr/local -xzf go$GO_VERSION.linux-amd64.tar.gz \
    && rm go$GO_VERSION.linux-amd64.tar.gz

# Adicione o Go ao PATH
ENV PATH="/usr/local/go/bin:${PATH}"

# Revertendo de volta para o usuário jenkins
USER jenkins