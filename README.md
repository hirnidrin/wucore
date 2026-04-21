# wucore server images
[![build badge](https://github.com/hirnidrin/wucore/actions/workflows/build.yml/badge.svg)](https://github.com/hirnidrin/wucore/actions/workflows/build.yml)

This repo is a copy of the [BlueBuild image customizer](github.com/blue-build/template) template. I use it to build slightly customized Fedora CoreOS server images.

Read the [BlueBuild docs](https://blue-build.org/learn/getting-started/) for detailed information on how to customize a base image.

## Images

name | base image | recipe | notes
-|-|-|-
wucore | [Fedora CoreOS stable](https://quay.io/repository/fedora/fedora-coreos?tab=tags) | [recipe-wucore.yml](./recipes/recipe-wucore.yml) | homelab container platform, ready for k3s install

## Installation

To manually rebase an existing Fedora Core OS installation to one of the customized images:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/hirnidrin/<image-name>:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/hirnidrin/<image-name>:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the base image version specified in `recipe-<image-name>.yml`, so you won't get accidentally updated to the next major version.

## ISO

There is no ISOs. Start off an existing Fedora Core OS installation and rebase.

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/hirnidrin/<image-name>
```
