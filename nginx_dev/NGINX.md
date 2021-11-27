# Nginx Server

## nginx Server のコンテナを起動 (Run)

```
docker run <container_name>

docker run -d --name nginx_dev -p 80:80 nginx
```

#### オプションの説明

-d --detach と同義。プロセスをバックグラウンドで起動

-p host_port : container_port

-- name <任意のコンテナ名> コンテナ名

## 作動中のコンテナの中にシェルで入る (Exec)

```
docker exec -it <container_name> <shell>

docker exec -it nginx sh
```

## コンテナの環境変数を設定する

```
docker run --env TEST_ENV=hellow_world nginx
```

## コンテナに繋げるホスト側のポートを変える

```
docker run -p 8080:80 nginx

curl localhost:8080
```

## Nginx の Config ファイルを見つける

```
docker exec -it nginx sh

find / -type d -name nginx | xargs grep -r  html
```

デフォルトの HTML ファイルを表示する

```
cat /usr/share/nginx/html/index.html
```

デフォルトの HTML ファイルを”Hello World"へ上書き

```
echo "Hello World" > /usr/share/nginx/html/index.html

exit
curl localhost:8080
> hello world
```

## コンテナにホストからファイルをコピー (attach volume)

```
touch index.html && echo “hello world” > index.html

docker run -d --volume $(pwd):/usr/share/nginx/html -p 80:80 --rm --name nginx nginx

curl localhost:80
```
