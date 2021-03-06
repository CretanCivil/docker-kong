# a minimal Nginx container including ContainerPilot and a simple virtulhost config
FROM nginx:latest

RUN apt-get update && apt-get install -y curl unzip

# we use consul-template to re-write our Nginx virtualhost config
RUN curl -Lo /tmp/consul_template_0.14.0_linux_amd64.zip https://releases.hashicorp.com/consul-template/0.14.0/consul-template_0.14.0_linux_amd64.zip && \
    unzip /tmp/consul_template_0.14.0_linux_amd64.zip && \
    mv consul-template /bin

# get ContainerPilot release
ENV CONTAINERPILOT_VERSION 2.7.4
RUN export CP_SHA1=073b7bc987f7bf0ca92ae5531d7df6eb02e31bfa \
    && curl -Lo /tmp/containerpilot.tar.gz \
         "https://github.com/joyent/containerpilot/releases/download/${CONTAINERPILOT_VERSION}/containerpilot-${CONTAINERPILOT_VERSION}.tar.gz" \
    && echo "${CP_SHA1}  /tmp/containerpilot.tar.gz" | sha1sum -c \
    && tar zxf /tmp/containerpilot.tar.gz -C /bin \
    && rm /tmp/containerpilot.tar.gz

# add ContainerPilot configuration and onChange handler
COPY containerpilot.json /etc/containerpilot/containerpilot.json
COPY reload-nginx.sh /bin/reload-nginx.sh

# add Nginx virtualhost configuration
COPY nginx.conf /etc/nginx/nginx.conf

# add Nginx virtualhost template that we'll overwrite
COPY nginx.conf.ctmpl /etc/containerpilot/nginx.conf.ctmpl

EXPOSE 8000 8443 8001
