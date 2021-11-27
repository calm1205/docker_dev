## Docker 基本操作

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
```

- 作動中のコンテナ一覧を表示 (List)

```
docker ps
```

- 停止中のコンテナも全て一覧表示

```
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
