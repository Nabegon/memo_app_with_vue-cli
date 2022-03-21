<template>
<div class="container">
  <div class="memoList">
    <div v-for="(memo, index) in memos" :key="memo.id">
      <label @click="editMemo(index)">{{ memo[0] }}</label>
    </div>
    <label @click="createNewMemo" class="createButton">+</label>
  </div>
  <div v-if="creatMemoStatus || editMemoStatus" class="memoForm">
      <label v-if="creatMemoStatus">Create a memo</label>
      <label v-if="editMemoStatus">Edit or delete a Memo</label>
      <textarea v-if="creatMemoStatus" name="memo" rows="10" required v-model="newMemo"></textarea>
      <textarea v-if="editMemoStatus" name="memo" rows="10" required v-model="memoData"></textarea>
      <button v-if="creatMemoStatus" @click="addMemo">add</button>
      <button v-if="editMemoStatus" @click="submitEditedMemo">edit</button>
      <button v-if="editMemoStatus" @click="deleteMemo">delete</button>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      memoData: '',
      memos: [],
      newMemoObject: '',
      creatMemoStatus: false,
      editMemoStatus: false,
      test: '',
      editedMemo: '',
      index: '',
      updatedMemo: '',
      newMemo: ''
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
      this.editMemoStatus = false
    },
    editMemo(index) {
      this.editMemoStatus = true
      this.creatMemoStatus = false
      this.test = this.memos[index]
      this.memoData = this.test.toString().replace(/,/g, "\n")
      this.index = index
    },
    submitEditedMemo() {
      this.updatedMemo = this.memoData.split(/\n/)
      this.memos.splice(this.index, 1, this.updatedMemo)
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.editMemoStatus = false
      this.memoData = ''
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
    margin: 0px 100px 0px;
    background: white;
    padding: 40px;
    border-radius: 10px;
  }
  label {
    color: #aaa;
    display: inline-block;
    margin: 0px 0 5px;
    padding: 5px;
    font-size: 0.8em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
  }
 
  textarea {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
  } 
  .container {
    width: 700px;
    margin: auto;
    background: white;
    display: flex;
  }
  .memoList {
    display: block;
    text-align: left;
    display: flex;
    flex-flow: column;
  }
  .createButton {
    display: block;
    text-align: left;
  }
  .memoForm {
    display: block;
    width: 80%;
  }
   
</style>