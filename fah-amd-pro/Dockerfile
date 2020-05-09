# Folding@home GPU Container for AMDGPU-PRO stack

FROM ubuntu:18.04
LABEL description="Folding@home GPU Container for AMDGPU-PRO stack"

#ADD amdgpu-pro-20.10-1048554-ubuntu-18.04.tar.xz .
ENV DEBIAN_FRONTEND=noninteractive \
    DRIVER_STRING=amdgpu-pro-20.10-1048554-ubuntu-18.04 \
    DRIVER_FILE=$DRIVER_STRING.tar.xz

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      curl \
      ca-certificates \
      xz-utils \
    && curl -fsSL https://drivers.amd.com/drivers/linux/amdgpu-pro-20.10-1048554-ubuntu-18.04.tar.xz \
       -e https://drivers.amd.com -o amdgpu-pro.tar.xz \
    && tar -Jxvf amdgpu-pro.tar.xz \
    && ./$DRIVER_STRING/amdgpu-pro-install -y --no-dkms --headless --opencl=pal,legacy \
    && rm -rf  $DRIVER_STRING \
    && mkdir -p /etc/fahclient && touch /etc/fahclient/config.xml \
    && curl -fsSL \
      https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-64bit/v7.6/fahclient_7.6.13_amd64.deb \
      -o fah.deb \
    && printf "113cf292f7cc3a49ed718f0c44849e30018e799fa9f1eacd75716badbcb64cc8 fah.deb" | sha256sum -c --strict - \
    && dpkg --install --force-depends fah.deb \
    && rm -rf fah.deb \
    && apt-get purge --autoremove -y curl \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

COPY README.md /

WORKDIR "/fah"
VOLUME ["/fah"]
EXPOSE 7396 36330

ENTRYPOINT ["/usr/bin/FAHClient", "--chdir", "/fah"]
