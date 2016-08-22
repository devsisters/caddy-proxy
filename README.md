# caddy-proxy
Automated [caddy](https://github.com/mholt/caddy) proxy for Docker containers using docker-gen.

### Usage

Start any containers you want proxied with an env var `VIRTUAL_HOST=subdomain.youdomain.com` just like [nginx-proxy](https://github.com/jwilder/nginx-proxy):
```sh
$ docker run -e VIRTUAL_HOST=foo.bar.com  ...
```

If you want the container protected by HTTP Basic Authentication add a `BASIC_AUTH` env var with the path to protect (i.e. `/`), username, and password:
```sh
$ docker run -e VIRTUAL_HOST=foo.bar.com -e BASIC_AUTH="/ myname mysecrect" ...
```

You must add AWS EC2 IAM Role or add `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` with permissions. See [lego](https://github.com/xenolf/lego#aws-route-53) for more details.

Then to run caddy-proxy:
```sh
$ docker run -v /var/run/docker.sock:/tmp/docker.sock:ro -v /data/.caddy:/root/.caddy --name caddy-proxy -p 80:80 -p 443:443 -e CADDY_OPTIONS="--email youremail@example.com" -d blackglory/caddy-proxy:0.2.1
```

When you launch new (or stop) containers caddy-proxy will reload its configuration to make the new containers available.
