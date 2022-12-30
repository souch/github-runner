# Pull base image. Put whatever debian-like you want
FROM ubuntu:18.04

ENV RUNNER_WORK_DIRECTORY="_work"

RUN \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y curl git git-lfs man unzip vim wget && \
  apt-get install -y build-essential 
  
# for entrypoint.sh
RUN \  
  apt-get install -y jq supervisor apt-transport-https \
        ca-certificates \
        software-properties-common \
        sudo \
        supervisor \
        jq \
        iputils-ping \
        zlib1g-dev \
        gettext \
        liblttng-ust0 \
        libcurl4-openssl-dev && \
  rm -rf /var/lib/apt/lists/* && \
  apt-get clean
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN chmod 644 /etc/supervisor/conf.d/supervisord.conf

RUN groupadd -r runner && useradd --no-log-init -r -g runner runner
RUN mkdir -p /home/runner
RUN chown -R runner:runner /home/runner
WORKDIR /home/runner
RUN GH_RUNNER_VERSION=${GH_RUNNER_VERSION:-$(curl --silent "https://api.github.com/repos/actions/runner/releases/latest" | grep tag_name | sed -E 's/.*"v([^"]+)".*/\1/')} \
    && curl -L -O https://github.com/actions/runner/releases/download/v${GH_RUNNER_VERSION}/actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
    && tar -zxf actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
    && rm -f actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
    && ./bin/installdependencies.sh \
    && chown -R root: /home/runner \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

ENV RUNNER_ALLOW_RUNASROOT=true

ARG STEP_COPY=true # useful to redo from this step
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
