#FROM golang:1.16.4 AS helm
#RUN git -C / clone --branch release-v3.7.0 --depth=1 https://github.com/rancher/helm
#RUN make -C /helm

FROM alpine:3.12-arm64 AS build
ARG ARCH=aarch64
RUN apk add -U curl xz
ENV KUBECTL_VERSION v1.19.7
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/${ARCH}/kubectl && \
    chmod +x kubectl

FROM alpine:3.12-arm64
RUN apk add -U --no-cache bash bash-completion jq
RUN echo 'shell:x:1000:1000:shell,,,:/home/shell:/bin/bash' > /etc/passwd && \
    echo 'shell:x:1000:' > /etc/group && \
    mkdir /home/shell && \
    echo '. /etc/profile.d/bash_completion.sh' >> /home/shell/.bashrc && \
    echo 'alias k="kubectl"' >> /home/shell/.bashrc && \
    echo 'alias ks="kubectl -n kube-system"' >> /home/shell/.bashrc && \
    echo 'alias kc="kubectl -n iecas-core"' >> /home/shell/.bashrc && \
    echo 'alias kg="kubectl -n iecas-geovis"' >> /home/shell/.bashrc && \
    echo 'source <(kubectl completion bash)' >> /home/shell/.bashrc && \
    echo 'PS1="> "' >> /home/shell/.bashrc && \
    mkdir /home/shell/.kube && \
    chown -R shell /home/shell && \
    chmod 700 /run
COPY --from=build /kubectl /usr/local/bin/
COPY package/welcome /usr/local/bin/
USER 1000
WORKDIR /home/shell
CMD ["welcome"]
