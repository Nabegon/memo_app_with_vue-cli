<template>
 <div v-for="(memo, index) in memos" :key="memo.id">
    {{ memo.memo[0] }}
 </div>
 <button @click="createNewMemo">+</button>
 <div v-if="creatMemoStatus">
    <form @submit.prevent="addMemo">
      <label>Create a memo</label>
      <textarea name="memo"  rows="10" required v-model="newMemo"></textarea>
      <button>add</button>
    </form>
 </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'
export default {
  data() {
    return {
      newMemo: '',
      memos: [],
      newMemoObject: '',
      uuid: '',
      newMemoObject: '',
      creatMemoStatus: false
    }
  },
  mounted() {
    this.memos = JSON.parse(localStorage.getItem('memos') || '[]')
  },
  methods: {
    saveJSONData() {
      localStorage.setItem('memos', JSON.stringify(this.memos))
    },
    addMemo() {
      let newMemoObject = this.newMemo.split(/\n/)
      var newMemoObjectWithId = {
        id: uuidv4(),
        memo: newMemoObject
      }
      this.memos.push(newMemoObjectWithId)
      newMemoObject = ''
      newMemoObjectWithId = ''
      this.saveJSONData()
    },
    createNewMemo() {
      this.creatMemoStatus = true
    }
  }
}
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
  }
  textarea {
    height: 100%;
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
  }
</style>