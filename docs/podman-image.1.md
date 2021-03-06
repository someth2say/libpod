% podman-image(1)

## NAME
podman\-image - Manage images

## SYNOPSIS
**podman image** *subcommand*

## DESCRIPTION
The image command allows you to manage images

## COMMANDS

| Command  | Man Page                                  | Description                                                                    |
| -------- | ----------------------------------------- | ------------------------------------------------------------------------------ |
| build    | [podman-build(1)](podman-build.1.md)      | Build a container using a Dockerfile.                                          |
| exists   | [podman-exists(1)](podman-image-exists.1.md)      | Check if a image exists in local storage                                          |
| history  | [podman-history(1)](podman-history.1.md)  | Show the history of an image.                                                  |
| import   | [podman-import(1)](podman-import.1.md)    | Import a tarball and save it as a filesystem image.                            |
| inspect  | [podman-inspect(1)](podman-inspect.1.md)  | Display a image or image's configuration.                                      |
| list     | [podman-images(1)](podman-images.1.md)    | List the container images on the system.                                           |
| load     | [podman-load(1)](podman-load.1.md)        | Load an image from the docker archive.                                         |
| ls       | [podman-images(1)](podman-images.1.md)    | List the container images on the system.                                           |
| pull     | [podman-pull(1)](podman-pull.1.md)        | Pull an image from a registry.                                                 |
| prune| [podman-container-prune(1)](podman-container-prune.1.md)        | Removed all unused images from the local store                                 |
| push     | [podman-push(1)](podman-push.1.md)        | Push an image from local storage to elsewhere.                                 |
| rm       | [podman-rm(1)](podman-rmi.1.md)           | Removes one or more locally stored images.                                     |
| save     | [podman-save(1)](podman-save.1.md)        | Save an image to docker-archive or oci.                                        |
| tag      | [podman-tag(1)](podman-tag.1.md)          | Add an additional name to a local image.                                       |
| trust    | [podman-image-trust(1)](podman-image-trust.1.md)  | Manage container image trust policy.                                   |
| sign    | [podman-image-sign(1)](podman-image-sign.1.md)  | Sign an image.                                                            |

## SEE ALSO
podman
