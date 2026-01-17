# os-image

[![bluebuild build badge](https://github.com/mattrayner/os-image/actions/workflows/build.yml/badge.svg)](https://github.com/mattrayner/os-image/actions/workflows/build.yml)

A custom atomic Fedora image based on [wayblue/hyprland](https://github.com/wayblueorg/wayblue/pkgs/container/hyprland), built with [BlueBuild](https://blue-build.org/).

## Features

- **Base Image**: wayblue/hyprland - Fedora atomic desktop with Hyprland window manager
- **Automatic Builds**: Daily builds at 06:00 UTC to stay up-to-date
- **Image Signing**: Signed with cosign for verification
- **Customizable**: Easy to extend with additional packages and configurations

## Installation

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable). Use at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

1. First rebase to the unsigned image to get the proper signing keys and policies installed:
   ```bash
   rpm-ostree rebase ostree-unverified-registry:ghcr.io/mattrayner/os-image:latest
   ```

2. Reboot to complete the rebase:
   ```bash
   systemctl reboot
   ```

3. Then rebase to the signed image:
   ```bash
   rpm-ostree rebase ostree-image-signed:docker://ghcr.io/mattrayner/os-image:latest
   ```

4. Reboot again to complete the installation:
   ```bash
   systemctl reboot
   ```

The `latest` tag will automatically point to the latest build.

## Customization

To customize this image for your own needs:

1. **Add packages**: Edit `recipes/recipe.yml` and add packages to the `dnf` module
2. **Add files**: Place custom files in `files/system/` - they will be copied to your image's root
3. **Add modules**: See [BlueBuild modules documentation](https://blue-build.org/reference/modules/)

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by running:

```bash
cosign verify --key cosign.pub ghcr.io/mattrayner/os-image:latest
```

## Setup

To enable image signing in GitHub Actions, you need to add the private key as a repository secret:

1. View the contents of `cosign.key` (this file is git-ignored and should never be committed)
2. Go to your repository's Settings → Secrets and variables → Actions
3. Create a new repository secret named `SIGNING_SECRET`
4. Paste the entire contents of `cosign.key` as the value

**Important**: Keep your `cosign.key` file secure and never commit it to the repository!

## Building Locally

You can build and test the image locally using the [BlueBuild CLI](https://blue-build.org/reference/bluebuild-cli/):

```bash
# Install BlueBuild CLI
cargo install --git https://github.com/blue-build/cli

# Build the image
bluebuild build recipes/recipe.yml
```

## Resources

- [BlueBuild Documentation](https://blue-build.org/)
- [wayblue Project](https://github.com/wayblueorg/wayblue)
- [Universal Blue](https://universal-blue.org/)
