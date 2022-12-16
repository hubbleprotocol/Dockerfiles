# Anchor Build Dockerfile

## Build and push

```shell
docker build . -t hubbleprotocol/anchor-build:0.26.0 --build-arg SOLANA_CLI=v1.13.5 --build-arg ANCHOR_CLI=v0.26.0

docker push hubbleprotocol/anchor-build:0.26.0
```
