## awesome 4.3 deb package for Ubuntu 16.04

This is the Dockerfile that can be used to build the deb package for *Ubuntu 16.04*.

To get a `deb` file just execute next commands:

```
cd awesome
docker build -t awesome:v1 .
docker run awesome:v1
docker cp $(docker ps -a -n=1 -q):/root/awesome_4.3.0-0~xenial0_amd64.deb .
```

Then, to install, execute

```
sudo dpkg -i ./awesome_4.3.0-0~xenial0_amd64.deb
sudo apt-get -f install  # To install missing dependencies (dpkg doesn't install any dependencies)
```

Tested with clean installation of Ubuntu 16.04 Xenial

Ubuntu 14.04 is supported too, checkout the branch [ubuntu-14.04](https://github.com/elw00d/awesome-deb-docker/tree/ubuntu-14.04)
