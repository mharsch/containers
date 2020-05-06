# Folding@home GPU Container

FROM ubuntu:18.04
LABEL description="Official Folding@home CPU Container"

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      curl \
      ca-certificates \
    && mkdir -p /etc/fahclient && touch /etc/fahclient/config.xml \
    # download and verify checksum
    && curl -fsSL \
      https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-64bit/v7.5/fahclient_7.5.1_amd64.deb \
      -o fah.deb \
    && echo "efe47061d328667259c1815b98bc6c37da339363fb8e9386673f6a96f4d37d26 fah.deb" \
      | sha256sum -c --strict - \
    # install and cleanup
    && DEBIAN_FRONTEND=noninteractive dpkg --install --force-depends fah.deb \
    && rm -rf fah.deb \
    && apt-get purge --autoremove -y curl \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

COPY README.md /

WORKDIR "/fah"
VOLUME ["/fah"]
EXPOSE 7396 36330

ENTRYPOINT ["/usr/bin/FAHClient", "--chdir", "/fah"]
