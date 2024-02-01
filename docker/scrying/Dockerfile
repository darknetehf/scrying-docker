FROM debian:bookworm-slim AS base

ARG UID
ARG GID
ARG USERNAME=anonymous

RUN apt-get update && apt-get upgrade -y && \
	apt-get -y install libwebkit2gtk-4.0-37 xvfb \
	chromium wget \
	git nano vim-tiny

# required by scrying
RUN wget http://ftp.de.debian.org/debian/pool/main/o/openssl/libssl1.1_1.1.1n-0+deb10u3_amd64.deb -O /tmp/libssl1.1_1.1.1n-0+deb10u3_amd64.deb
RUN dpkg -i /tmp/libssl1.1_1.1.1n-0+deb10u3_amd64.deb

# install scrying
RUN wget https://github.com/nccgroup/scrying/releases/download/v0.9.2/scrying_0.9.2_amd64.deb -O /tmp/scrying_0.9.2_amd64.deb
RUN dpkg -i /tmp/scrying_0.9.2_amd64.deb

RUN groupadd --gid $GID $USERNAME \
    && useradd --uid $UID --gid $GID -m $USERNAME

USER $USERNAME

WORKDIR "/home/$USERNAME/"
