# tutorial

NuxtjsでAPIデータの操作を学ぶために簡単に作った英語版のギャグサイト。

こちらのプロジェクトは[Nuxt JS Crash Course](https://www.youtube.com/watch?v=ltzlhAxJr74)のチュートリアルを勉強したものになる。

NuxtはVueのフレームワークで、Vueを基盤に開発されている。<b>Nuxtは自動でルーティングが設定されるのでVueとは違いわざわざ`router`を用意する必要はない。</b>

これはあくまで一個人の感想に過ぎないが、何気なくプロトタイプ開発の質や効率を向上させている。

# 設定

まずは、以下のコマンドを入力する。

```
npx create-nuxt-app tutorial
```

入力後、Nuxtプロジェクトの基本的な設定は以下の通り。

* Package Manager => npm
* Programming language => JavaScript
* UI Framework => None
* Nuxt Modules => Axios - Promise based HTTP Client
* Linting Tools => None
* Testing framework => None
* Rednering mode => Universal
* Deployment target => Server
* Development tools => jsconfig.json
* Version => Git

以上の内容を選択し終えた後、以下のコマンドを入力して仮想サーバを立ち上げる。

```
cd tutorial
npm run dev
```

これでNuxtのインストールは終了。

# Aboutページの作成

`pages/about.vue`を作成し、以下のプログラムを書く。

```html
<template>
    <div>
       <h1>About Page</h1>
       <p>This is a sample page made in Nuxt.</p>
    </div>
</template>

<script>
export default {
    head() {
        return {
            title: 'About App',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }
}
</script>

<style>
    
</style>
```

# ヘッダーの作成

新しく`components/AppHeader.vue`を作成。

```html
<!---components/AppHeader.vue--->
<template>
    <div>
        <header class="header">
            <h1 class="title">Sample Page</h1>
            <ul>
                <li>
                    <nuxt-link to="/">Home</nuxt-link>
                </li>
                <li>
                    <nuxt-link to="/jokes">Jokes</nuxt-link>
                </li>
                <li>
                    <nuxt-link to="/about">About</nuxt-link>
                </li>
            </ul>
        </header>
    </div>
</template>

<script>
export default {
   name: 'AppHeader', 
}
</script>

<style>
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
    padding-bottom: 1rem;
    border-bottom: 1px dotted #ccc;
}
.header .title {
    font-size: 3rem;
    color: #526488;
}
.header ul {
    display: flex;
}
.header a {
    display: inline-block;
    background: #333;
    color: #fff;
    padding: 0.3rem 1rem;
    margin-right: 0.5rem; 
}
</style>
```

`pages/index.vue`に戻る。

```html
<template>
  <div>
    <h1>Welcome to My Sample Page.</h1>
  </div>
</template>

<script>
export default {
    head() {
        return {
            title: 'Welcome to Sample Page',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }
}
</script>
```

`pages`フォルダの配下に新しく`jokes`フォルダを作成し、配下に`index.vue`を作成。

```html
<template>
    <div>
       <h1>This is Joke Page.</h1> 
    </div>
</template>

<script>
export default {
    head() {
        return {
            title: 'Welcome to Sample Page',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }    
}
</script>

<style>
    
</style>
```

# トップページの作成

`layouts/default.vue`を以下のプログラムに書き換える。

```html
<template>
    <div class="container">
        <AppHeader />
        <nuxt />
    </div>
</template>

<script>
import AppHeader from '../components/AppHeader.vue'
export default {
    components: {
        AppHeader
    }
}
</script>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
body {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 1rem;
    line-height: 1.6;
    background: #f4f4f4;
}
a {
    color: #666;
    text-decoration: none;
}
ul {
    list-style: none;
}
.container {
    max-width: 800px;
    margin: 2rem auto;
    overflow: hidden;
    padding: 1rem 2rem;
    background: #fff;
}
</style>
```

## 要点

* Nuxtでリンク先を設定する際には、`<nuxt-link>`タグを活用する。
* コンポーネントとして設定する際には、テンプレートの名前はファイル名と必ず同じにする。(ファイル構造を理解しやすくするため)
* もともとある`components/NuxtLogo.vue`と`components/Tutorials.vue`は削除。
* `pages/index.vue`ファイルの中身を別のファイルに反映させるためには、`<nuxt>`タグを活用する。__この際、`components`にテンプレートの名前を記す必要はない。__

# API活用

本プロジェクトで活用するAPIは[Bad Dad Jokes](https://icanhazdadjoke.com/:title)。

これは、REST APIの簡単な操作方法を初心者にもわかりやすく詳細に解説してあるチュートリアル用のAPIである。

本プロジェクトでは、

* 一覧の表示
* ギャグの詳細表示
* ギャグの検索機能

これら３つの機能をNuxtjsで実装する。

`pages/jokes/index.vue`を開く。

```html
<template>
    <div>
       <h1>This is Joke Page.</h1> 
    </div>
</template>

<script>
export default {
    data() {
        return {
            jokes: []
        }
    },
    async created() {
        const config = {
            headers: {
                'Accept': 'application/json'
            }
        }
        try {
            // これでAPIからデータを取得できる
            const res = await axios.get('https://icanhazdadjoke.com/search', config)
            this.jokes = res.data.results
        } catch (error) {
            console.log(error)
        }
    },
    // Nuxtではこのように書くことで、HTMLファイルのタイトルや要素を簡単に変えられる。 
    head() {
        return {
            title: 'Welcome to Sample Page',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }    
}
</script>

<style>
    
</style>
```

`components/Jokes.vue`を新しく作成する。

```html
<template>
    <div class="joke">
        <p>{{ joke }}</p>
    </div>
</template>

<script>
export default {
    name: 'Jokes',
    props: ['joke', 'id']
}
</script>

<style>
.joke {
    padding: 1rem;
    border: 1px dotted #ccc;
    margin: 1rem 0;
}
</style>
```

`pages/jokes/index.vue`を修正する。

```html
<template>
    <div>
        <!---APIから取り出したデータを順番に抽出する--->
        <Jokes v-for="joke in jokes" :key="joke.id" :id="joke.id" :joke="joke.joke" />
    </div>
</template>

<script>
import axios from 'axios'
import Jokes from '../../components/Jokes.vue'

export default {
    components: {
        Jokes
    },   
    data() {
        return {
            jokes: []
        }
    },
    async created() {
        const config = {
            headers: {
                'Accept': 'application/json'
            }
        }
        try {
            const res = await axios.get('https://icanhazdadjoke.com/search', config)
            this.jokes = res.data.results
        } catch (error) {
            console.log(error)
        }
    },  
    head() {
        return {
            title: 'Welcome to Sample Page',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }    
}
</script>

<style>
    
</style>
```

# 詳細ページの表示

詳細ページには、詳細を見たい要素にクリックすることで表示させるようにした。

`components/Jokes.vue`を開く。プログラムを修正。

```html
<template>
    <nuxt-link :to="`jokes/${id}`">
        <div class="joke">
            <p>{{ joke }}</p>
        </div>
    </nuxt-link>
</template>

<script>
export default {
    name: 'Jokes',
    props: ['joke', 'id']
}
</script>

<style>
.joke {
    padding: 1rem;
    border: 1px dotted #ccc;
    margin: 1rem 0;
}
</style>
```

Nuxtで詳細ページを開くには、<b>別途`_id`ファイルを用意する必要がある。</b>

`pages/jokes/_id/index.vue`を開く。

```html
<template>
    <div>
        <nuxt-link to="/jokes">Back to Jokes</nuxt-link>
        <h2>{{ joke }}</h2>
        <hr />
        <!---Nuxtでは、IDを表示する際には$route.params.idと表記する必要がある。--->
        <small>Joke ID: {{ $route.params.id }}</small>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            joke: {}
        }
    },
    async created() {
        const config = {
            headers: {
                'Accept': 'application/json'
            }
        }
        try {
            const res = await axios.get(`https://icanhazdadjoke.com/j/${this.$route.params.id}`, config)
            this.joke = res.data.joke
        } catch (error) {
            console.log(error)
        }
    },
    head() {
        return {
            title: this.joke,
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }
}
</script>

