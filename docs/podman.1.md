% podman(1)

## NAME
podman - Simple management tool for containers and images

## SYNOPSIS
**podman** [*options*] *command*

## DESCRIPTION
podman is a simple client only tool to help with debugging issues when daemons
such as CRI runtime and the kubelet are not responding or failing. A shared API
layer could be created to share code between the daemon and podman. podman does not
require any daemon running. podman utilizes the same underlying components that
crio uses i.e. containers/image, container/storage, oci-runtime-tool/generate,
runc or any other OCI compatible runtime. podman shares state with crio and so
has the capability to debug pods/images created by crio.

**podman [GLOBAL OPTIONS]**

## GLOBAL OPTIONS

**--help, -h**

Print usage statement

**--cgroup-manager**

CGroup manager to use for container cgroups. Supported values are cgroupfs or systemd (default). Setting this flag can cause certain commands to break when called on containers created by the other CGroup manager type.

**--cpu-profile**

Path to where the cpu performance results should be written

**--hooks-dir**=**path**

Each `*.json` file in the path configures a hook for Podman containers.  For more details on the syntax of the JSON files and the semantics of hook injection, see `oci-hooks(5)`.  Podman and libpod currently support both the 1.0.0 and 0.1.0 hook schemas, although the 0.1.0 schema is deprecated.

This option may be set multiple times; paths from later options have higher precedence (`oci-hooks(5)` discusses directory precedence).

For the annotation conditions, libpod uses any annotations set in the generated OCI configuration.

For the bind-mount conditions, only mounts explicitly requested by the caller via `--volume` are considered.  Bind mounts that libpod inserts by default (e.g. `/dev/shm`) are not considered.

If `--hooks-dir` is unset for root callers, Podman and libpod will currently default to `/usr/share/containers/oci/hooks.d` and `/etc/containers/oci/hooks.d` in order of increasing precedence.  Using these defaults is deprecated, and callers should migrate to explicitly setting `--hooks-dir`.

Podman and libpod currently support an additional `precreate` state which is called before the runtime's `create` operation.  Unlike the other stages, which receive the container state on their standard input, `precreate` hooks receive the proposed runtime configuration on their standard input.  They may alter that configuration as they see fit, and write the altered form to their standard output.

**WARNING**: the `precreate` hook lets you do powerful things, such as adding additional mounts to the runtime configuration.  That power also makes it easy to break things.  Before reporting libpod errors, try running your container with `precreate` hooks disabled to see if the problem is due to one of your hooks.

**--log-level**

Log messages above specified level: debug, info, warn, error (default), fatal or panic

**--namespace**

Set libpod namespace. Namespaces are used to separate groups of containers and pods in libpod's state.
When namespace is set, created containers and pods will join the given namespace, and only containers and pods in the given namespace will be visible to Podman.

**--root**=**value**

Storage root dir in which data, including images, is stored (default: "/var/lib/containers/storage" for UID 0, "$HOME/.local/share/containers/storage" for other users).
Default root dir is configured in /etc/containers/storage.conf.

**--runroot**=**value**

Storage state directory where all state information is stored (default: "/var/run/containers/storage" for UID 0, "/var/run/user/$UID/run" for other users).
Default state dir is configured in /etc/containers/storage.conf.

**--runtime**=**value**

Name of the OCI runtime as specified in libpod.conf or absolute path to the OCI compatible binary used to run containers.

**--storage-driver, -s**=**value**

Storage driver.  The default storage driver for UID 0 is configured in /etc/containers/storage.conf (`$HOME/.config/containers/storage.conf` in rootless mode), and is *vfs* for non-root users when *fuse-overlayfs* is not available.  The `STORAGE_DRIVER` environment variable overrides the default.  The --storage-driver specified driver overrides all.

Overriding this option will cause the *storage-opt* settings in /etc/containers/storage.conf to be ignored.  The user must
specify additional options via the `--storage-opt` flag.

**--storage-opt**=**value**

Storage driver option, Default storage driver options are configured in /etc/containers/storage.conf (`$HOME/.config/containers/storage.conf` in rootless mode). The `STORAGE_OPTS` environment variable overrides the default. The --storage-opt specified options overrides all.

