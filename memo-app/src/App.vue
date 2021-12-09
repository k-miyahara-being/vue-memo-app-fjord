<template>
  <div>
    {{$data}}
    <h1>メモアプリ</h1>
    <ul>
      <li v-for="(memo, index) in memos" :key="memo.content">
        <a v-if="index!=edit" href="#" @click.prevent="edit_memo(index)">{{ memo.firstline }}</a>
        <span v-else>
          <span>{{ memo.firstline }}</span>
          <textarea v-model="editmemo"></textarea>
          <button class="edit_btn" @click="change_memo(index)">編集</button>
          <button class="delete_btn" @click="delete_memo(index)">削除</button>
        </span>
      </li>
      <li><a class="plus" href="#" @click.prevent="add_memo()">+</a></li>
    </ul>

  </div>
</template>

<script>
export default {
  data () {
    return {
      memos: [],
      newmemo: '新規メモ',
      editmemo: '',
      edit: -1,
    }
  },
  mounted(){
    this.memos = JSON.parse(localStorage.getItem('items'))
  },
  methods: {
    add_memo () {
      this.memos.push({
        content: this.newmemo,
        firstline: this.newmemo
      })
      this.save_memo()
    },
    edit_memo (index) {
      this.edit = index
      this.editmemo = this.memos[index].content
    },
    change_memo (index) {
      this.memos[index].content = this.editmemo
      this.memos[index].firstline = this.get_firstline(this.memos[index].content)
      this.save_memo()
      this.edit = -1
    },
    delete_memo (index) {
      this.memos.splice(index, 1)
      this.save_memo()
      this.edit = -1
    },    
    save_memo () {
      localStorage.setItem('items', JSON.stringify(this.memos))
    },
    get_firstline (text) {
      return text.split('\n')[0]
    }
  }
}
</script>

<style>
  body {
    width: 500px;
    min-height: 500px;
    text-align: center;
  }
  
  h1 {
   background-color: gray;
   font-size: 30px;
   color: white;
  }

  ul {
    position: relative;
    list-style-type: none;
    text-align: left;
  }

  li {
    font-size: 20px;
    padding: 0.5em 0;
  }

  textarea {
    position: absolute;
    right: 5px;
    top: 0px;
    resize: none;
    width: 250px;
    height: 330px;
    font-size: 20px;
    font-family: sans-serif;
    padding: 0.5em;
    border-radius: 8px;
  }

  textarea:focus {
    box-shadow: 0 0 4px gray;
    outline: none;
  }

  button {
    margin-top: 5px;
    font-size: 18px;
    border-radius: 8px;
  }
  
  .edit_btn {
    position: absolute;
    width: 190px;
    top: 360px;
    left: 225px;
  }

  .delete_btn {
    position: absolute;
    width: 65px;
    top: 360px;
    left: 425px;
  }

  .plus {
    font-size: 30px;
    text-decoration: none;
  }
</style>
