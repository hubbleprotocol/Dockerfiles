# Anchor Build Dockerfile

## Build and push

```shell
docker build . -t hubbleprotocol/anchor-build:0.27.0 --build-arg SOLANA_CLI=v1.14.19 --build-arg ANCHOR_CLI=v0.27.0

docker push hubbleprotocol/anchor-build:0.27.0
```