**--syslog**

output logging information to syslog as well as the console

**--version, -v**

Print the version

## Exit Status

The exit code from `podman` gives information about why the container
failed to run or why it exited.  When `podman` commands exit with a non-zero code,
the exit codes follow the `chroot` standard, see below:

**_125_** if the error is with podman **_itself_**

    $ podman run --foo busybox; echo $?
    # flag provided but not defined: --foo
      See 'podman run --help'.
      125

**_126_** if executing a **_container command_** and the the **_command_** cannot be invoked

    $ podman run busybox /etc; echo $?
    # exec: "/etc": permission denied
      podman: Error response from daemon: Contained command could not be invoked
      126

**_127_** if executing a **_container command_** and the the **_command_** cannot be found
    $ podman run busybox foo; echo $?
    # exec: "foo": executable file not found in $PATH
      podman: Error response from daemon: Contained command not found or does not exist
      127

**_Exit code_** of **_container command_** otherwise

    $ podman run busybox /bin/sh -c 'exit 3'
    # 3


## COMMANDS

| Command                                   | Description                                                                    |
| ----------------------------------------- | ------------------------------------------------------------------------------ |
| [podman-attach(1)](podman-attach.1.md)    | Attach to a running container.                                                 |
| [podman-build(1)](podman-build.1.md)      | Build a container using a Dockerfile.                                          |
| [podman-commit(1)](podman-commit.1.md)    | Create new image based on the changed container.                               |
| [podman-container(1)](podman-container.1.md)    | Manage Containers.                                                       |
| [podman-cp(1)](podman-cp.1.md)            | Copy files/folders between a container and the local filesystem.               |
| [podman-create(1)](podman-create.1.md)    | Create a new container.                                                        |
| [podman-diff(1)](podman-diff.1.md)        | Inspect changes on a container or image's filesystem.                          |
| [podman-exec(1)](podman-exec.1.md)        | Execute a command in a running container.                                      |
| [podman-export(1)](podman-export.1.md)    | Export a container's filesystem contents as a tar archive.                     |
| [podman-history(1)](podman-history.1.md)  | Show the history of an image.                                                  |
| [podman-image(1)](podman-image.1.md)      | Manage Images.                                                                 |
| [podman-images(1)](podman-images.1.md)    | List images in local storage.                                                  |
| [podman-import(1)](podman-import.1.md)    | Import a tarball and save it as a filesystem image.                            |
| [podman-info(1)](podman-info.1.md)        | Displays Podman related system information.                                    |
| [podman-inspect(1)](podman-inspect.1.md)  | Display a container or image's configuration.                                  |
| [podman-kill(1)](podman-kill.1.md)        | Kill the main process in one or more containers.                               |
| [podman-load(1)](podman-load.1.md)        | Load an image from the docker archive.                                         |
| [podman-login(1)](podman-login.1.md)      | Login to a container registry.                                                 |
| [podman-logout(1)](podman-logout.1.md)    | Logout of a container registry.                                                |
| [podman-logs(1)](podman-logs.1.md)        | Display the logs of a container.                                               |
| [podman-mount(1)](podman-mount.1.md)      | Mount a working container's root filesystem.                                   |
| [podman-pause(1)](podman-pause.1.md)      | Pause one or more containers.                                                  |
| [podman-port(1)](podman-port.1.md)        | List port mappings for the container.                                          |
| [podman-ps(1)](podman-ps.1.md)            | Prints out information about containers.                                       |
| [podman-pull(1)](podman-pull.1.md)        | Pull an image from a registry.                                                 |
| [podman-push(1)](podman-push.1.md)        | Push an image from local storage to elsewhere.                                 |
| [podman-restart(1)](podman-restart.1.md)  | Restart one or more containers.                                                |
| [podman-rm(1)](podman-rm.1.md)            | Remove one or more containers.                                                 |
| [podman-rmi(1)](podman-rmi.1.md)          | Removes one or more locally stored images.                                     |
| [podman-run(1)](podman-run.1.md)          | Run a command in a container.                                                  |
| [podman-save(1)](podman-save.1.md)        | Save an image to docker-archive or oci.                                        |
| [podman-search(1)](podman-search.1.md)    | Search a registry for an image.                                                |
| [podman-start(1)](podman-start.1.md)      | Starts one or more containers.                                                 |
| [podman-stats(1)](podman-stats.1.md)      | Display a live stream of one or more container's resource usage statistics.    |
| [podman-stop(1)](podman-stop.1.md)        | Stop one or more running containers.                                           |
| [podman-tag(1)](podman-tag.1.md)          | Add an additional name to a local image.                                       |
| [podman-top(1)](podman-top.1.md)          | Display the running processes of a container.                                  |
| [podman-umount(1)](podman-umount.1.md)    | Unmount a working container's root filesystem.                                 |
| [podman-unpause(1)](podman-unpause.1.md)  | Unpause one or more containers.                                                |
| [podman-version(1)](podman-version.1.md)  | Display the Podman version information.                                        |
| [podman-wait(1)](podman-wait.1.md)        | Wait on one or more containers to stop and print their exit codes.             |

