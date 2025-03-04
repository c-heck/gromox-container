FROM opensuse/leap:15.5

ARG GROMMUNIO_REPO=openSUSE_Leap_15.5

ARG S6_OVERLAY_VERSION=3.1.5.0

# Install Grommunio packages
RUN zypper install -y curl && \
  curl https://download.grommunio.com/RPM-GPG-KEY-grommunio > gr.key && \
  rpm --import gr.key && \
  zypper --non-interactive --quiet ar -C https://download.grommunio.com/community/${GROMMUNIO_REPO} grommunio && \
  zypper --gpg-auto-import-keys ref && \
  zypper -n refresh grommunio && \
  zypper in -y grommunio-common mariadb nginx nginx-module-vts nginx-module-brotli nginx-module-zstd \
		 grommunio-admin-api grommunio-admin-web gromox vim xz tar wget && \
  zypper clean --all

# Setup S6
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-noarch.tar.xz
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-x86_64.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-x86_64.tar.xz
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-symlinks-noarch.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-symlinks-noarch.tar.xz
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-symlinks-arch.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-symlinks-arch.tar.xz

# Setup services in s6
COPY build-assets/s6-overlay/ /etc/s6-overlay/

ENTRYPOINT ["/init"]
