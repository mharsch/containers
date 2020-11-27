# Folding@home AMD ROCm Container

FROM ubuntu:18.04
LABEL description="Folding@home 64-bit ARM Container"

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      curl \
      ca-certificates \
    # next line gets past the fahclient.postinst
    && mkdir -p /etc/fahclient && touch /etc/fahclient/config.xml \
    # download and verify checksum
    && curl -fsSL \
      https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-arm64/v7.6/fahclient_7.6.21_arm64.deb \
      -o fah.deb \
    && echo "1dc9fa4ca332783c9cafc90a9cb64d36a5bdfd9a69dbb7887cea326b8e79e965 fah.deb" \
      | sha256sum -c --strict - \
    # install and cleanup
    && DEBIAN_FRONTEND=noninteractive dpkg --install --force-depends fah.deb \
    && rm -rf fah.deb \
    && apt-get purge --autoremove -y curl \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

WORKDIR "/fah"
VOLUME ["/fah"]
EXPOSE 7396 36330

ENTRYPOINT ["/usr/bin/FAHClient", "--chdir", "/fah"]
