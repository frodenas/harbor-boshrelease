# Harbor BOSH Release

This is a [BOSH](http://bosh.io/) release for [VMware Harbor](https://vmware.github.io/harbor/), an enterprise-class registry server that stores and distributes Docker images.

## Disclaimer

This is **NOT** a production ready [BOSH](http://bosh.io/) release. I created it for my own experimentation and it may not be supported in the future.

If you are looking for a supported [Docker Registry](https://docs.docker.com/registry/) [BOSH](http://bosh.io/) release, please check the [bosh.io](http://bosh.io/releases) releases page or the [cloudfoundry-community](https://github.com/cloudfoundry-community) github organization.

## Table of Contents

* [Usage](https://github.com/frodenas/harbor-boshrelease#usage)
  * [Requirements](https://github.com/frodenas/harbor-boshrelease#requirements)
  * [Clone the repository](https://github.com/frodenas/harbor-boshrelease#clone-the-repository)
  * [Basic deployment](https://github.com/frodenas/harbor-boshrelease#basic-deployment)
  * [Operations files](https://github.com/frodenas/harbor-boshrelease#operations-files)
  * [Deployment variables and the var-store](https://github.com/frodenas/harbor-boshrelease#deployment-variables-and-the-var-store)
* [Contributing](https://github.com/frodenas/harbor-boshrelease#contributing)
* [License](https://github.com/frodenas/harbor-boshrelease#license)

## Usage

### Requirements

In order to use this BOSH release you will need:

* [BOSH CLI v2](https://bosh.io/docs/cli-v2.html)
* An already deployed [BOSH environment](http://bosh.io/docs/init.html)
* A compatible [cloud-config](http://bosh.io/docs/terminology.html#cloud-config) with a `default` option for `network` and `vm_types` (you can use the example that comes from [cf-deployment](https://github.com/cloudfoundry/cf-deployment/blob/master/bosh-lite/cloud-config.yml))

###  Clone the repository

First, clone this repository into your workspace:

```
git clone https://github.com/frodenas/harbor-boshrelease
cd harbor-boshrelease
export BOSH_ENVIRONMENT=<name>
```

### Basic deployment

To deploy a basic `harbor` server use the following command:

```
bosh -d harbor deploy manifests/harbor.yml \
  --vars-store tmp/deployment-vars.yml \
  -v harbor_secret_key=123456789012345678901234
```

*NOTE: The `harbor_secret_key` variable MUST be a 16, 24, or 32 bytes AES key*

### Operations files

Additional [operations files](http://bosh.io/docs/cli-ops-files.html) are located at the [manifests/operators](https://github.com/frodenas/harbor-boshrelease/tree/master/manifests/operators) directory. Those files includes a basic configuration, so extra ops files might be needed for additional configuration.

Please review the op files before deploying them to check the requeriments, dependencies and necessary variables.

| File | Description |
| ---- | ----------- |
| [enable-cf-route-registrar.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-cf-route-registrar.yml) | Registers `harbor` as a [Cloud Foundry route](https://docs.cloudfoundry.org/devguide/deploy-apps/routes-domains.html) (under your `system domain`) |
| [enable-redis-cache.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-redis-cache.yml) | Enables [nginx](https://nginx.org/) as a proxy in front of your [Docker Registry](https://docs.docker.com/registry/) instances |
| [enable-registry-azure.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-registry-azure.yml) | Uses [Microsoft Azure Storage](https://azure.microsoft.com/en-us/services/storage/) as the [Docker Registry storage backend](https://docs.docker.com/registry/configuration/#storage) |
| [enable-registry-gcs.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-registry-gcs.yml) | Uses [Google Cloud Storage](https://cloud.google.com/storage/) as the [Docker Registry storage backend](https://docs.docker.com/registry/configuration/#storage) |
| [enable-registry-oss.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-registry-oss.yml) | Uses [Aliyun Object Storage Service](https://www.alibabacloud.com/product/oss) as the [Docker Registry storage backend](https://docs.docker.com/registry/configuration/#storage) |
| [enable-registry-s3.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-registry-s3.yml) | Uses [Amazon S3](https://cloud.google.com/storage/) as the [Docker Registry storage backend](https://docs.docker.com/registry/configuration/#storage) |
| [enable-registry-swift.yml](https://github.com/frodenas/harbor-boshrelease/blob/master/manifests/operators/enable-registry-swift.yml) | Uses [OpenStack Swift](https://docs.openstack.org/swift/latest/) as the [Docker Registry storage backend](https://docs.docker.com/registry/configuration/#storage) |

### Deployment variables and the var-store

Some operators files requires additional information to provide environment-specific or sensitive configuration such as various credentials. To do this in the default configuration, we use the `--vars-store`. This flag takes the name of a `yml` file that it will read and write to. Where necessary credential values are not present, it will generate new values based on the type information stored at the different deployment files. Necessary variables that BOSH can't generate need to be supplied as well.
See each particular op files you're using for any additional necessary variables.

See also the [BOSH CLI documentation](http://bosh.io/docs/cli-int.html#value-sources) for more information about ways to supply such additional variables.

## Contributing

Refer to [CONTRIBUTING.md](https://github.com/frodenas/harbor-boshrelease/blob/master/CONTRIBUTING.md).

## License

Apache License 2.0, see [LICENSE](https://github.com/frodenas/harbor-boshrelease/blob/master/LICENSE).