## FILES

**libpod.conf** (`/etc/containers/libpod.conf`)

    libpod.conf is the configuration file for all tools using libpod to manage containers.  When Podman runs in rootless mode, then the file `$HOME/.config/containers/libpod.conf` is used.

**mounts.conf** (`/usr/share/containers/mounts.conf` and optionally `/etc/containers/mounts.conf`)

    The mounts.conf file specifies volume mount directories that are automatically mounted inside containers when executing the `podman run` or `podman start` commands. When Podman runs in rootless mode, the file `$HOME/.config/containers/mounts.conf` is also used. Please refer to containers-mounts.conf(5) for further details.

**policy.json** (`/etc/containers/policy.json`)

    Signature verification policy files are used to specify policy, e.g. trusted keys, applicable when deciding whether to accept an image, or individual signatures of that image, as valid.

**registries.conf** (`/etc/containers/registries.conf`)

    registries.conf is the configuration file which specifies which container registries should be consulted when completing image names which do not include a registry or domain portion.

    When Podman runs in rootless mode, the file `$HOME/.config/containers/registries.conf` is used.

**storage.conf** (`/etc/containers/storage.conf`)

    storage.conf is the storage configuration file for all tools using containers/storage

    The storage configuration file specifies all of the available container storage options for tools using shared container storage.

    When Podman runs in rootless mode, the file `$HOME/.config/containers/storage.conf` is also loaded.

## Rootless mode
Podman can also be used as non-root user.  When podman runs in rootless mode, a user namespace is automatically created for the user, defined in /etc/subuid and /etc/subgid.

Containers created by a non-root user are not visible to other users and are not seen or managed by podman running as root.

It is required to have multiple uids/gids set for an user.  Be sure the user is present in the files `/etc/subuid` and `/etc/subgid`.

If you have a recent version of usermod, you can execute the following
commands to add the ranges to the files

	$ sudo usermod --add-subuids 10000-75535 USERNAME
	$ sudo usermod --add-subgids 10000-75535 USERNAME

Or just add the content manually.

	$ echo USERNAME:10000:65536 >> /etc/subuid
	$ echo USERNAME:10000:65536 >> /etc/subgid

See the `subuid(5)` and `subgid(5)` man pages for more information.

Images are pulled under `XDG_DATA_HOME` when specified, otherwise in the home directory of the user under `.local/share/containers/storage`.

Currently the slirp4netns package is required to be installed to create a network device, otherwise rootless containers need to run in the network namespace of the host.

## SEE ALSO
`containers-mounts.conf(5)`, `containers-registries.conf(5)`, `containers-storage.conf(5)`, `crio(8)`, `libpod.conf(5)`, `oci-hooks(5)`, `policy.json(5)`, `subuid(5)`, `subgid(5)`, `slirp4netns(1)`

## HISTORY
Dec 2016, Originally compiled by Dan Walsh <dwalsh@redhat.com>
