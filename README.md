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

## Laravelをインストールする
```shell
$ composer create-project "laravel/laravel=5.5.*" app
```

これで、Laravelを起動するための環境と、Laravelのインストールが完了しました。