# Anchor Build Dockerfile

## Build and push

```shell
docker buildx build . -t hubbleprotocol/anchor-build:0.29.0 --build-arg SOLANA_CLI="1.17.20" --build-arg ANCHOR_CLI="0.29.0" --build-arg RUST_VERSION="1.73.0"

docker push hubbleprotocol/anchor-build:0.29.0
```
