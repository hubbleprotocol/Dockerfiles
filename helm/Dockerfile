FROM ubuntu:22.10 as helm-base

RUN apt-get update \
	&& apt-get install -y apt-utils apt-transport-https ca-certificates gnupg2 wget curl git gettext jq unzip

RUN wget $(curl -s https://api.github.com/repos/mikefarah/yq/releases/latest | grep browser_download_url | grep -m 1 linux_amd64 | cut -d '"' -f 4) -O /usr/bin/yq \
	&& chmod +x /usr/bin/yq

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
	&& unzip awscliv2.zip \
  && rm awscliv2.zip \
	&& ./aws/install

RUN curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg \
	&& echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list \
	&& apt-get update \
	&& apt-get install -y kubectl

RUN curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | tee /usr/share/keyrings/helm.gpg > /dev/null \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | tee /etc/apt/sources.list.d/helm-stable-debian.list \
	&& apt-get update \
	&& apt-get install -y helm \
	&& helm plugin install https://github.com/hypnoglow/helm-s3.git

RUN kubectl version --client \
	&& helm version
