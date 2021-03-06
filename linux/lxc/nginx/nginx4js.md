<div style="text-align: center;font-size: 40px;">vue & react nginx</div>

## [nginx](../docker/nginx.md)

## [Dockerfile](Dockerfile)

```dockerfile
FROM nginx:stable-alpine

MAINTAINER bougainvilleas <caddyren@qq.com>

WORKDIR app

ENV TZ=Asia/Shanghai \
    LANG=en_US.UTF-8

COPY nginx.conf /etc/nginx/nginx.conf
# 将npm build 生成的前端包拷贝 /usr/share/nginx/html/ 这个目录下面
COPY dist  /usr/share/nginx/html/

RUN echo 'echo init ok!!'
```

## [build](build.sh)

```shell
#!/bin/bash

image_name="app"
version="0.0.0"
port="3000"


echo -e "\n==> begin delete all container of " $image_name
docker rm $(docker stop $(docker ps -a | grep $image_name | awk '{print $1}'))

echo -e "\n==> begin delete all images of " $image_name
docker rmi -f $(docker images | grep $image_name | awk '{print $3}')

echo -e "\n==> begin build your images of " $image_name
docker build -f Dockerfile -t $image_name:$version .

echo -e "\n==> begin to create a container of " $image_name
docker run -d -p $port:$port --name=$image_name --privileged=true $image_name:$version

docker logs -f -t --tail 500 $(docker ps | grep $image_name | awk '{print $1}')

docker exec -it $(docker container ls | grep $image_name | awk '{print $1}') /bin/sh

docker stop $(docker ps -a | grep $image_name | awk '{print $1}')
psid=$(docker ps -a | grep $image_name | awk '{print $1}')
if [[ -n $psid ]]; then
  docker rm $psid
fi

echo -e "\n==> begin to package your image to tar file"
docker save $image_name:$version > ../docker/$image_name-$version.tar

echo -e "\n==> begin load your images of " $image_name
docker load < $image_name-$version.tar
```

## [HOME](../../../index.md)