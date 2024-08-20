FROM alpine:3.18

# Set environment variables
ENV SONAR_SCANNER_VERSION=4.8.0.2856

# Install required packages
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

# Install Snyk
RUN npm install -g snyk

# Install Sonar Scanner
RUN curl -fsSL https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip -o sonar-scanner.zip \
    && mkdir -p /opt/sonar-scanner \
    && unzip sonar-scanner.zip -d /opt/sonar-scanner/ \
    && ln -s /opt/sonar-scanner/sonar-scanner-${SONAR_SCANNER_VERSION}-linux/bin/sonar-scanner /usr/local/bin/sonar-scanner \
    && rm sonar-scanner.zip

# Install Trivy
RUN curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin

# Set Java Home (for Sonar Scanner)
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk
ENV PATH=$JAVA_HOME/bin:$PATH

# Verify installations
RUN sonar-scanner --version && snyk --version && trivy --version

# Set the entrypoint to bash
ENTRYPOINT ["/bin/bash"]
