# 都道府県別の総人口推移グラフを表示するSPA

> Netlify
> https://competent-mestorf-690b18.netlify.app/

## 内容
1. RESAS(地域経済分析システム) APIの「都道府県一覧」からAPIを取得する
2. APIレスポンスから都道府県一覧のチェックボックスを動的に生成する
3. 都道府県にチェックを入れると、RESAS APIから選択された都道府県の「人口構成」を取得する
4. 人口構成APIレスポンスから、X軸:年、Y軸:人口数の折れ線グラフを動的に生成して表示する

## 制約
-React/Vue.js/Angularのいずれかを用いてSPAを構築すること（バージョンはできるだけ最新版をご使用ください）
React（ https://reactjs.org/ ）
Vue（ https://vuejs.org/index.html ）
Angular（ https://angular.io/ ）Nuxt.jsやNext.jsなどの、これらを内包したフレームワークの利用も許可する
Nuxt.jsやNext.jsなどの、これらを内包したフレームワークの利用も許可する

-都道府県一覧および総人口情報はRESAS APIのデータを用いること
-グラフは Highcharts や Rechart.js などのサードパーティ製のグラフライブラリを用いて描画すること
ただし、グラフライブラリは上記のものに限らず、任意のものを用いてよい
-Google Chrome最新版で正しく動くこと
-PC/スマートフォン表示に対応すること(レスポンシブデザイン対応)
ただし実機でなく、Google Chromeの検証ツールで確認できればよい
-リンターやフォーマッターを適切に設定すること
リンターにはESLint、フォーマッターにはPrettierを使用すること
-styleは自分で記述し、CSS・UIフレームワークなどは原則使用しないこと
ただし、chartライブラリ内包のstyle、リセット系のCSSライブラリについてはこの限りではない
また、css-in-jsやcss-modules、sassなどのエコシステムの利用を妨げるものではなく、あくまでcssの記述力を測る趣旨に留まる
-リードエンジニア・テックリード応募の場合は、以下も制約に加える
TypeScriptで記述すること
-テストケース/テストコードを作成すること
テストツールは任意のものを用いてよい
テスト実行時にエラーが発生しないこと

## 参考
-RESAS API: https://opendata.resas-portal.go.jp/
-RESAS API仕様書: https://opendata.resas-portal.go.jp/docs/api/v1/index.html
-Highcharts: https://www.highcharts.com/


## Build Setup

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

For detailed explanation on how things work, check out the [documentation](https://nuxtjs.org).

## Special Directories

You can create the following extra directories, some of which have special behaviors. Only `pages` is required; you can delete them if you don't want to use their functionality.

### `assets`

The assets directory contains your uncompiled assets such as Stylus or Sass files, images, or fonts.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/assets).

### `components`

The components directory contains your Vue.js components. Components make up the different parts of your page and can be reused and imported into your pages, layouts and even other components.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/components).

### `layouts`

Layouts are a great help when you want to change the look and feel of your Nuxt app, whether you want to include a sidebar or have distinct layouts for mobile and desktop.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/layouts).


### `pages`

This directory contains your application views and routes. Nuxt will read all the `*.vue` files inside this directory and setup Vue Router automatically.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/get-started/routing).

### `plugins`

The plugins directory contains JavaScript plugins that you want to run before instantiating the root Vue.js Application. This is the place to add Vue plugins and to inject functions or constants. Every time you need to use `Vue.use()`, you should create a file in `plugins/` and add its path to plugins in `nuxt.config.js`.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/plugins).

### `static`

This directory contains your static files. Each file inside this directory is mapped to `/`.

Example: `/static/robots.txt` is mapped as `/robots.txt`.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/static).

### `store`

This directory contains your Vuex store files. Creating a file in this directory automatically activates Vuex.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/store).
