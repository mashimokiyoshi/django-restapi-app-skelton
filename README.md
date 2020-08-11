# django-restapi-app-skelton

参考：[DockerでReact + Django + Postgresの連携・SPA構築チュートリアル](https://hodalog.com/tutorial-django-rest-framework-and-react/)

## バックエンド側のセットアップ

clone後に以下のコマンドを順に実行

```
# コンテナの起動
docker-compose up -d

# DBのセットアップ
docker container exec -it django_db psql -U postgres

# psql内で以下のSQLを実行
CREATE DATABASE django_app;
CREATE USER admin WITH PASSWORD 'password';
GRANT ALL PRIVILEGES ON DATABASE django_app TO admin;

# DBのマイグレーション
docker container exec -it django_rest_api python django_app/manage.py makemigrations

docker container exec -it django_rest_api python django_app/manage.py migrate

docker container exec -it django_rest_api python django_app/manage.py createsuperuser


# 動作確認
docker container exec -it django_rest_api python django_app/manage.py runserver 0:8000
```

#### localhost:8888/admin にアクセス
#### マイグレーション時に設定したユーザ名、パスワードでログインできれば成功

## フロントエンド側のセットアップ

- ### Reactの場合
```
docker container exec -it django_web_front npx create-react-app .

docker container exec -it django_web_front npm start
```

- ### Vueの場合
```
docker container exec -it django_web_front npm install -g @vue/cli

docker container exec -it django_web_front vue create .

docker container exec -it django_web_front touch vue.config.js

# 上記のファイルに以下の内容を書き込む
module.exports = {
    devServer: {
        port: 8888,
        disableHostCheck: true,
    },
};

docker container exec -it django_web_front npm run serve

```



#### localhost:3333 にアクセスしてそれぞれの画面が表示されればOK