# Build Nodejs

```sh
curl -L https://nodejs.org/dist/v24.16.0/node-v24.16.0-linux-x64.tar.xz | tar -xJ
mv node-v24.16.0-linux-x64 node

# Clone code
git clone --depth=1 --branch=v24.16.0 https://github.com/nodejs/node.git
cd node
git branch -vvv

# Build
export CFLAGS=-march=x86-64-v3
export CXXFLAGS="$CFLAGS"

./configure --enable-lto --without-npm --without-amaro --without-corepack --with-intl=none
  --without-inspector --without-node-options --without-sqlite # For production
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
