FROM amazonlinux

# Install AWS CLI, yum-utils, shadow-utils, git, Docker, Packer, Terraform, Maven, and MySQL
RUN yum install -y aws-cli yum-utils shadow-utils git docker \
    && yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo \
    && yum install -y packer terraform \
    && yum install -y maven \
    && systemctl enable docker \
    && systemctl start docker

# Install SSM Agent
RUN yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm

# Install CloudWatch Agent
RUN yum install -y https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm \
    && /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a start

# Install AWS Inspector
RUN curl -O https://inspector-agent.amazonaws.com/linux/latest/install \
    && bash install
