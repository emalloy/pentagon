# Documentation

## Getting Started

### System Requirements

This repository relies on system tools and Python libraries for your system.

System Tools:
* [Terraform](https://www.terraform.io) version 0.8.8
* [kops](https://github.com/kubernetes/kops) version 1.5.3
* [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/) version 1.5.x

kubectl should try to match the cluster version, in this case the 1.5.x series for Kubernetes.

Python Libraries:

Libraries required can be installed with `pip install -r requirements.txt`. These can be installed into a [virtualenv](https://virtualenv.pypa.io/en/stable/) to isolate the installation from your system.

### Shell environment

Shell variables are used extensively for configuration of the above tools. Where possible these are checked into the repository. Secrets/credentials must be obtained separately.

Some shell variables are stored as YAML and require [shyaml](https://github.com/0k/shyaml) installed as a Python requirement above.

First create the `config/private/secrets.yml` file and supply values:

```
AWS_ACCESS_KEY:
AWS_ACCESS_KEY_ID:
TF_VAR_aws_access_key:
AWS_SECRET_KEY:
AWS_SECRET_ACCESS_KEY:
TF_VAR_aws_secret_key:
```

Set `INFRASTRUCTURE_REPO` to the location of this project then source the environment variable setup script:

```
INFRASTRUCTURE_REPO="$(pwd)"
source config/local/env-vars.sh
source default/account/vars.sh
```

Per AWS account variables are in `default/account/vars.sh` used for creating the Kubernetes cluster but not needed for most other operations. When creating a cluster the `kops.sh` file

Per user configuration needs to be generated. These files cannot directly use the `$INFRASTRUCTURE_REPO` environment variable for various reasons. The `config/local/local-config-init` script will generate the files needed from templates in that same folder.

```
./config/local/local-config-init
```

### VPC

The VPC Terraform code is in `default/vpc` and has a `Makefile` to help with the commands needed.

### Kubernetes

If you have an existing Kubernetes Config you can place the file as `config/private/kube_config`

If you are creating a cluster then the `default/clusters/*/cluster-config/kops.sh` file has steps to do so.
