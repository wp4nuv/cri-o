# Install with Package Managers

CRI-O builds for native package managers using [openSUSE's OBS](build.opensuse.org)

## Supported Versions

Below is a compatibility matrix between versions of CRI-O (y-axis)
and distributions (x-axis)
<!-- markdownlint-disable MD013 -->

|      | Fedora 31+ | openSUSE | CentOS_8 | CentOS_8_Stream | CentOS_7 | Debian_Unstable | Debian_Testing | Debian 10 | Rasbian_10 | xUbuntu_20.04 | xUbuntu_19.10 | xUbuntu_19.04 | xUbuntu_18.04 |
| ---- | ---------- | -------- | -------- | --------------- | -------- | --------------- | -------------- | --------- | ---------- | ------------- | ------------- | ------------- | ------------- |
| 1.18 | ✓          | ✓        | ✓        | ✓               |          | ✓               | ✓              |           |            | ✓             |               |               |               |
| 1.17 | ✓          | ✓        | ✓        | ✓               | ✓        | ✓               | ✓              | ✓         | ✓          | ✓             | ✓             | ✓             | ✓             |
| 1.16 | ✓          | ✓        | ✓        | ✓               | ✓        | ✓               | ✓              | ✓         | ✓          | ✓             | ✓             | ✓             | ✓             |
<!-- markdownlint-enable MD013 -->

## Installation Instructions

### The `VERSION` variable

Before installing `cri-o`, choose a supported version for your operating system
that corresponds to the kubernetes minor version you are using. For example,
if you are using kubernetes `1.18.x`, we recommend the `VERSION` variable
match our `1.18` branch.
For example: `export VERSION=1.18`
<!-- Added the directory separator for pinning an install to a specific version -->
We also save releases as subprojects. If you'd, for instance, like to
pin `cri-o` to a specific version, for example, `1.18.3` you can set `export VERSION=1.18:/1.18.3`

### openSUSE

```shell
sudo zypper install cri-o
```

### Fedora 31 or later

```shell
sudo dnf module enable cri-o:$VERSION
sudo dnf install cri-o
```

For Fedora, we only support setting minor versions. i.e: `VERSION=1.18`, and do
not support pinning patch versions: `VERSION=1.18.3`

### Other yum based operating systems

To install on the following operating systems, set the environment variable ```$OS```
to the appropriate value from the following table:

| Operating system | $OS               |
| ---------------- | ----------------- |
| Centos 8         | `CentOS_8`        |
| Centos 8 Stream  | `CentOS_8_Stream` |
| Centos 7         | `CentOS_7`        |
<!--- TODO: figure out if Fedora is included in patch version releases
and figure out how to make the $VERSION variable work for both yum and apt os
Is revisiting the idea of a single VERSION variable worthwhile?
PMD
--->
And then run the following as root:

<!-- markdownlint-disable MD013 -->
```shell
curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/devel:kubic:libcontainers:stable.repo
curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.repo https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/devel:kubic:libcontainers:stable:cri-o:$VERSION.repo
yum install cri-o
```
<!-- markdownlint-enable MD013 -->

### Apt based operating systems
<!-- Modified the text so it reiterates the need for the VERSION env variable -->
Note: this tutorial assumes you have set the environment variable `VERSION`
as stated above and having `curl` and `gnupg` installed.

Set the environment variable `$OS` to the appropriate value from the
following table:

| Operating system | $OS               |
| ---------------- | ----------------- |
| Debian 10        | `Debian_10`       |
| Debian 11        | `Debian_11`       |
| Raspbian 10      | `Raspbian_10`     |
| Raspbian 11      | `Raspbian_11`     |
| Ubuntu 18.04     | `xUbuntu_18.04`   |
| Ubuntu 20.04     | `xUbuntu_20.04`   |
| Ubuntu 21.10     | `xUbuntu_21.10`   |
| Ubuntu 22.04     | `xUbuntu_22.04`   |

And then run the following as root or using `sudo`:

<!-- markdownlint-disable MD013 -->
```shell
echo "deb [signed-by=/usr/share/keyrings/libcontainers-archive-keyring.gpg] https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb [signed-by=/usr/share/keyrings/libcontainers-crio-archive-keyring.gpg] https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

mkdir -p /usr/share/keyrings
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | gpg --dearmor -o /usr/share/keyrings/libcontainers-archive-keyring.gpg
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/Release.key | gpg --dearmor -o /usr/share/keyrings/libcontainers-crio-archive-keyring.gpg

apt-get update
apt-get install cri-o
```
<!-- markdownlint-enable MD013 -->
