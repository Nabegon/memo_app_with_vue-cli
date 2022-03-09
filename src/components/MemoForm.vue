<template>
 <div v-for="(memo, index) in memos" :key="memo.id">
    <!-- <label @dblclick="editMemo(index)">{{ memo.memo[0] }}</label> -->
    <label @dblclick="editMemo(index)">{{ memo[0] }}</label>
 </div>
 <button @click="createNewMemo">+</button>
 <div v-if="creatMemoStatus">
    <form @submit.prevent="addMemo">
      <label>Create a memo</label>
      <textarea name="memo"  rows="10" required v-model="newMemo"></textarea>
      <button>add</button>
    </form>
 </div>
 <div v-if="editMemoStatus">
    <form>
      <label>Edit or delete a Memo</label>
      <textarea name="memo" rows="10" required v-model="editedMemo"></textarea>
      <button @click="submitEditedMemo">edit</button>
      <button @click="deleteMemo">delete</button>
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
      creatMemoStatus: false,
      editMemoStatus: false,
      test: '',
      editedMemo: '',
      index: '',
      updatedMemo: ''
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
      this.memos.push(newMemoObject)
      this.newMemoObject = ''
      this.newMemo = ''
      this.creatMemoStatus = ''
      this.saveJSONData()
    },
    createNewMemo() {
      this.creatMemoStatus = true
    },
    editMemo(index) {
      console.log(this.memos)
      this.editMemoStatus = true
      this.test = this.memos[index]
      this.editedMemo = this.test.toString().replace(/,/g, "\n")
      this.index = index
    },
    submitEditedMemo() {
      this.updatedMemo = this.editedMemo.split(/\n/)
      this.memos.splice(this.index, 1, this.updatedMemo)
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.editMemoStatus = false
      this.editedMemo = ''
    },
    deleteMemo() {
      if (confirm('are you sure to delete this todo?')) {
        this.memos.splice(this.index, 1)
      }
      localStorage.setItem('memos', JSON.stringify(this.memos))
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