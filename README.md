# Rootfs

## Introduction

in order to support user build base Clusterimage automatically,open this repo and share the related rootfs files. user can
use `auto-build` to do this job and meanwhile using git hub action with issue comment to do it. at the same time you can
use this repo to customize your own base Clusterimage such as choosing the different CRI plugin version or CNI plugin
version,even the default kubernetes cluster configuration and so on.

## How to use auto-build

auto-build only accept one arg that is image version,and will use this version to pull related kubernetes container
images, so make sure it is a valid value.

```shell
## auto-build options:
  --k8s-version         set the kubernetes k8s_version of the Clusterimage, k8s_version must be greater than 1.13
  -c, --cri             cri can be set to docker or containerd between kubernetes 1.20-1.24 versions
  -n, --buildName       set build image name, default is 'registry.cn-qingdao.aliyuncs.com/sealer-io/kubernetes:${k8s_version}'
  --platform            set the build mirror platform, the default is linux/amd64,linux/arm64
  --push                push clusterimage after building the clusterimage. The image name must contain the full name of the repository, and use -u and -p to specify the username and password.
  -u, --username        specify the user's username for pushing the Clusterimage
  -p, --password        specify the user's password for pushing the Clusterimage
  -d, --debug           show all script logs
  -h, --help            help for auto build shell scripts
```

### default build

this is will build the Clusterimage named "kubernetes:v1.22.8" without CNI plugin. and both have two platform: amd64 and
arm64 platform. that means you got four Clusterimages at the same time.

```shell
auto-build --k8s-version=v1.22.8
```

### build with specify platform

This will build a Clusterimage with amd64 platform, default is linux/amd64,linux/arm64.

```shell
bash auto-build.sh --k8s-version=v1.22.17 --cri=docker --buildName=docker.io/moss-public/kubernetes:v1.22.17 --username=xxx --password=xxx --platform=linux/amd64 --push
```

### build with specified name

this will build a Clusterimage with amd64 platform.

```shell
auto-build --k8s-version=v1.22.8 --buildName=registry.cn-qingdao.aliyuncs.com/sealer-io/kubernetes:v1.22.8
```

### build with specify CRI

this will build a Clusterimage with containerd. if user not specify the CRI ,we use containerd as Clusterimage default cri.

```shell
auto-build --k8s-version=v1.22.8 --cri=docker
```

### build with customized Clusterimage name

this will build a Clusterimage named `registry.cn-qingdao.aliyuncs.com/sealer-io/myk8s:v1.22.8`

```shell
auto-build --k8s-version=v1.22.8 --buildName=registry.cn-qingdao.aliyuncs.com/sealer-io/myk8s:v1.22.8
```

### build without pushing

if `--push`, push the clusterimage to the image registry. The image name must contain the full name of the repository.

```shell
auto-build --k8s-version=v1.22.8 --buildName=registry.cn-qingdao.aliyuncs.com/sealer-io/kubernetes:v1.22.8 --push
```

The image warehouse address is registry.cn-qingdao.aliyuncs.com.

If you do not log in to the mirror warehouse, you need to use -u and -p to specify the username and password

```shell
auto-build --k8s-version=v1.22.8 --buildName=registry.cn-qingdao.aliyuncs.com/sealer-io/kubernetes:v1.22.8 --push --username=specifyUser --password=specifyPasswd
```
