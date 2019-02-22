# yukure/laravel-docker
このリポジトリには、Laravel5.xを動かすための
- php7.2
- nginx
- mysql5.7
の docker イメージが含まれています。

## 事前準備
- [Docker Desktop](https://www.docker.com/products/docker-desktop) をインストール済み。
- git コマンドが叩ける。
- [composer](https://getcomposer.org/)をインストール済み

## セットアップ手順
Laravelをインストールするまでのセットアップ手順を順番に説明します。

### 環境の構築
まず、ディレクトリを作成して、そのディレクトリに移動します。
※ディレクトリ名は好きな名前でOKです。

```shell
$ mkdir laravel-docker
$ cd laravel-docker
```

そして、作成したディレクトリ内で`laravel-docker`をgithubからクローンします。

```shell
$ git clone git@github.com:yukure/laravel-docker.git
```

### コンテナの起動
docker-compose コマンドを使用してコンテナの作成と起動を行います。

```shell
$ docker-compose up -d
```

## phpコンテナに入る
`exec`コマンドを使用して、コンテナ内に入ります。
```shell
docker-compose exec php bash
```

## Laravelをインストールする
```shell
$ laravel new
```

これで、Laravelを起動するための環境と、Laravelのインストールが完了しました。

## ブラウザでの確認
ブラウザで、[localhost](http://localhost/)にアクセスして↓のページが表示されたらOKです！
![Laravel Top Page](https://user-images.githubusercontent.com/18342796/53179719-a02e4980-3637-11e9-90a5-a5490e60ac7d.png)
