#FROM golang:1.16.4 AS helm
#RUN git -C / clone --branch release-v3.7.0 --depth=1 https://github.com/rancher/helm
#RUN make -C /helm

FROM alpine:3.12-arm64 AS build
ARG ARCH=arm64
RUN apk add -U curl xz
ENV KUBECTL_VERSION v1.19.7
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/${ARCH}/kubectl && \
    chmod +x kubectl

FROM alpine:3.12-arm64
RUN apk add -U --no-cache bash bash-completion jq
RUN echo '. /etc/profile.d/bash_completion.sh' >> /root/.bashrc && \
    echo 'alias k="kubectl"' >> /root/.bashrc && \
    echo 'alias ks="kubectl -n kube-system"' >> /root/.bashrc && \
    echo 'alias kc="kubectl -n iecas-core"' >> /root/.bashrc && \
    echo 'alias kg="kubectl -n iecas-geovis"' >> /root/.bashrc && \
    echo 'source <(kubectl completion bash)' >> /root/.bashrc && \
    echo 'PS1="> "' >> //root/.bashrc && \
    mkdir /root/.kube && \
COPY --from=build /kubectl /usr/local/bin/
COPY package/welcome /usr/local/bin/
WORKDIR /
CMD ["welcome"]
