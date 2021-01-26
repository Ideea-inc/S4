<p align="center">
<img width="192" src="https://ideea.io/assets/img/logos/s4/s4-logo-dark.svg" alt="S4">
</p>

---
### [S4](https://ideea.io/s4) &nbsp;&nbsp;|&nbsp;&nbsp; [YouTube Tutorial](https://youtu.be/RMNjpAmCvcQ) &nbsp;&nbsp;|&nbsp;&nbsp; [Ideea.io](https://ideea.io) &nbsp;&nbsp;|&nbsp;&nbsp; [Hire Me](https://ideea.io/about#hire-me)
---


# S4
S4 is 100% compatible S3 storage, accessed through Tor and distributed using IPFS.

Tor hides the physical location of the S4 server. [IPFS](https://github.com/ipfs/ipfs) makes your data permanently accessible and is impossible to take offline once it has been published. A [sidecar docker container](https://github.com/Ideea-inc/s4-client) is provided to seamlessly proxy requests from your existing S3 code over Tor to S4.


<p align="center">
<a href="https://youtu.be/RMNjpAmCvcQ">
<img width="450" src="https://ideea.io/assets/img/s4-youtube-embed.png" alt="S4 Video Tutorial">
</a>
</p>

## Create S4 server
You can set-up your own local S4 Server in just 4 commands.
```sh
$ git clone git@github.com:Ideea-inc/S4.git

$ cd S4/

$ docker run -it --rm -v $(pwd)/data/tor:/web ideea/s4-tor-proxy generate ^s4

$ docker-compose up
```
## Node.js AWS-SDK Example
Once you have created your local S4 server, you can test it using the included [example](https://github.com/Ideea-inc/S4/blob/master/example/src/index.js). This demo shows the current Node.js AWS-SDK working with the [sidecar docker container](https://github.com/Ideea-inc/s4-client) to proxy the AWS S3 requests over Tor back to your local S4 server.

```sh
$ cd example/

$ docker-compose up -d

$ docker exec -ti my-app node src/index.js
```

## Architecture
<p  align="center">
<img src="https://ideea.io/assets/img/s4-diagram.png"  alt="S4">
</p>

Tor does not just provide anonymity, Tor acts as a DNS. S4 uses Tor hidden services, data is uploaded into an S4 bucket over Tor. This also means you can create a free .onion domain name without needing any 3rd-parties. A [sidecar container](https://github.com/Ideea-inc/s4-client) is provided to seamlessly proxy requests from your existing S3 code over Tor to S4.

IPFS acts as a CDN. Each S4 bucket is published to IPFS under a separate key allowing you to address your files using any existing IPFS HTTP gateway like this [https://ipfs.io/ipns/[BUCKET-KEY]/image.jpg](http://ipfs.io/ipns/QmTvcZ719MEiYjhn2RxwtiYGkJr6ea4xnSGqdxj5vRii8k/flag.jpg). This link will never change or go offline, the more people who request it, the faster the data will be returned to the user.


## Build a Node
<p  align="center">
<img src="https://ideea.io/assets/img/s4-node.png#1" width="300" alt="S4 node">
</p>
S4 is designed to democratise object-storage technology so anyone can have S3-like data storage without needing to depend on giant cloud tech providers. It was specifically built to run on a Raspberry Pi in production. A basic overview on how to create an S4 node is provided below.

- Install Ubuntu 20.04

-  `$ git clone git@github.com:Ideea-inc/S4.git`

-  `$ cd S4`

-  `$ ./install` 

-  `$ sudo mount /dev/XXX /mnt/ssd`

-  `$ sed -i "" "s#S4_ROOT=.*#S4_ROOT=/mnt/ssd#" .env`

-  `$ sed -i "" "s#ACCESS_KEY=.*#ACCESS_KEY=$(openssl rand -hex 10)#" .env`

-  `$ sed -i "" "s#SECRET_KEY=.*#SECRET_KEY=$(openssl rand -hex 20)#" .env`

-  `$ sudo reboot`

-  `$ docker run -it --rm -v /mnt/ssd/tor:/web ideea/s4-tor-proxy:arm64v8 generate ^s4`

-  `$ docker-compose up`


