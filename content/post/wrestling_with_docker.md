+++
title = "Wrestling with Docker"
author = ["Alasdair McAndrew"]
date = 2018-09-15
draft = false
mathjax = true
+++

For years I have been running a blog and other web apps on a VPS running Ubuntu
14.04 and Apache - a standard
[LAMP](<https://en.wikipedia.org/wiki/LAMP%5F(software%5Fbundle)>) system.  However,
after experimenting with some apps - temporarily installing them and testing
them, only to discard them, the system was becoming a total mess.  Worst of all,
various MySQL files were ballooning out in size: the `ibdata1` file in
`/var/lib/mysql` was coming in at a whopping 37Gb (39568015360 bytes to be more accurate).

Now, there are ways of dealing with this, but I don't want to have to become an
expert in MySQL; all I wanted to do was to recover my system and make it more
manageable.

I decided to use [Docker](www.docker.com).  This is a "container system" where
each app runs in its own container - a sort of mini system which contains all
the files required to serve it up to the web.  This clearly requires a certain
amount of repetition between containers, but that's the price to be paid for
independence.  The idea is that you can start or stop any container without
affecting any of the others.  For web apps many containers are based on [Alpine
Linux](<https://alpinelinux.org>) which is a system designed to be as tiny as
possible, along with the [nginx](<https://www.nginx.com>) web server.

There seems to be a sizable ecosystem of tools to help manage and deploy
docker containers.  Given my starting position of knowing nothing, I wanted to
keep my extra tools to a minimum; I went with just two over and above docker
itself: [docker-compose](<https://docs.docker.com/compose/>), which helps design, configure, and run docker
containers, and [traefik](<https://traefik.io>), a reverse proxy, which handles all requests from the
outside world to docker containers - thus managing things like ports - as well
as interfacing with the certificate authority [Lets
Encrypt](<https://letsencrypt.org>).

My hope was that I should be able to get these all set up so they would work as
happily together as they were supposed to do.  And so indeed it has turned out,
although it took many days of fiddling, and innumerable questions to forums and
web sites (such as reddit) to make it work.

So here's my traefik configuration:

```toml
defaultEntryPoints = ["http", "https"]

[web]
address = ":8080"
  [web.auth.basic]
  users = ["admin:$apr1$v7kJtvT7$h0F7kxt.lAzFH4sZ8Z9ik."]

[entryPoints]
  [entryPoints.http]
  address = ":80"
    [entryPoints.http.redirect]
      entryPoint = "https"
  [entryPoints.https]
  address = ":443"
    [entryPoints.https.tls]

[traefikLog]
  filePath="./traefik.log"
  format = "json"


# Below here comes from
#   www.smarthomebeginner.com/traefik-reverse-proxy-tutorial-for-docker/
# with values adjusted for local use, of course

# Let's encrypt configuration
[acme]
email="amca01@gmail.com"
storage="./acme.json"
acmeLogging=true
onHostRule = true
entryPoint = "https"
  # Use a HTTP-01 acme challenge rather than TLS-SNI-01 challenge
  [acme.httpChallenge]
  entryPoint = "http"

[[acme.domains]]
  main = "numbersandshapes.net"
  sans = ["monitor.numbersandshapes.net", "adminer.numbersandshapes.net", "portainer.numbersandshapes.net", "kanboard.numbersandshapes.net", "webwork.numbersandshapes.net",
 "blog.numbersandshapes.net"]

# Connection to docker host system (docker.sock)
[docker]
endpoint = "unix:///var/run/docker.sock"
domain = "numbersandshapes.net"
watch = true
# This will hide all docker containers that don't have explicitly set label to "enable"
exposedbydefault = false
```

and (part of) my docker-compose configuration, the file `docker-compose.yml`:

```yml
version: "3"

networks:
  proxy:
    external: true
  internal:
    external: false

services:

  traefik:
    image: traefik:1.6.0-alpine
    container_name: traefik
    restart: always
    command: --web --docker --logLevel=DEBUG
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - $PWD/traefik.toml:/traefik.toml
      - $PWD/acme.json:/acme.json
    networks:
      - proxy
    ports:
      - "80:80"
      - "443:443"
    labels:
      - traefik.enable=true
      - traefik.backend=traefik
      - traefik.frontend.rule=Host:monitor.numbersandshapes.net
      - traefik.port=8080
      - traefik.docker.network=proxy

  blog:
    image: blog
    volumes:
      - /home/amca/docker/whats_this/public:/usr/share/nginx/html
    networks:
      - internal
      - proxy
    labels:
      - traefik.enable=true
      - traefik.backend=blog
      - traefik.docker.network=proxy
      - traefik.port=80
      - traefik.frontend.rule=Host:blog.numbersandshapes.net
```

The way this works, at least in respect of this blog, is that files copied into
the directory `/home/amca/docker/whats_this/public` on my VPS will be
automatically served by nginx.  So all I now need is a command on my local
system (on which I do all my blog writing), which serves up these files.  I've
called it `docker-deploy`:

```nil
hugo -b "https://blog.numbersandshapes.net/" -t "blackburn" && rsync -avz -e "ssh" --delete public/ amca@numbersandshapes.net:~/docker/whats_this/public
```

Remarkably enough, it all works!

One issue I had at the beginning was that my original blog was served up at the
URL `https://numberdsandshapes.net/blog` and for some reason these links were
still appearing in my new blog.  It turned out (after a lot of anguished
messages) that it was my mis-handling of `rsync`.  I just ended up deleting
everything except for the blog source files, and re-created everything from scratch.

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
