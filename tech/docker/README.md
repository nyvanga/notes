# Docker

## Docker can build using an image in the registry as cache

[Specifying external cache sources](https://docs.docker.com/engine/reference/commandline/build/#specifying-external-cache-sources)

[Speedy builds with Docker Layer Caching](https://medium.com/condenastengineering/speedy-builds-with-docker-layer-caching-7ed590ac2fd1)

## Docker-in-docker

Running docker with `-v /var/run/docker.sock:/var/run/docker.sock` is NOT [docker-in-docker](https://itnext.io/docker-in-docker-521958d34efd).

Docker runs directly on the host system, which has implications. (https://stackoverflow.com/a/55849875)

Real Docker-in-docker is running the special `dind` image, which runs a "real" docker inside the docker.
```
docker run --privileged -d docker:dind
```

## Docker Compose:

- [Releases](https://github.com/docker/compose/releases/)

## Network

- https://docs.docker.com/engine/reference/commandline/network_create/
- https://medium.com/@havloujian.joachim/advanced-docker-networking-outgoing-ip-921fc3090b09


- [Container Network Model or CNM](https://github.com/docker/libnetwork/blob/master/docs/design.md)
- [bridge vs macvlan](http://hicu.be/bridge-vs-macvlan)
- [macvlan vs ipvlan](https://hicu.be/macvlan-vs-ipvlan)
- [VLAN via macvlan](http://en-designetwork.hatenablog.com/entry/2018/05/02/docker-compose-network-vlan-tag)
- [macvlan and ipvlan](https://github.com/moby/moby/blob/8926af95e4af4c3f8c6e28d1d054b9b6b7a13ff9/experimental/vlan-networks.md)

### Host access to macvlan:

- [Using Docker macvlan networks](http://blog.oddbit.com/2018/03/12/using-docker-macvlan-networks/)

In comments:
I've been pulling my hair out trying to avoid macvlans, and use existing linux bridges.

```
docker network create --driver=bridge --ip-range=192.168.1.144/28 --subnet=192.168.1.0/24 --gateway=192.168.1.1 --attachable -o "com.docker.network.bridge.name=br0" br0
```

It was annoying because docker would clobber my br0 bridge and overwrite my local IP on br0 with whatever I set in gateway. My local machine is not the gateway for br0. In the end I went old school and tried starting docker with the args I used with "docker -d" previously eg:

```
dockerd --fixed-cidr=192.168.1.144/28 -b br0 --default-gateway=192.168.1.1
```

I noticed after inspecting the default bridge from that configuration that it was setting an aux address "DefaultGatewayIP", so I took the command line options out, and created a new network like this:

```
docker network create --driver=bridge --ip-range=192.168.1.144/28 --subnet=192.168.1.0/24 --gateway=192.168.1.50 --attachable -o "com.docker.network.bridge.name=br0" --aux-address="DefaultGatewayIPv4=192.168.1.1" br0
```

Hey presto it works, my brctl defined bridge work harmoniously with my containers :-D


### Advanced brdige config:

- [Source](https://forums.docker.com/t/docker-1-10-containers-ip-in-lan/7007)

If you want your containers to join a pre-existing network, use the external option:

```
networks:
  default:
    external:
      name: my-pre-existing-network
```