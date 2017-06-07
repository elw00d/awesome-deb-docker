## awesome 4.1 deb package for Ubuntu 14.04

![Screenshot](/screenshot.png?raw=true "Screenshot")

This is the Dockerfile that can be used to build the deb package for *Ubuntu 14.04*.

To get a `deb` file just execute next commands:

```
cd awesome
docker build -t awesome:v4.1 .
docker run awesome:v4.1
docker cp $(docker ps -a -n=1 -q):/root/awesome_4.1.0-1~trusty0_amd64.deb .
```

Then, to install, execute

```
sudo dpkg -i ./awesome_4.1.0-1~trusty0_amd64.deb
sudo apt-get -f install  # To install missing dependencies (dpkg doesn't install any dependencies)
```

Tested with clean installation of Ubuntu 14.04 Trusty

Ubuntu 16.04 is supported too, checkout the branch [ubuntu-16.04](https://github.com/elw00d/awesome-deb-docker/tree/ubuntu-16.04)