<style>
    
</style>
```

これで詳細ページを開くことができる。

# 検索機能の実装

`components/SearchJokes.vue`を開いて、以下のコードを書く。

```html
<template>
    <form @submit.prevent="onSubmit">
        <input type="text" placeholder="Search Jokes" v-model="text">
        <input type="submit" value="Search Jokes">
    </form>
</template>

<script>
export default {
    name: 'SearchJokes',
    data() {
        return {
            text: ''
        }
    },
    // Nuxt(Vue)で文字列を検索するには$emitメソッドを活用する
    methods: {
        onSubmit() {
            this.$emit('search-text', this.text)
            this.text = '' // 変数データの初期化も忘れずに
        }
    }
}
</script>

<style>
    
</style>
```

`pages/jokes/index.vue`を開く。

```html
<template>
    <div>
        <SearchJokes v-on:search-text="searchText" /> <!--追加-->
        <Jokes v-for="joke in jokes" :key="joke.id" :id="joke.id" :joke="joke.joke" />
    </div>
</template>

<script>
import axios from 'axios'
import Jokes from '../../components/Jokes.vue'
import SearchJokes from '../../components/SearchJokes.vue' <!--追加-->

export default {
    components: {
        Jokes
    },   
    data() {
        return {
            jokes: []
        }
    },
    // 修正
    methods: {
       async searchText(text) {
           const config = {
            headers: {
                'Accept': 'application/json'
            }
        }
        try {
            const res = await axios.get(`https://icanhazdadjoke.com/search?term=${text}`, config)
            this.jokes = res.data.results
        } catch (error) {
            console.log(error)
        } 
       } 
    }, 
    head() {
        return {
            title: 'Welcome to Sample Page',
            meta: [
                {
                    hid: 'description',
                    name: 'description',
                    content: 'Bast Place for sample Page'
                }
            ]
        }
    }    
}
</script>

<style>
    
</style>
```

これで実装は終了。基本的にはREST APIは`GET`メソッドしか使っていないが、それなりのプロジェクトを開発できた。

# 開発環境

* Nuxt.js 2.15.8
* Visual Studio Code 1.63 
