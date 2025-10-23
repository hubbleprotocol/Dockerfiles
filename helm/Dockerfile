FROM ubuntu:24.04 AS helm-base

ARG KUBECTL_VERSION="v1.34"

RUN apt-get update \
    && apt-get install -y \
      apt-transport-https \
      ca-certificates \
      curl \
      gettext \
      git \
      gnupg \
      jq \
      pip \
      unzip \
      virtualenv \
      wget \
      yq

# Install jinja2-cli in a virtual environment
RUN virtualenv /venv \
    && . /venv/bin/activate \
    && pip install jinja2-cli \
    && jinja2 --version

RUN echo "source /venv/bin/activate" >> /etc/profile

# Install kubectl
RUN curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.34/deb/Release.key \
    | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/${KUBECTL_VERSION}/deb/ /" \
    | tee /etc/apt/sources.list.d/kubernetes.list \
    # helps tools such as command-not-found to work correctly
    && chmod 644 /etc/apt/sources.list.d/kubernetes.list \
    && chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg \
    && apt-get update && apt-get install kubectl -y

# Install AWS CLI v2 with GPG verification
COPY ./utils/aws-cli-gpg-key.pub /tmp/aws-cli-gpg-key.pub
RUN gpg --import /tmp/aws-cli-gpg-key.pub \
    && rm /tmp/aws-cli-gpg-key.pub \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && curl -o awscliv2.sig https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip.sig \
    && gpg --verify awscliv2.sig awscliv2.zip \
    && unzip awscliv2.zip \
    && rm awscliv2.zip awscliv2.sig \
    && ./aws/install

# Install Helm
RUN curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey \
    | gpg --dearmor \
    | tee /usr/share/keyrings/helm.gpg > /dev/null \
    && echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" \
    | tee /etc/apt/sources.list.d/helm-stable-debian.list \
    && apt-get update \
    && apt-get install helm \
    && helm plugin install https://github.com/hypnoglow/helm-s3.git

RUN apt-get clean

RUN kubectl version --client && aws --version && helm version

# This allows git to be used in any mounted volume without safe directory errors
RUN git config --global --add safe.directory '*'

COPY ./utils/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash"]
