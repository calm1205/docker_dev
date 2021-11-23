## Nginx Docker Image を取得

- Docker Image を取得 (Pull)

```
docker pull nginx
```

- Docker Image を表示 (List)

```
docker images
```

- Docker Image の履歴を表示 (History)

```
docker history nginx
```

- Docker Image を削除 (Delete Docker Image)

```
docker rmi nginx

docker images
```

## Nginx Server のコンテナを起動

それでは Docker を使って、Hello World を Server から表示させてみましょう

- nginx Server のコンテナを起動 (Run)

```
$ docker run -p 80:80 --name nginx nginx
```

- 作動中のコンテナ一覧を表示 (List)

```
docker ps

# 停止中のコンテナも全て一覧表示
docker ps --all
```

- コンテナのログを表示 (Log)

```
docker logs nginx
```

- コンテナのメタデータを見てみる (Inspect Docker container)

```
docker inspect nginx
```

- Stop

```
docker stop nginx
```

- 停まったコンテナを削除 (Remove)

```
docker rm nginx
```

- コンテナをバックグラウンドで起動 `--detach`
  (Start container in background)

```
docker run --detach -p 80:80 --name nginx nginx
```

- 作動中のコンテナの中にシェルで入る (Exec) `exec -it`

```
docker exec -it nginx sh

exit
docker stop nginx
docker rm nginx
```

- コンテナの環境変数を設定する `--env TEST_ENV=hellow_world`

```
docker run --env TEST_ENV=hellow_world -d --name nginx nginx

docker exec -it nginx env
docker stop nginx
docker rm nginx
```

- コンテナに繋げるホスト側のポートを変える `-p 8080:80`

```
docker run -p 8080:80 -d --name nginx nginx

curl localhost:8080
```

- Nginx の Config ファイルを見つける

```
docker exec -it nginx sh

find / -type d -name nginx | xargs grep -r  html
```

- デフォルトの HTML ファイルを表示する

```
cat /usr/share/nginx/html/index.html
```

- デフォルトの HTML ファイルを”Hello World"へ上書き

```
echo "Hello World" > /usr/share/nginx/html/index.html

exit
curl localhost:8080
docker stop nginx
docker rm nginx
```

- コンテナにホストからファイルをコピー (attach volume) `--volume "$(pwd)":/usr/share/nginx/html`

```
touch index.html && echo “hello world” > index.html

docker run -d --volume "$(pwd)":/usr/share/nginx/html -p 80:80 --rm --name nginx nginx

curl localhost:80
```
