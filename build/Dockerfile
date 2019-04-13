FROM debian:sid-slim

SHELL ["/bin/bash", "-c"]

ENV NODE_VERSION=10.15.3
ENV NPM_VERSION=6.9.0
ENV UPX_VERSION=3.95

WORKDIR /home/nodec/

# installs dependencies required for provisioning and node packer
RUN apt-get update && apt-get install -y --no-install-recommends curl git squashfs-tools python gcc g++ make xz-utils \
 && rm -rf /var/lib/apt/lists/*

# installs upx
RUN curl -sSLko- https://github.com/upx/upx/releases/download/v${UPX_VERSION}/upx-${UPX_VERSION}-amd64_linux.tar.xz | tar xJf - \
  && mv upx-${UPX_VERSION}-amd64_linux/upx /usr/local/bin/ && chmod a+x /usr/local/bin/upx && rm -rf upx-${UPX_VERSION}-amd64_linux

# installs node and npm
RUN curl -sSLko- https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz | tar xfz - \
  && cp -R node-v${NODE_VERSION}-linux-x64/lib/node_modules /usr/local/lib/ \
  && ln -s /usr/local/lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx \
  && ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
  && cp node-v${NODE_VERSION}-linux-x64/bin/node /usr/local/bin \
  && rm -rf node-v${NODE_VERSION}-linux-x64 \
  && chmod a+x /usr/local/bin/npx \
  && chmod a+x /usr/local/bin/npm \
  && chmod a+x /usr/local/bin/node \
  && npm install -g npm@${NPM_VERSION}

# adds node packer and precompiled node sources
ADD nodec /usr/local/bin/nodec
ADD nodec-node-${NODE_VERSION}-precompiled.tar.gz /

# removes dependencies only required for provisioning. note: only works with --squash
RUN apt-get remove --purge -y curl xz-utils && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/bin/bash"]
