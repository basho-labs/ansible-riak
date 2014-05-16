# Asset Caching

In order to speed up subsequent Vagrant runs we suggest you install the
Vagrant [proxyconf plugin](https://github.com/tmatilai/vagrant-proxyconf) and
[Polipo](http://www.pps.univ-paris-diderot.fr/~jch/software/polipo/).

## Installation

### vagrant-proxyconf

In order to install the `vagrant-proxyconf` plugin, ensure that Vagrant is
installed and run the following command:

```bash
$ vagrant plugin install vagrant-proxyconf
```

### Polipo

There are several methods of installing Polipo. A few are listed below:

#### Homebrew

```bash
$ brew install polipo
```

#### Ubuntu

```bash
$ sudo apt-get install polipo
```

## Configuration

In order to configure Polipo, first, create a cache directory:

```bash
$ mkdir -p ~/.polipo-cache
```

Then, create a Polipo configuration file (`~/.polipo` or `/etc/polipo/config`)
with the following settings:

```
proxyAddress=0.0.0.0
disableIndexing=false
disableServersList=false
allowedClients=0.0.0.0/0
diskCacheRoot=~/.polipo-cache
```
