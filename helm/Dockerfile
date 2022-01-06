FROM ubuntu:22.04 as helm-base

RUN apt-get update \
	&& apt-get install -y apt-utils apt-transport-https ca-certificates gnupg2 wget curl git gettext jq

RUN wget $(curl -s https://api.github.com/repos/mikefarah/yq/releases/latest | grep browser_download_url | grep -m 1 linux_amd64 | cut -d '"' -f 4) -O /usr/bin/yq \
	&& chmod +x /usr/bin/yq

RUN curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg \
	&& echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list \
	&& apt-get update \
	&& apt-get install -y kubectl

RUN curl https://baltocdn.com/helm/signing.asc | apt-key add - \
	&& echo "deb https://baltocdn.com/helm/stable/debian/ all main" | tee /etc/apt/sources.list.d/helm-stable-debian.list \
	&& apt-get update \
	&& apt-get install -y helm \
	&& helm plugin install https://github.com/hypnoglow/helm-s3.git

RUN kubectl version --client \
	&& helm version \
