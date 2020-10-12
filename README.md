# Electron と Vue.js でデスクトップアプリ開発を始める

## 実施環境
- Windows 10 Home 64bit バージョン 1909（OS ビルド 18363.1082）
- Node.js v14.11.0

## 概略
1. Vue CLI で Vue.js プロジェクトを作成
2. electron-builder プラグイン で

## Vue CLI
npm もしくは、yarn を使って Vue CLI をインストールする。
```console
npm i -g @vue/cli
# OR
yarn global add @vue/cli
```

インストールできたか確認する。
```console
vue --version
```
実行結果:
```
@vue/cli 4.5.7
```

Vue CLI は upgrade コマンドで最新バージョンに更新できる。
```console
yarn global upgrade --latest @vue/cli
```

## Vue.js プロジェクトの作成
Vue.js プロジェクトを作成する。
```console
vue create my-project
```

### 補足: 実行バイナリについて
Vue CLIプ ロジェクト内に、@vue/cli-service という名前のバイナリがインストールされる。場所は、./node_modules/.bin/vue-cli-service でターミナルからバイナリにアクセスできる。

実行されるのは、./node_modules/@vue/cli-service/bin/vue-cli-service.js ファイル。

package.json には、`serve` コマンドと `build` コマンドが定義されている。

```json
{
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  }
}
```

## プロジェクトの実行
プロジェクト直下のディレクトリに移動する。
```console
cd ./my-project
```

npm もしくは yarn を使って vue-cli-service を呼び出す。
```console
npm run serve
# OR
yarn serve
```

NPX（npm に付属）を使う場合は、直接バイナリを呼び出せる。
```console
npx vue-cli-service serve
```

コンパイルが完了し、アプリが起動したら、Web ブラウザで以下のアドレスにアクセスする。
```
http://localhost:8080/
```

Welcome ページが表示されていれば、確認 OK。Ctr + C でアプリを停止できる。

## electron-builderプラグイン
electron-builder プラグインをインストール する。
```console
vue add electron-builder
```

この時点で git init される。

## 実行とアプリケーションのビルド
Web アプリとして実行する。
```console
npm run serve
# OR
yarn serve
```

デスクトップアプリとして実行する。
```console
npm run electron:serve
# OR
yarn electron:build
```

コマンド実行後に、dist_electron ディレクトリ内にアプリのインストーラーが作成されていればビルド成功。

Mac用: your-app-name-1.0.0.dmg
Windows用: your-app-name Setup 1.0.0.exe

このインストーラーを起動すれば、アプリが PC にインストールされて起動する。

# Laravel 8 Rest API CRUD を Passport 認証で

Passport 認証のチュートリアルで、Laravel 8 による Rest API を使った CRUD を学習する。このチュートリアルでは、Laravel 8 で Passport 認証を使用して RESTful CRUD API を作成する方法を学習する。Passport 認証は通常、デジタル署名を使用して信頼および検証できる情報を送信するために使用される。RESTful API では、アクションとしてHTTP メソッドを使用し、エンドポイントは作用を受けるリソースです。その意味のために HTTP 動詞を使用している。

- GET：リソースを取得する
- POST：リソースを作成する
- PUT：リソースを更新します
- 削除：リソースを削除します

それでは、LaravelAPIを段階的に使用する方法を見てみましょう。パスポート認証を使用したLaravel8のRESTfulAPI。また、APIを使用したユーザー製品の完全に機能するCRUDも紹介します。

- Login API
- Register API
- GetUser Info API
- Product List API
- Create Product API
- Edit Product API
- Update Product API
- Delete Product API

## 実施環境
- Windows 10 Home 64bit バージョン 1909（OS ビルド 18363.1082）
- Node.js v14.11.0

## Step 0: ワークスペースの確保
チュートリアルを行うために今回は、デスクトップにワークスペースを用意する。以降のコマンドは、Git Bash で実行する。

デスクトップに移動する。
```console
cd /c/Users/＜ユーザ名>/Desktop
```

作業フォルダ workspace.test.laravel8.api を作成する。
```console
mkdir ./workspace.test.laravel8.api
```

作業フォルダ内に移動する。
```console
cd ./workspace.test.laravel8.api
```

## Step 1: Download Laravel 8 App
Laravel 8 プロジェクトを作成する。
```console
composer create-project --prefer-dist laravel/laravel my-laravel8-api-proj
```

プロジェクト内に移動する。
```console
cd ./my-laravel8-api-proj
```

## Step 2: Database Configuration
次に、パスポートチュートリアルプロジェクトを使用して、インストールしたlaravel8のRESTful認証APIのルートディレクトリに移動します。そして、.env ファイルを開きます。次に、データベースの詳細を次のように追加します。

```
DB_CONNECTION=mysql
DB_HOST=192.168.99.100
DB_PORT=13306
DB_DATABASE=products
DB_USERNAME=fsedu
DB_PASSWORD=secret
```

## Step 3: Install Passport Auth
passport パッケージをインストールする。
```console
composer require laravel/passport
```

passport パッケージをインストールしたら、プロバイダーを追加する。

config / app.php:
```php
'providers' =>[
    Laravel\Passport\PassportServiceProvider::class,
],
```

一度、マイグレーションを実行しておく。
```console
php artisan migrate
```


次に、パスポート暗号化キーを生成するためにlaravel8をインストールする必要があります。このコマンドは、安全なアクセストークンを生成するために必要な暗号化キーを作成します。
```console
php artisan passport:install
```

## Step 4: Passport Configuration
App/Models/User.php を編集する。
```php
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
```diff
+ use Laravel\Passport\HasApiTokens;
```
class User extends Authenticatable
{
    use HasApiTokens, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
}
```

