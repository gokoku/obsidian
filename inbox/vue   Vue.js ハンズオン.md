#js/vue 

---
2021-06-21

# 第1章 Vue.js を理解しよう

## Step 01 Hello World

```html
<!DOCTYPE html>
<html lang="en">
<meta charset="UTF-8">
<title>hello world</title>
<div id="app">
  <p>{{ display }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@3.0.0/dist/vue.global.js"></script>
<script>
    const app = Vue.createApp({
      data: () => ({
        display: 'Hello World!'
      })
    })
    app.mount("#p")
</script>
</html>
```
![[Pasted image 20210621134729.png]]

## Step 02　フォームの双方向バインディング

双方向バインディング。

Grid の使い方がすごい。こう使うのか。

```html
<!DOCTYPE html>
<html lang="en">
<meta charset="UTF-8">
<title>データの入力</title>
<style>
  .input-container {
    display: grid;
    grid-template-rows: 200px 50px;
    grid-template-columns: 300px 300px;
    grid-template-areas: 
	    "inputArea previewArea"
	    "buttonArea buttonArea";
    grid-gap: 10px;
  }
  .inputArea {
    grid-area: inputArea;
    display: grid;
  }
  .previewArea {
    grid-area: previewArea;
    display: grid;
  }
  .buttonArea {
    grid-area: buttonArea;
    display: grid;
    justify-items: center;
    align-items: center;
  }
</style>
<div id="app">
  <div class="input-container">
    <form class="inputArea">
      <p>ユーザー登録</p>
      <label>ニックネーム</label>
      <input v-model="nickname">
      <label>メールアドレス</label>
      <input v-model="email">
    </form>
    <div class="previewArea">
      <p>プレビュー</p>
      <label>ニックネーム</label>
      <input v-bind:value="nickname" readonly>
      <label>メールアドレス</label>
      <input v-bind:value="email" readonly>
    </div>
    <div class="buttonArea">
      <button v-on:click="saveUser">ユーザーを登録する</button>
    </div>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@3.0.0/dist/vue.global.js"></script>
<script>
  const app = Vue.createApp({
    data: () => ({
      users: [],
      nickname: '',
      email: '',
    }),
    methods: {
      saveUser: function() {
        let user = {
          nickname: this.nickname,
          email: this.email,
        }
        this.users.push(user)
        alert('ニックネーム: ' + this.nickname + '、メールアドレス: ' + this.email + 'で登録しました。')
        console.log(this.nickname + " : " + this.email)
      }
    }
  })
  app.mount('#p')
</script>

</html>
```

![[Pasted image 20210621145822.png]]



## Step 03　登録データ表示

登録データを表示するアプリケーションを作る。

絞り込みを設ける。

算出プロパティを使うと、キャッシュが効いて余計な更新をしない。

```html
<!DOCTYPE html\>

<html lang\="en"\>

<meta charset\="UTF-8"\>

<title\>データの入力</title\>

<style\>

.input-container {

display: grid;

grid-template-rows: 200px 50px;

grid-template-columns: 300px 300px;

grid-template-areas:

"inputArea previewArea"

"buttonArea buttonArea";

grid-gap: 10px;

}

.inputArea {

grid-area: inputArea;

display: grid;

}

.previewArea {

grid-area: previewArea;

display: grid;

}

.buttonArea {

grid-area: buttonArea;

display: grid;

justify-items: center;

align-items: center;

}

.filterArea {

display: grid;

grid-template-columns: 300px 300px;

grid-gap: 10px;

}

table {

margin-top: 10px;

width: 610px;

border-collapse: collapse;

}

th, td {

width: 50%;

padding-left: 5px;

word-break : break-all;

}

</style\>

<div id\="app"\>

<div class\="input-container"\>

<form class\="inputArea"\>

<p\>ユーザー登録</p\>

<label\>ニックネーム</label\>

<input v-model\="nickname"\>

<label\>メールアドレス</label\>

<input v-model\="email"\>

</form\>

<div class\="previewArea"\>

<p\>プレビュー</p\>

<label\>ニックネーム</label\>

<input v-bind:value\="nickname" readonly\>

<label\>メールアドレス</label\>

<input v-bind:value\="email" readonly\>

</div\>

<div class\="buttonArea"\>

<button v-on:click\="saveUser"\>ユーザーを登録する</button\>

</div\>

<div\>

<div class\="filterArea"\>

<label\>リストをニックネームで絞り込む</label\>

<input v-model\="nicknameFilter"\>

</div\>

<table\>

<thead\>

<th\>ニックネーム</th\>

<th\>メールアドレス</th\>

</thead\>

<tbody\>

<tr v-for\="user in filteredUsers"\>

<td\>{{ user.nickname}}</td\>

<td\>{{ user.email }}</td\>

</tr\>

</tbody\>

</table\>

</div\>

</div\>

</div\>

<script src\="https://cdn.jsdelivr.net/npm/vue@3.0.0/dist/vue.global.js"\></script\>

<script\>

const app = Vue.createApp({

data: () \=> ({

users: \[\],

nickname: '',

email: '',

nicknameFilter: '',

}),

methods: {

saveUser: function() {

let user = {

nickname: this.nickname,

email: this.email,

}

this.users.push(user)

alert('ニックネーム: ' + this.nickname + '、メールアドレス: ' + this.email + 'で登録しました。')

console.log(this.nickname + " : " + this.email)

}

},

computed: {

filteredUsers: function() {

console.log('called\*\*\*\*\*')

return this.users.filter(user \=> user.nickname.includes(this.nicknameFilter))

}

}

})

app.mount('#p')

</script\>

  

</html\>
```

