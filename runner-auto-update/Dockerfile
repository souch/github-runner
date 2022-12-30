
# Pull base image.
FROM ubuntu:18.04

ENV RUNNER_WORK_DIRECTORY="_work"

# Install.
RUN \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y curl git git-lfs man unzip vim wget && \
  apt-get install -y build-essential 
# 

# RUN \
#   apt-get install -y 'g\+\+-8'
  
# eesep  
# RUN \
#   DEBIAN_FRONTEND=noninteractive apt-get install -y libbz2-dev libgl1-mesa-dev mesa-common-dev libomp-dev gfortran yasm pkg-config libtool nasm automake
# 
# RUN \  
#   DEBIAN_FRONTEND=noninteractive apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev ffmpeg libavcodec-dev libavformat-dev libavdevice-dev libv4l-dev libgtk2.0-dev ocl-icd-* opencl-headers gstreamer1.0-plugins-bad ubuntu-restricted-extras libdvd-pkg libgmp-dev libmpfr-dev python3-pip
  
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

# RUN \
#   echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections && \
#   echo libdvd-pkg libdvd-pkg/post-invoke_hook-install select true | debconf-set-selections && \
#   echo libdvd-pkg libdvd-pkg/build select true | debconf-set-selections && \
#   echo libdvd-pkg libdvd-pkg/post-invoke_hook-remove select false | debconf-set-selections && \
#   DEBIAN_FRONTEND=noninteractive apt-get install -y libbz2-dev libgl1-mesa-dev mesa-common-dev libomp-dev gfortran yasm pkg-config libtool nasm automake libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev ffmpeg libavcodec-dev libavformat-dev libavdevice-dev libv4l-dev libgtk2.0-dev ocl-icd-* opencl-headers gstreamer1.0-plugins-bad ubuntu-restricted-extras libdvd-pkg libgmp-dev libmpfr-dev python3-pip

# Add files.
# ADD root/.bashrc /root/.bashrc
# ADD root/.gitconfig /root/.gitconfig
# ADD root/.scripts /root/.scripts

# Set environment variables.

# from https://github.com/tcardonne/docker-github-runner/blob/master/docker/Dockerfile

#RUN useradd -m -d /home/runner -s /bin/bash runner
RUN groupadd -r runner && useradd --no-log-init -r -g runner runner
#RUN useradd -s /bin/bash -m -d /home/runner -G sudo runner
RUN mkdir -p /home/runner
RUN chown -R runner:runner /home/runner
WORKDIR /home/runner
#USER runner
RUN GH_RUNNER_VERSION=${GH_RUNNER_VERSION:-$(curl --silent "https://api.github.com/repos/actions/runner/releases/latest" | grep tag_name | sed -E 's/.*"v([^"]+)".*/\1/')} \
    && curl -L -O https://github.com/actions/runner/releases/download/v${GH_RUNNER_VERSION}/actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
    && tar -zxf actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
    && rm -f actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
    && ./bin/installdependencies.sh \
    && chown -R root: /home/runner \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# RUN GH_RUNNER_VERSION=${GH_RUNNER_VERSION:-$(curl --silent "https://api.github.com/repos/actions/runner/releases/latest" | grep tag_name | sed -E 's/.*"v([^"]+)".*/\1/')} \
#     && curl -L -O https://github.com/actions/runner/releases/download/v${GH_RUNNER_VERSION}/actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
#     && tar -zxf actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
#     && rm -f actions-runner-linux-x64-${GH_RUNNER_VERSION}.tar.gz \
#     && chown -R runner: /home/runner \
#     && rm -rf /var/lib/apt/lists/* \
#     && apt-get clean    

ENV RUNNER_ALLOW_RUNASROOT=true
#ENV RUNNER_ALLOW_RUNASROOT 1
#export RUNNER_ALLOW_RUNASROOT="1" ; 
# RUN \  
#     ./config.sh --url https://github.com/souch/test_CI --token AA72OF7LH7CK2XNFQ2D34JLDVVVA6 
      
#CMD ["/home/runner/run.sh"]
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
