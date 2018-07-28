<template>
<div class="editer">
    <div class="memoListWrapper">
        <div class="memoBtns">
            <button class="addMemoBtn" @click="addMemo">メモの追加</button>
            <button class="saveMemosBtn" @click="saveMemo">メモの保存</button>
            <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">メモの削除</button>
        </div>
        <div class="memoList" v-for="(memo, index) in memos" @click="selectMemo(index)":data-selected="index==selectedIndex">
            <p class="memoTitle">{{displayTitle( memo.markdown )}}</p>
        </div>
    </div>
    <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
    <div class="previewWrapper" ref="preview">
        <p class="previewTitle">Preview Area</p>
        <div class="preview markdownHtml" v-html="preview()"></div>
    </div>
</div>
</template>

<script>
import marked from 'marked';
export default {
  name: 'editor',
  props: ['user'],
  data() {
    return {
        memos:[{
            markdown:''
        }],
        selectedIndex: 0
    }
   },
   created: function() {
    firebase
      .database()
      .ref('memos/' + this.user.uid)
      .once('value')
      .then(result => {
        if (result.val()) {
          this.memos = result.val();
        }
      })
   },
   methods: {
   logout: function() {
        firebase.auth().signOut();
   },
   addMemo: function(){
    this.memos.push({
        markdown: '無題のメモ',
    })
   },
   deleteMemo: function(){
    this.memos.splice(this.seletedIndex,1);
     if (this.selectedIndex > 0) {
        this.selectedIndex--;
     }
   },
   saveMemo: function(){
    firebase
    .database()
    .ref('memos/' + this.user.uid)
    .set(this.memos)
   },
   selectMemo: function(index){
        this.selectedIndex = index;
   },
   preview: function() {
        return marked(this.memos[this.selectedIndex].markdown);
   },
   displayTitle: function(text){
    return text.split(/¥n/)[0];
    },
   
 }
}
</script>

<style lang="scss" scoped>
.editer {
    height: 100%;
}

.memoListWrapper {
    width: 30%;
    padding-bottom: 20px;
}

.memoList{
    padding: 10px;
    box-sizing: border-box;
    text-align: left;
    border-bottom: 1px solid #000;
    background-color:#555555;
    &:nth-child(even){
        backgroud-color:#333;
    }
    &[data-selected="true"]{
        background-color:#333;
    }
    
}

.memoTitle{
    height: 1.5em;
    margin: 0;
    color: white;
    white-space: nowrap;
    overflow: hidden;
}

.MemoBtns{
    padding: 10px;
    border-bottom: #ccc 1px solid;
    :nth-child(n+2) {
        margin-left: 10px;
    }
}
.deleteMemoBtn {
    margin: 10px;
}

button {
  border: none;
  border-radius: 4px;
  line-height: 24px;
  padding: 0.8px;
  background: #0a9b94;
  color: #fff;
  cursor: pointer;
}

.markdown {
    border: none;
    border-right: #ccc 1px solid;
    border-left: #ccc 1px solid;
    background-color: #eee;
    box-shadow: inset 0 0 5px 0 rgba(0, 0, 0, 0.1);
    padding: 10px;
    width: 40%;
    resize: none;
}
.previewWrapper{
    text-align: left;
    width: 30%;
}

.previewTitle {
    color: #888;
    padding: 10px;
    font-size: 14px;
    border-bottom: #ddd 1px dotted;
}

.preview {
    padding: 10px;
}

.memoListWrapper,
.markdown,
.previewWrapper {
    overflow: scroll;
    float: left;
    height: 100%;
    box-sizing: border-box;
}
</style>
