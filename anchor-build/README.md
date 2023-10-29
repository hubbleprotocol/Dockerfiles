# Anchor Build Dockerfile

## Build and push

```shell
docker build . -t hubbleprotocol/anchor-build:0.29.0 --build-arg SOLANA_CLI=v1.16.18 --build-arg ANCHOR_CLI=v0.29.0

docker push hubbleprotocol/anchor-build:0.29.0
```
