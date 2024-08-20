# Use Alpine as the base image
FROM alpine:latest

# Set environment variables
ENV DOCKER_VERSION=20.10.8
ENV MAVEN_VERSION=3.9.4
ENV SONAR_SCANNER_VERSION=4.8.0.2856
ENV JFROG_CLI_VERSION=2.47.0
ENV TRIVY_VERSION=0.54.1

# Install necessary packages
RUN apk add --no-cache \
    bash \
    curl \
    openjdk11 \
    git \
    docker \
    nodejs \
    npm \
    ca-certificates \
    openssl \
    && update-ca-certificates

# Install Docker CLI
RUN curl -fsSL https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}.tgz | tar -xzC /usr/local/bin --strip=1 docker/docker

# Install Maven
RUN curl -fsSL https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz | tar -xzC /opt \
    && ln -s /opt/apache-maven-${MAVEN_VERSION}/bin/mvn /usr/local/bin/mvn

# Install Sonar Scanner
RUN curl -fsSL https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip -o sonar-scanner.zip \
    && unzip sonar-scanner.zip -d /opt \
    && ln -s /opt/sonar-scanner-${SONAR_SCANNER_VERSION}-linux/bin/sonar-scanner /usr/local/bin/sonar-scanner \
    && rm sonar-scanner.zip

# Install JFrog CLI
RUN curl -fsSL https://releases.jfrog.io/artifactory/jfrog-cli/v2/${JFROG_CLI_VERSION}/jfrog-cli-linux-amd64/jfrog -o /usr/local/bin/jfrog \
    && chmod +x /usr/local/bin/jfrog

# Install Trivy
RUN curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin \
    && trivy --version

# Verify installations
RUN docker --version \
    && mvn -v \
    && sonar-scanner --version \
    && jfrog -v \
    && trivy --version

# Set the entrypoint
ENTRYPOINT ["/bin/bash"]
