# Build Docker image

起動中のコンテナから new_nginx イメージ作成

### run container

```bash
docker run -d --rm -p 80:80 --name nginx --volume $(pwd):/usr/share/nginx/html nginx
curl localhost:80
# -> hello world
```

### build docker image

```bash
docker commit nginx new_nginx
```

新しく作成した image から container を起動&中身の確認

```bash
docker images
# REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
# new_nginx        latest    838325f3c8fb   4 seconds ago   141MB

# new_nginxイメージからコンテナ起動して確認
docker run -d --rm -p 80:80 --name nginx new_nginx

curl localhost:80
# -> <!DOCTYPE html>...
# Hello Worldは返ってこない。
# Docker Volumeでmountされたファイルはコンテナ内に保存されない。
# コンテナの内部から参照しているだけ。
```

container の中身が更新された image を build

```bash
# container 起動
docker run -d --rm -p 80:80 --name nginx nginx

# containerの中で直接ファイル編集
docker exec -it nginx bash
echo 'hello world' > /usr/share/nginx/html/index.html
exit

curl localhost:80
# -> hello world

# imageを作成
docker commit nginx update_nginx
docker images
# REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
# update_nginx     latest    ae351f794e64   5 seconds ago   141MB

# update_nginx imageからコンテナ起動
docker run -d --rm --name nginx -p 80:80 update_nginx

curl localhost:80
# -> hello world
```
