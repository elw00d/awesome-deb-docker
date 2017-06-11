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

After that you can install `lua-lgi=0.9.1` from sources to avoid errors with menubar (see https://github.com/awesomeWM/awesome/pull/1350), but it is not required to run awesome (especially, if you don't use menubar `super` + `p`). But if you decided to do this, you can use following commands:

```bash
sudo apt-get install liblua5.2-dev lua5.2 libgirepository1.0-dev
wget https://keplerproject.github.io/luarocks/releases/luarocks-2.3.0.tar.gz
tar xf luarocks-2.3.0.tar.gz
cd luarocks-2.3.0
./configure --lua-version=5.2 --with-lua-include=/usr/include/lua5.2
make build
sudo make install
luarocks install lgi 0.9.1
```

These steps will build and install fresh version of `lgi` into your `/usr/local`, and menubar will work without errors. But it is not required to run awesome.

Tested with clean installation of Ubuntu 14.04 Trusty

Ubuntu 16.04 is supported too, checkout the branch [ubuntu-16.04](https://github.com/elw00d/awesome-deb-docker/tree/ubuntu-16.04)
