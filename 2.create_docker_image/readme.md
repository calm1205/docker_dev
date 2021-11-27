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

**Dockerfile でレシピの作成**

```sh
# Dockerfileを作成
touch Dockerfile
```

```sh
＃　Dockerfileにマルチラインテクストを保存

cat > Dockerfile <<EOF
# FROMでイメージを指定
FROM nginx:latest

WORKDIR /usr/share/nginx/html

# COPYでホストからファイルをイメージにコピー
COPY index.html index.html

# RUNでシェルコマンドを実行
RUN apt update && apt install -y curl

# 他にもWORKDIR,ENV,CMDなど変数があります
EOF
```

実はドッカーイメージは、この Dockerfile の各ラインが追加されるごとに、薄い ReadOnly イメージレイヤーを追加しているんですね.例えで言うと、玉ねぎの層が増す感じです

その証拠に、docker build コマンドのアウトプットで、各コマンドステップで新たなコンテナイメージ層が作られているのが確認できます

- Dockerfile からイメージ作成 (build)

```
docker build --tag dockerfile_hello .

# 自作イメージからコンテナを起動
$ docker run -d -p 9090:80 --name hello_v2 dockerfile_hello

curl localhost:9090
```

イメージレイヤーは Dockerfile で指定したコマンドと一致しています. つまり、Dockerfile の１コマンドごとに ReadOnly イメージレイヤーが上乗せされていくんです.

- ドッカーイメージの履歴を表示 (History)

```
docker history dockerfile_hello
```

## 3.4 自作イメージをリポで公開

ドッカーイメージを作ったら、あとはタグ付けをして、DockerHub などのコンテナレジストリーにプッシュをします

まずは Dockerhub の無料アカウント作成：　https://hub.docker.com/signup

- 作成したイメージをタグ付けする（ユーザーネーム/リポ名）

```
docker tag dockerfile_hello cscareerkaizen/dockerfile_hello_world_nginx
```

- Docker Hub にログイン

```
docker login
```

- DockerHub の自分のリポにイメージをアップロード

```
docker push cscareerkaizen/dockerfile_hello_world_nginx
```

## コンテナ・イメージのクリーンアップ

- 停止中のコンテナと無名のイメージを削除

```
docker system prune
```

- 起動中のコンテナ含め全コンテナを削除(注意)

```
docker rm -vf $(docker ps -a -q)
```

- 全てのイメージを削除(注意)

```
docker rmi -f $(docker images -a -q)
```

---

NEXT > [4_Multi_Containers_with_docker_compose](../4_Multi_Containers_with_docker_compose/README.md)
