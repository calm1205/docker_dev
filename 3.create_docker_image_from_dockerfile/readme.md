# Build docker image from dockerfile

```bash
# .はdockerfileのパス。dockerfileが存在しているディレクトリで実行するので.になっている
docker build --tag new_nginx .

# imageができていることを確認
docker images
# REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
# new_nginx        latest    c7ae333ccf7b   6 seconds ago   159MB

# container起動
docker run -d --rm --name nginx -p 80:80 new_nginx

# index.html確認
curl localhost:80
# -> hello world
```

## docker history

dockerfile で実行された内容がレイヤーとして確認できる。

```bash
docker history new_nginx
# IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
# c7ae333ccf7b   7 minutes ago   RUN /bin/sh -c apt update && apt install -y …   17.7MB    buildkit.dockerfile.v0
# <missing>      7 minutes ago   COPY index.html index.html # buildkit           12B       buildkit.dockerfile.v0
# <missing>      7 minutes ago   WORKDIR /usr/share/nginx/html                   0B        buildkit.dockerfile.v0
```
