Skopeo
=====
#### list image tags
```bash
skopeo list-tags docker://docker.io/library/nginx
```

#### inspect image
```bash
skopeo inspect docker://docker.io/library/nginx:1.29.5-alpine3.23

skopeo copy docker://docker.io/library/nginx:1.29.5-alpine3.23 oci:/tmp/nginx:1.29.5-alpine3.23
podman run -dit -p 8080:8080 oci:/tmp/nginx:1.29.5-alpine3.23
```

Podman
=====
#### Create pod
```bash 
podman pod create --name test-pod -p 8080:8080 -v ./:/var/www/app
```

#### Add Container
```bash
podman create --pod test-pod  --name nginx docker.io/library/nginx:1.29.5-alpine3.23

podman run --pod new:test-pod3 --name nginx3 -d -p 8082:80 docker.io/library/nginx:1.29.5-alpine3.23

podman pod list

podman container ps -a

podman search nginx --filter=is-official
podman search docker.io/library/nginx --list-tags
podman pull docker.io/library/nginx:1-alpine3.22
podman images

podman unshare
podman image mount docker.io/library/nginx:1-alpine3.22
```

#### systemd
```bash
podman generate systemd --name nginx --files

systemctl --user daemon-reload
systemctl --user start nginx.service
systemctl --user enable nginx.service

loginctl enable-linger
```

#### Podman secret
```bash
printf "mysecret" | podman secret create mysecret -
podman secret create mysecret < secret.txt
podman secret ls
podman secret inspect mysecret
podman run -it --rm --secret source=mysecret,target=/run/secrets/mysecret ubi9/ubi /bin/bash
podman run -it --rm --secret mysecret,type=env,target=MY_SECRET ubi9/ubi /bin/bash
```

Troubleshooting
===============
```bash
podman inspect nginx | grep Pid
podman inspect nginx --format {{.State.Pid}}

crun list

podman events --filter container=nginx --stream=false
man podman-events

### List namespaces
lsns -p <CONTAINER_PID>
### Enter Network namespace
sudo nsenter -n -t <CONTAINER_PID> ss -pant
### Enter Process namespace
sudo nsenter -p -t <CONTAINER_PID> -r top
### Enter UTC namespace
sudo nsenter -u -t <CONTAINER_PID> hostname
### Enter all namespaces
sudo nsenter -t 31674 -a sh
man 7 namespaces

podman ps -a --format "{{.ID}} {{.Image}} {{.Names}} {{.Status}}"

#### registries
man containers-registries.conf
podman info | grep -A 5 registries
```

Podman VS Code Dev Container
=============
```bash
systemctl --user enable podman.socket
systemctl --user start podman.socket

alias docker=podman
export DOCKER_HOST=unix:///run/user/$(id -u)/podman/podman.sock

sudo dnf install slirp4netns
sudo touch /etc/containers/nodocker
```

Podman compose
============
```bash
podman compose -f compose.elk.yml up -d
podman compose -f compose.dev.yml --project-name python-test up -d --build --scale backend=2
podman compose -f compose.elk.yml --project-name elk up -d
podman compose down --project-name python-test -f compose.dev.yml
```

Podlet
======
```bash
podlet run -d -p 8080:8080 --name nginx docker.io/library/nginx:1.29.5-alpine3.23
```