![[Pasted image 20210621153429.png]]



## Step 04 データ更新機能

データ更新できるようにする。

$refs が変だ。なので、focus() が動いてない。

サンプルでも同じく変だった。

```html
<!DOCTYPE html>
<html lang="en">
<meta charset="UTF-8">
<title>データの入力</title>
<style>
  .input-container {
    display: grid;
    grid-template-rows: 200px 50px;
    grid-template-columns: 300px 300px;
    grid-template-areas:
      "inputArea previewArea"
      "buttonArea buttonArea";
    grid-gap: 10px;
  }
  .inputArea {
    grid-area: inputArea;
    display: grid;
  }
  .previewArea {
    grid-area: previewArea;
    display: grid;
  }
  .buttonArea {
    grid-area: buttonArea;
    display: grid;
    justify-items: center;
    align-items: center;
  }
  .filterArea {
    display: grid;
    grid-template-columns: 300px 300px;
    grid-gap: 10px;
  }
  table {
    margin-top: 10px;
    width: 610px;
    border-collapse: collapse;
  }
  th, td {
    width: 50%;
    padding-left: 5px;
    word-break : break-all;
  }
  td input {
    width: 95%
  }
  .buttonWrapper {
    width: 610px;
    height: 50px;
    display: grid;
    justify-content: center;
    align-items: center;;
  }
</style>
<div id="app">
  <div class="input-container">
    <form class="inputArea">
      <p>ユーザー登録</p>
      <label>ニックネーム</label>
      <input v-model="nickname">
      <label>メールアドレス</label>
      <input v-model="email">
    </form>
    <div class="previewArea">
      <p>プレビュー</p>
      <label>ニックネーム</label>
      <input v-bind:value="nickname" readonly>
      <label>メールアドレス</label>
      <input v-bind:value="email" readonly>
    </div>
    <div class="buttonArea">
      <button @click="saveUser">ユーザーを登録する</button>
    </div>
    <div>
      <div class="filterArea">
        <label>リストをニックネームで絞り込む</label>
        <input v-model="nicknameFilter">
      </div>
      <table>
        <thead>
          <th>ニックネーム</th>
          <th>メールアドレス</th>
        </thead>
        <tbody>
          <tr v-for="(user, index) in filteredUsers">
            <td>
              <span v-if="!user.editable" @click="edit(user, index)">{{ user.nickname }}</span>
              <input v-show="user.editable" v-model="user.nickname" @blur="user.editable = false" ref="editNickname">
            </td>
            <td>{{ user.email }}</td>
          </tr>
        </tbody>
      </table>
      <div class="buttonWrapper">
        <button @click="displayUsers">ユーザーを表示する</button>
      </div>
    </div>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@3.0.0/dist/vue.global.js"></script>
<script>
  const app = Vue.createApp({
    data: () => ({
      users: [],
      nickname: '',
      email: '',
      nicknameFilter: '',
    }),
    methods: {
      saveUser: function() {
        let user = {
          nickname: this.nickname,
          email: this.email,
          editable: false,
        }
        this.users.push(user)
        alert('ニックネーム: ' + this.nickname + '、メールアドレス: ' + this.email + 'で登録しました。')
        this.nickname = ''
        this.email = ''
      },
      edit: function(user, index) {
        user.editable = true
        this.$nextTick(() => {
          // DOM 更新後に実行
          this.$refs.editNickname.focus()
        })
      },
      displayUsers: function() {
        let message = this.users.length + ' 人のユーザーが登録されています。'
        for (const user of this.users) {
          message += '\n' + user.nickname;
        }
        alert(message)
      }
    },
    computed: {
      filteredUsers: function() {
        console.log('called*****')
        return this.users.filter(user => user.nickname.includes(this.nicknameFilter))
      }
    }
  })
  app.mount('#p')
</script>

</html>
```

![[Pasted image 20210621165447.png]]






