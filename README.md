# yukure/laravel-docker
このリポジトリには、Laravel5.xを動かすための
- php7.2
- nginx
- mysql5.7
の docker イメージが含まれています。

## 備考
※ Macでの仕様を前提としています。
Windowsではパスの指定が違ってくるため、クローンしてきた状態だとうまくコンテナの作成を行ってくれません。

## 事前準備
- [Docker Desktop](https://www.docker.com/products/docker-desktop) をインストール済み。
- git コマンドが叩ける。

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

## Laravel のセットアップ

ページを表示することはできましたが、まだセットアップは終わってません。
とりあえず、タイムゾーンと言語の設定しておきましょう。
これをしておかないと、ログに出力される日時や、DB Recordのcreated_at/updated_atなどが9時間前ずれてしまったり、バリデーションメッセージが英語になってしまいます。

## タイムゾーンと言語の設定
```diff
// config/app.php
    
- 'timezone' => 'UTC',
+ 'timezone' => 'Asia/Tokyo',

...

- 'locale' => 'en',
+ 'locale' => 'ja',
```

これでOKです。

## mysql の設定

まず、mysqlのコンテナに入ります。

```shell
# コンテナに入る
docker-compose exec db bash

# mysqlにrootユーザでログインする
$ mysql -u root -p

# パスワード聞かれるので root と入力
Enter password:

# ログイン完了！！
mysql>
```

ログインできたら、DBの作成をします。

```
# laravel という名前のDB作成
mysql> create database laravel;

# 新しいユーザー作成
mysql> CREATE USER 'laravel'@'%' IDENTIFIED WITH mysql_native_password BY 'laravel';

# 作ったユーザに権限付与
GRANT ALL PRIVILEGES ON laravel.* TO 'laravel'@'%'; FLUSH PRIVILEGES;

# 一旦、ログアウト
mysql> exit

# 先程作ったユーザでログイン (passwordはlaravel)
$ mysql -u laravel -p

# データベース一覧表示
mysql> show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| laravel            |
+--------------------+

# mysqlからログアウト
mysql> exit

# コンテナから抜ける
$ exit
```

これでmysqlにDB作って、それにアクセスできるユーザの作成をしました。

## .envの編集

次は、`.env`の編集をします。
.envとは、環境変数が格納されているファイルです。

laravelの各種設定の値が一覧になっているファイルです。
このファイルの値を変更すると、laravelのアプリケーション自体に変更がおきるので、取扱は慎重に！

```diff
# 接続先のDBと接続する際のユーザの変更
-DB_HOST=127.0.0.1
+DB_HOST=db
DB_PORT=3306
- DB_DATABASE=homestead
+ DB_DATABASE=laravel
- DB_USERNAME=homestead
+ DB_USERNAME=laravel
- DB_PASSWORD=secret
+ DB_PASSWORD=laravel
```

## 認証機能(ログイン機能)の実装

webサービスなどでよくある、ログイン機能をlaravelではコマンド１発で（正確には２発）実装できます。

```
# php のコンテナに入る
$ docker-compose exec php bash

# 認証機能実装(マイグレーションファイル作成やルートの定義などの設定)
php artisan make:auth

# マイグレーション実行
php artisan migrate
```
