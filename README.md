Ever frustrated with endless fetching from docker hub / other registry? 

# How to

- `{YOUR_HOST_IP}` must be replaced by your host ip in all files/configs/commands mentioned below
- `docker-compose up -d`
- set HTTP_PROXY for your docker for example with: `systemd edit docker.service` and put there something like
  ```
  [Service]
  Environment=HTTP_PROXY=http://{YOUR_HOST_IP}:3128/
  ```
- adjust and copy `daemon.json` to `/etc/docker/daemon.json` (merge if needed, or use systemctl drop-ins)
- execute `systemctl restart docker.service`
- run caches with `docker-compose up -d`

# What it actually does

- It spawns squid as caching http/https server and HTTP_PROXY will be set correctly for all your docker executions
- It spawns docker pull through mirror for docker hub requests: https://docs.docker.com/registry/recipes/mirror/#gotcha
- Requests to mutable objects are not cached (so `:latest` works fine)
- Only HTTP GET is cached


## Wipe cache

0. `rm -fr squid_cache` to remove squid cache
0. `rm -fr registry_cache` to remove registry mirror cache