## Step 05 コンポーネント化

コンポーネント化する。

こっちは $refs がうまく動いている。

```html
<!DOCTYPE html>
<html lang="en">
<meta charset="UTF-8">
<title>データの入力</title>
<style>
  .input-container {
    display: grid;
    grid-template-rows: 200px 50px;
    grid-template-columns: 300px 300px;
    grid-template-areas:
      "inputArea previewArea"
      "buttonArea buttonArea";
    grid-gap: 10px;
  }
  .inputArea {
    grid-area: inputArea;
    display: grid;
  }
  .previewArea {
    grid-area: previewArea;
    display: grid;
  }
  .buttonArea {
    grid-area: buttonArea;
    display: grid;
    justify-items: center;
    align-items: center;
  }
  .filterArea {
    display: grid;
    grid-template-columns: 300px 300px;
    grid-gap: 10px;
  }
  table {
    margin-top: 10px;
    width: 610px;
    border-collapse: collapse;
  }
  th, td {
    width: 50%;
    padding-left: 5px;
    word-break : break-all;
  }
  td input {
    width: 95%
  }
  .buttonWrapper {
    width: 610px;
    height: 50px;
    display: grid;
    justify-content: center;
    align-items: center;;
  }
</style>
<div id="app">
  <div class="input-container">
    <form class="inputArea">
      <p>ユーザー登録</p>
      <label>ニックネーム</label>
      <input v-model="nickname">
      <label>メールアドレス</label>
      <input v-model="email">
    </form>
    <div class="previewArea">
      <p>プレビュー</p>
      <label>ニックネーム</label>
      <input v-bind:value="nickname" readonly>
      <label>メールアドレス</label>
      <input v-bind:value="email" readonly>
    </div>
    <div class="buttonArea">
      <button @click="saveUser">ユーザーを登録する</button>
    </div>
    <div>
      <div class="filterArea">
        <label>リストをニックネームで絞り込む</label>
        <input v-model="nicknameFilter">
      </div>
      <table>
        <thead>
          <th>ニックネーム</th>
          <th>メールアドレス</th>
        </thead>
        <tbody>
          <tr v-is="'user-row'" v-for="(user, index) in filteredUsers" :user="user" :key="index">
        </tbody>
      </table>
      <div class="buttonWrapper">
        <button @click="displayUsers">ユーザーを表示する</button>
      </div>
    </div>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@3.0.0/dist/vue.global.js"></script>
<script>
  const userRowComponent = Vue.defineComponent({
    data: function() { return { editable: false } },
    methods: {
      edit: function() {
        this.user.editable = true
        this.$nextTick(() => {
          //DOM更新後に実行
          this.$refs.editNickname.focus()
        })
      }
    },
    computed: {
      judge: function() {
        return this.user.editable
      },
    },
    template: `
      <tr>
        <td>
          <span v-if="!judge" @click="edit()"> {{ user.nickname }}</span>
          <input v-show="judge" v-model="user.nickname" @blur="user.editable = false" ref="editNickname">
        </td>
        <td>{{ user.email }}</td>
      </tr>
    `,
    props: ['user']
  })
  const app = Vue.createApp({
    components: {
      'user-row': userRowComponent
    },
    data: () => ({
      users: [],
      nickname: '',
      email: '',
      nicknameFilter: '',
    }),
    methods: {
      saveUser: function() {
        let user = {
          nickname: this.nickname,
          email: this.email,
          editable: false,
        }
        this.users.push(user)
        alert('ニックネーム: ' + this.nickname + '、メールアドレス: ' + this.email + 'で登録しました。')
        this.nickname = ''
        this.email = ''
      },
      edit: function(user, index) {
        user.editable = true
        this.$nextTick(() => {
          // DOM 更新後に実行
          this.$refs.editNickname.focus()
        })
      },
      displayUsers: function() {
        let message = this.users.length + ' 人のユーザーが登録されています。'
        for (const user of this.users) {
          message += '\n' + user.nickname;
        }
        alert(message)
      }
    },
    computed: {
      filteredUsers: function() {
        console.log('called*****')
        return this.users.filter(user => user.nickname.includes(this.nicknameFilter))
      }
    }
  })
  app.mount('#p')
</script>

</html>
```

![[Pasted image 20210621174629.png]]



# 第2章 開発環境を準備しよう

```shell
$ vue create sample-app

今は、vue2 か vue3 かだけの選択と、

Manually select を選んで組み込むだ。<--こちらにする

PWA 以外は全入れにする。
veu 2 にする。(本が Vue2)
```



よくわからないまま、1章の html を vue project に入れ込もうとした。

```shell
$ vue create sample-app
```

![[Pasted image 20210622135050.png]]

sass-loader を入れる。

```shell
$ npm install sass-loader node-sass --save-dev
```