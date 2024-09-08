---
theme: apple-basic
layout: intro
---

# 最近作ってるライブラリ怒涛の紹介！

Svelte Japan Online Meetup #4

<div class="absolute bottom-10">
  <span class="font-700">
    <a href="https://ryoppippi.com" target="_blank">@ryoppippi</a>
  </span>
</div>

---
layout: image-right
image: https://ryoppippi.com/ryoppippi.jpg
---

# Who am I?

- Svelte歴 - 4年目
- <budoux>フリーランス & 博士学生 (最近は色々お休み中)</budoux>
- UK在住
- <budoux>Webとか機械学習系のPoCを回してる</budoux>
- <budoux>好きあらばSvelte/SvelteKitを案件にぶちこむ人</budoux>
- [vim-jp](https://vim-jp.org/) にも生息してます(Neovimはいいぞ)

---
layout: statement
---

# <budoux>ここ数ヶ月の怒涛のOSSの成果を見せてやるぜ！</budoux>

---
layout: image-right
image: ./budoux.png
---

# [svelte-preprocess-budoux](https://github.com/ryoppippi/svelte-preprocess-budoux)
[![npm version](https://img.shields.io/npm/v/svelte-preprocess-budoux?color=yellow)](https://npmjs.com/package/svelte-preprocess-budoux)

### モチベーション
- 改行がおかしくなる問題を解決したい
- <budoux>[`Budoux`](https://github.com/google/budoux) を用いて、日本語を分かち書きしたいが、bundle cost は増やしたくない</budoux>

### 特徴
- `Svelte`の`proprocessor`
- <budoux>ビルド時に分かち書き、埋め込みをするので、**bundle / runtime cost free!**</budoux>
- <budoux>`word-break: auto-phrase;` が Chrome 以外でも使えるになるまでは必要そう</budoux>

---
layout: image-right
image: ./budoux.png
---

# [svelte-preprocess-budoux](https://github.com/ryoppippi/svelte-preprocess-budoux)
[![npm version](https://img.shields.io/npm/v/svelte-preprocess-budoux?color=yellow)](https://npmjs.com/package/svelte-preprocess-budoux)

```ts
/* svelte.config.js */
import {
  budouxPreprocess
} from 'svelte-preprocess-budoux';

export default {
	preprocess: [
		budouxPreprocess({ language: 'ja' }),
	],
};

```


````md magic-move
```svelte
<!-- +page.svelte -->
<p data-budoux> 本日は晴天です。明日は曇りでしょう。 </p>
```

```svelte
<!-- +page.svelte -->
<p
  style="
    word-break: keep-all; 
    overflow-wrap: anywhere;
    "
> 
  本日は 晴天です。 明日は 曇りでしょう。 
</p>
```

````

---
layout: iframe-right
url: https://sveltweet.vercel.app/async/1829286885627445584
---

# [Sveltweet](https://github.com/ryoppippi/sveltweet)
## ~~(スベるツイート)~~
[![npm version](https://img.shields.io/npm/v/sveltweet?color=yellow)](https://npmjs.com/package/sveltweet)

### モチベーション
- <budoux>Tweet をサイトに埋め込みたいが、runtime cost が気になる</budoux>

### 特徴
- [react-tweet](https://react-tweet.vercel.app/) の`Svelte`版
- <budoux>Tweet(post?) を `Svelte` に埋め込むライブラリ</budoux>
- <budoux>SSR に対応しているので、JavaScript がなくても表示可能</budoux>
- [Demo](https://sveltweet.vercel.app/)

---

# [svelte-preprocess-import-css](https://github.com/ryoppippi/svelte-preprocess-import-css)
[![JSR](https://jsr.io/badges/@ryoppippi/svelte-preprocess-import-css)](https://jsr.io/@ryoppippi/svelte-preprocess-import-css)


### モチベーション
- 外部のCSS File を読み込んで使いたいが、scoped にしたい
<!-- svelte script tagでimport css すると、scoped にならない -->
### 特徴
- Svelte Preprocessor
- <budoux> `style` タグ内で `@import` を使うことで、外部 css を scoped に読み込むことができる</budoux>


```css
/* a.css */
.message { color: blue; }

```
````md magic-move
```svelte
<!-- App.svelte -->
<div> hello </div>
<p class="message"> world </p>

<style>
@import "./a.css?.message" scoped;

div { color: green; }
</style>
```

```svelte
<!-- App.svelte -->
<div> hello </div>
<p class="message"> world </p>

<style>
.message { color: blue; }

div { color: green; }
</style>
```
````

---

# [vite-plugin-cloudflare-redirect](https://github.com/ryoppippi/vite-plugin-cloudflare-redirect)
[![JSR](https://jsr.io/badges/@ryoppippi/vite-plugin-cloudflare-redirect)](https://jsr.io/@ryoppippi/vite-plugin-cloudflare-redirect)

- `Vite` の設定から `_redirects` を生成するプラグイン
- [`Cloudflare Pages`](https://pages.cloudflare.com/) でredirectをworkerなしで行える
- `dev`/`preview`にもリダイレクトを確認できる

```ts
// vite.config.js
import { defineConfig } from 'vite'
import { cloudflareRedirect } from '@ryoppippi/vite-plugin-cloudflare-redirect'

export default defineConfig({
    plugins: [
        cloudflareRedirect({
            mode: "parse",
            entries: [
                { from: '/foo', to: 'https://example.com', status: 302 },
                // ...
            ]
        })
    ]
})
```

```
# /public/_redirects
/foo https://example.com 302
```

---

# [vite-plugin-favicon 🎨](https://github.com/ryoppippi/vite-plugin-favicons)
[![npm version](https://img.shields.io/npm/v/vite-plugin-favicons?color=yellow)](https://npmjs.com/package/vite-plugin-favicons)

- `Vite` で `favicons` や `manifest.json` を自動生成するプラグイン
- 複数のサイズのアイコンを生成
- キャッシュ機能もあるよ！


````md magic-move
```ts
// vite.config.js
export default defineConfig({
	plugins: [
		faviconsPlugin({
			imgSrc: relativePath('./src/assets/vimjp-radio-cover-art/3000x3000-fs8.png'),

			path: `/favicons`,
			theme_color: background,
			background,
			appName: VIM_JP_RADIO_INFO.title,
			appShortName: VIM_JP_RADIO_INFO.title,
			appDescription: VIM_JP_RADIO_INFO.description,
			lang: 'ja-JP',
			orientation: 'portrait',
		}),
```

```html
<link rel="icon" type="image/x-icon" href="/favicons/favicon.ico">
<link rel="icon" type="image/png" sizes="16x16" href="/favicons/favicon-16x16.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicons/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="48x48" href="/favicons/favicon-48x48.png">
<link rel="manifest" href="/favicons/manifest.webmanifest">
<meta name="mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#010a01">
<meta name="application-name" content="エンジニアの楽園 vim-jpラジオ">
<link rel="apple-touch-icon" sizes="57x57" href="/favicons/apple-touch-icon-57x57.png">
<link rel="apple-touch-icon" sizes="60x60" href="/favicons/apple-touch-icon-60x60.png">
<link rel="apple-touch-icon" sizes="72x72" href="/favicons/apple-touch-icon-72x72.png">
<link rel="apple-touch-icon" sizes="76x76" href="/favicons/apple-touch-icon-76x76.png">
<link rel="apple-touch-icon" sizes="114x114" href="/favicons/apple-touch-icon-114x114.png">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="エンジニアの楽園 vim-jpラジオ">
<meta name="msapplication-TileColor" content="#010a01">
<meta name="msapplication-TileImage" content="/favicons/mstile-144x144.png">
<meta name="msapplication-config" content="/favicons/browserconfig.xml">
<link rel="yandex-tableau-widget" href="/favicons/yandex-browser-manifest.json">
<!-- ... -->
```
````

---
layout: two-cols-header
---

# [unplugin-typia](https://github.com/ryoppippi/unplugin-typia)

[![JSR](https://jsr.io/badges/@ryoppippi/unplugin-typia)](https://jsr.io/@ryoppippi/unplugin-typia)

::left::

- <budoux> [`Typia`](https://typia.io) という TS の型をそのままValidationに使えるライブラリ</budoux>
- `unplugin` で `Vite` に組み込むことができる
- <budoux>コンパイル時にコード生成を行うので、高速かつ軽量</budoux>
- [記事も書いたので読んでね！](https://zenn.dev/ryoppippi/articles/c4775a3a5f3c11)
- 先日 v1.0.0 をリリースしました
- <budoux>`Svelte` の Script tag で `Typia` が使える！</budoux>
- [Demo](https://unplugin-typia-sveltekit.pages.dev/)
- <budoux>[`Superforms`](https://superforms.rocks/) 等のエコシステムライブラリにPRを送ってます</budoux>
- <budoux>みんな使ってね！</budoux>


::right::

```svelte
<script lang="ts">
  import typia, { type tags } from "typia";

  interface IMember {
    email: string & tags.Format<"email">;
    age: number & tags.Maximum<100>;
  }

  const check = typia.createIs<IMember>();
  let isValid = $derived(check(member));

  let member = $state<IMember>({
    email: "samchon.github@gmai19l.com",
    age: 20
  });
</script>

<input type="text" id="email" bind:value={member.email} />
<h1> {isValid ? "Valid" : "Invalid"} </h1>
<div> <p>{member.id}</p> </div>

```

---
layout: intro-image
image: ./vim-jp-radio.png
---

<div class="absolute bottom-50">
  <h2><a href="https://vim-jp-radio.com/" target="_blank">エンジニアの楽園 vim-jp ラジオ</a></h2>
  <p>毎週月曜 昼12時配信</p>
  <p class="text-size-3"><a href="https://zenn.dev/vim_jp/articles/e1192d17156a2d" target="_blank">制作記事はこちら(made w/ SvelteKit)</a></p>
</div>

---
layout: end
---

# ご清聴ありがとうございました！
