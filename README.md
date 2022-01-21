# percollate-docker
Docker image containing percollate running in user mode with a minimal set of permissions.

Get it from https://hub.docker.com/repository/docker/xiangronglin/percollate-alpine
with `docker pull xiangronglin/percollate-alpine`

The missing permissions are added with security options (preferred) or through linux capabilities.
See this article: https://ndportmann.com/chrome-in-docker/

## Security options
The required system calls are explicitly added to a whitelist.
Use `docker run --security-opt seccomp=seccomp.json` with the provided [seccomp.json](./seccomp.json).
It is based on [Moby's default](https://github.com/moby/moby/blob/eddbd6ff1ebf3df92129cc301d00693381f89d64/profiles/seccomp/default.json) taken one 21.01.2021 and extended with the required calls
`arch_prctl chroot clone fanotify_init name_to_handle_at open_by_handle_at setdomainname sethostname syslog unshare vhangup setns` [source](https://github.com/docker/for-linux/issues/496#issuecomment-441149510)

## Linux capabilities
Capabilities are grouped which then can be specifically assigned.
Use `docker run --cap-add=SYS_ADMIN` which contain the required ones.
Beware that this is basically root with a few less system calls available.