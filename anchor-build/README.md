# Anchor Build Dockerfile

### Build and push

```shell
docker build . -t hubbleprotocol/anchor-build:0.25.0 --build-arg SOLANA_CLI=v1.10.29 --build-arg ANCHOR_CLI=v0.25.0

docker push hubbleprotocol/anchor-build:0.25.0
```
