## awesome 4.0 deb package for Ubuntu

This is Dockerfile for building the deb-package for Ubuntu.

To get a `deb` file just execute next commands:

```
cd awesome
docker build -t awesome:v1 .
docker run awesome:v1
docker cp $(docker ps -a -n=1 -q):/root/awesome_4.0.0-4~trusty0_amd64.deb .
```

Tested with 14.04 (Trusty)
