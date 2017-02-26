FROM nginx:latest

# install consul
ENV VERSION 0.7.2
ENV SHA256 aa97f4e5a552d986b2a36d48fdc3a4a909463e7de5f726f3c5a89b8a1be74a58

RUN mkdir -p /etc/consul.d

RUN apt-get -qq update \
    && apt-get -y install curl unzip ca-certificates \
    && curl -sSL https://releases.hashicorp.com/consul/${VERSION}/consul_${VERSION}_linux_amd64.zip -o /tmp/consul.zip \
    && echo "${SHA256}  /tmp/consul.zip" > /tmp/sha256sum.txt \
    && sha256sum -c /tmp/sha256sum.txt \
    && cd /bin \
    && unzip /tmp/consul.zip \
    && chmod +x /bin/consul \
    && rm /tmp/consul.zip && rm /tmp/sha256sum.txt

ENV CONSUL_TEMPLATE_VERSION 0.18.1

ADD https://releases.hashicorp.com/consul-template/${CONSUL_TEMPLATE_VERSION}/consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip /

RUN unzip consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip && \
    mv consul-template /usr/local/bin/consul-template &&\
    rm -rf /consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip && \
    mkdir -p /etc/consul-template/config.d/templates

ADD ./http_request .
ADD template /etc/consul-template/config.d/templates

CMD ["nginx", "-g", "daemon off;"]
