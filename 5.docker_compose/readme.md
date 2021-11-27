# docker compose

## docker compose とは

`docker run`コマンドで docker container を立ち上げていたのを yaml 形式のファイルで定義が可能

以下の`docker run`コマンドと `docker-compose.yaml`は同義

```bash
docker run -d --name new_nginx --env="KEY=VALUE" -p 80:80 --volume $(pwd):/usr/share/nginx/html nginx
```

docker-compose.yaml

```yaml
varsion: "3.7"
  services:
    nginx:
      image: nginx:latest
      container_name: new_nginx
      environment:
       - KEY=VALUE
      ports:
       - "80:80"
      volumes:
       - ${pwd}:/usr/share/nginx/html
```

## docker compose CLI

docker compose を用いた container 起動

```bash
docker compose up
# コマンドを実行したディレクトリに有るdocker-composeファイルを参照
# docker compose -d up バックグラウンド起動
```

docker compose で立ち上げた container の削除

```bash
docker compose down
```

## レプリカ

指定した container を複数起動することが可能

yaml ファイルを以下のように修正

```yaml
version: "3.8"
services:
  nginx:
    image: nginx:latest
    environment:
      - env_key=env_value
    ports:
      - "80-85:80" # portを固定で指定すると重複してしまうのでレンジ指定
    volumes:
      - ${PWD}:/usr/share/nginx/html:ro
  curl:
    image: curlimages/curl:7.68.0
    container_name: docker_compose_curl
    command: ["sleep", "500"]
    ports:
      - "8080:8080"
  busybox:
    image: busybox
    container_name: docker_compose_busybox
    command: ["sleep", "1500"]
```

#### nginx を 3 つ起動

```bash
# nginx containerを3つ作成
docker compose -f docker-compose.rep.yaml up --scale nginx=3
```
