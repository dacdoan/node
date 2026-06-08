# Build Nodejs

```sh
curl -L https://nodejs.org/dist/v24.16.0/node-v24.16.0.tar.xz | tar -xJ
mv node-v24.16.0 node

# Clone code
git clone --depth=1 --branch=v24.16.0 https://github.com/nodejs/node.git
cd node
git branch -vvv

# Build
export CFLAGS=-march=x86-64-v3
export CXXFLAGS="$CFLAGS"

docker run --rm -v $PWD:/node -it ubuntu:26.04 /bin/bash
sed -i 's/^Types: deb$/Types: deb deb-src/' /etc/apt/sources.list.d/ubuntu.sources
apt update && apt upgrade
apt install python3 g++ gcc make python3-pip
apt build-dep nodejs

./configure --enable-lto --without-npm --without-amaro --without-corepack --shared-zlib --shared-zstd --shared-sqlite --shared-libuv --shared-brotli --shared-nghttp2 --shared-openssl --openssl-use-def-ca-store --prefix=/usr
  --with-intl=none --without-inspector --without-node-options # For production
  --with-intl=system-icu --shared-ngtcp2 --shared-nghttp3 # For local
make BUILDTYPE=Release -j4

strip -s out/Release/node

cp out/Release/node node
tar --zstd -cf node.tar.zst node
```

## Install
```sh
curl -L https://github.com/dacdoan/node/releases/latest/download/node.tar.zst | tar --zstd -x
```
```sh
curl -L https://github.com/dacdoan/node/releases/latest/download/node-debug.tar.zst | tar --zstd -x
```
