# docker-cert-asterisk13-centos7
Author: Gonzalo Marcote - gonzalomarcote@gmail.com

##### Docker image with Certified Asterisk 13 LTS version on Centos7 64bits

This is the Certified Asterisk 13.1-cert1 version on Centos7 X86_64 with SIP and new PJSIP channels. Certified Asterisk 13 is the latest LTS version recommended for production systems with a release frequency of 2 - 4 times per year.

It includes:

- Certified Asterisk 13.1-cert1
- Sip and new pjsip channel enabled
- Only g729 english sounds and MOH

To pull it:

`# docker pull gonzalomarcote/docker-cert-asterisk13-centos7`

If you pull it from Dcoker it will be compiled within the "Automated Build Repository" and for 64bits platform. In this case your platform could differ too much from the one it was compiled and arise some "cross platform" errors when you try to start asterisk image (for ex: asterisk Illegal instruction core dumped).
If this is the case, it will be better to download it from git and compile it on your own platform/server from the Dockerfile:

`$ git clone https://github.com/gonzalomarcote/docker-cert-asterisk13-centos7 docker-cert-asterisk13-centos7`

`$ docker build -t="myrepository/asterisk01" .`

To execute it:

Asterisk PBX needs to use a big range of ports, so it needs to be executed with docker version 1.5.0 or higher (available in docker ubuntu sources) for being able to launch the image specifying a range of ports. For example:

`# docker run --restart=always --name asterisk01 -d -p 5060-5065:5060-5065/tcp -p 10000-10500:10000-10500/udp gonzalomarcote/docker-cert-asterisk13-centos7`

and connect to asterisk CLI with:
`# docker exec -t -i asterisk01 /usr/sbin/rasterisk -vvvvv`

Notice:

> Seems that opening too much ports in a docker images, consumes a lot of resources in your docker host and may fail to launch it. So giving that every SIP call can use up to 4 RTP ports, it is convenient to open only the necessary RTP ports for the expected calls. In this case we open 500 RTP ports for 125 expected concurrent calls. From 10000 to 10500. Don't forget to configure that RTP ports in the /etc/asterisk/rtp.conf file:

```
# rtpstart=10000
# rtpend=10500
```
