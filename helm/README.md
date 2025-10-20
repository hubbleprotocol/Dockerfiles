# Helm Dockerfile

Image contains all dependencies needed to publish Helm charts to a Helm repository.

It also contains various utilities to help with chart management, including:
- `git` - for handling mounted git repositories
- `helm s3 plugin` - for publishing charts to S3-backed Helm repositories
- `jinja2` - templating engine for generating Helm chart values files
- `yq` - command-line YAML processor

### Build and push

```shell
docker build . -t hubbleprotocol/helm:0.0.4

docker push hubbleprotocol/helm:0.0.4
```
