# Docker Networking

## Network Namespace

実はコンテナが起動する時、裏では Network Namespace が作られそこにコンテナが割り当てられます。

異なる Network Namespace 同士は通信できませんが、Docker はデフォルトで Bridge ネットワークを作り個々を繋げています。

しかし、Docker-compose で起動したコンテナは新しい Bridge ネットワークの中に作られ、

デフォルト Bridge ネットワークの中のコンテナとは接続が取れないのです

```bash
docker-compose up
# [+] Running 4/4
# ⠿ Network 6_default                 Created
```

Network の確認

```bash
docker network ls
# NETWORK ID     NAME                    DRIVER    SCOPE
# 2a8811276151   6_default               bridge    local

# curlコンテナにshで入る
docker run --rm -it --name curl curlimages/curl sh

# 疎通確認
curl docker_compose_nginx:80
# -> curl: (6) Could not resolve host: docker_compose_nginx
```

ネットワークと IP レンジの確認

```bash
docker inspect docker_compose_nginx | grep -i network
# -> "NetworkMode": "6_default",
docker inspect docker_compose_nginx | grep -i ipaddress
# -> "IPAddress": "172.19.0.4",

# curlコンテナを再度起動
docker run -d --rm --name curl curlimages/curl sleep 500

docker inspect test | grep -i network
# -> "NetworkMode": "default",

docker inspect test | grep -i ipaddress
# -> "IPAddress": "172.17.0.2",
```

## Docker ネットワークを確認

Docker network には 4 つモードがある

デフォルトの Bridge、host、null、overlay

- ネットワークを表示する

```
docker network ls
```

- ネットワークを--driver オプションで作成する

```
docker network create --driver bridge custom_bridge
```

- ネットワークを削除する

```
docker network rm custom_bridge
```

### None network

None モードの場合、コンテナは Bridge にも Host ネットワークにもリンクされず IP もないので、接続不可

```
docker run --network=none --rm -d --name none -p 80:80 nginx
docker inspect none

curl localhost:80
```

### Bridge network (default)

Bridge モードの場合、ホストの中の DockerBridge ネットワークから IP が振り分けられるので、IP レンジがホストと異なる

```
docker run --rm -d --name bridge -p 81:80 nginx
docker inspect bridge

curl localhost:81
```

### Host network

Host モードの場合、ホストの IP レンジから IP が振り分けら、ホストポートを使う

```
docker run --network=host --rm -d --name host  -p 82:80 nginx
docker inspect host

# for Mac, it doesn't work
# ref: https://forums.docker.com/t/should-docker-run-net-host-work/14215
curl localhost:81
```
