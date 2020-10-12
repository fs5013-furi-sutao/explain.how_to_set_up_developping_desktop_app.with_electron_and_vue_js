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

```diff
<?php

namespace App\Models;

use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

+ use Laravel\Passport\HasApiTokens;

class User extends Authenticatable
```

