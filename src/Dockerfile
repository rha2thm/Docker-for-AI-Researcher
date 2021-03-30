FROM ufoym/deepo:cpu

#Tini
ENV DEBIAN_FRONTEND=noninteractive \
    TINI_VERSION=0.17.0 \
    TINI_SHA512SUM=772a4bfaefcb4293a31c1fbfe83a0f61e028a79aff6ef56cc33c061025689dd8ae071db5e417f466bc0b415da0c3aa2e6613e2c610128617e71d2fec9ee6f831 \
    BUILD_HOME=/tmp/home \
    BUILD_UID=1000 \
    BUILD_GID=1000

ADD https://github.com/krallin/tini/releases/download/v$TINI_VERSION/tini-amd64 /usr/local/bin/tini
RUN echo "$TINI_SHA512SUM  /usr/local/bin/tini" | sha512sum -c \
    && chmod +x /usr/local/bin/tini

## install VSCODE dependancies
RUN apt-get update && apt-get install -y \
	sudo \
    nano \
	git \
	git-lfs \
	zsh \
	dumb-init \
	openssh-server \
	cron \
    tree
	
COPY requirements.txt /tmp
RUN pip install --upgrade pip
RUN pip install -r /tmp/requirements.txt

# RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
# RUN apt-get install -y nodejs

RUN curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
RUN chsh -s `which zsh`

#Cleanup
RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

#VSCODE
# WORKDIR /
RUN curl -fsSL https://code-server.dev/install.sh | sh
RUN mkdir -p /root/.config/code-server
RUN echo """bind-addr: 0.0.0.0:8080 \
auth: password \
password: root \
cert: false""" >> /root/.config/code-server/config.yaml

########################################################
# Thanks to https://github.com/touchifyapp/docker-ssh
# # Replace ssh_config
COPY sshd/* /usr/local/etc/sshd/
COPY sshd/sshd_config /etc/ssh/sshd_config

# set password
RUN echo 'root:root' |chpasswd
RUN service ssh start

EXPOSE 22
EXPOSE 8080
EXPOSE 6006
EXPOSE 8000
WORKDIR /root
# RUN systemctl enable code-server
# RUN nohup bash -c "code-server --bind-addr 0.0.0.0:8080 &"

ENTRYPOINT ["/usr/sbin/sshd","-D"]