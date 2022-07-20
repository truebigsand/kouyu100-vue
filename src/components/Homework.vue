<script setup>
import mdui from 'mdui';
import { onMounted, reactive, ref } from 'vue'

const BASE_API_URL = 'http://106.15.92.146:5000'

const props = defineProps(['homework', 'authToken'])

const state = reactive({answerString: '', isSubmitDisabled: false})
function getAnswerString() {
  fetch(`${BASE_API_URL}/getAnswerString?authToken=${props.authToken}&ExamId=${props.homework.ExamId}`)
  .then(response => {
    if (response.status == 500) {
      state.isSubmitDisabled = true
      return '暂不支持此类型作业'
    }
    return response.text()
  })
  .then(data => {
    state.answerString = data
  })
}
function uploadAnswerString() {
  var urlString = `${BASE_API_URL}/uploadAnswer?authToken=${props.authToken}&ExamId=${props.homework.ExamId}&HomeworkId=${props.homework.HomeworkId}&AnswerString=${state.answerString}`
  fetch(urlString)
  .then(response => response.text())
  .then(data => {
    console.log(data);
    mdui.snackbar({
      message: '上传' + props.homework.Name + (data != {"scoreList":[]} ? '成功' : '失败'),
      position: 'right-top'
    })
  })
}
onMounted(() => {
  mdui.mutation() // active widget
  getAnswerString()
})

</script>

<template>
  <div class="mdui-panel-item">
    <div id="homework-panel-item-header" class="mdui-panel-item-header">
      {{ homework.Name }} | {{ homework.Status === 2 ? '已完成' : '未完成' }}
    </div>
    <div class="mdui-panel-item-body">
      <p>ExamId: {{ homework.ExamId }}</p>
      <p>HomeworkId: {{ homework.HomeworkId }}</p>
      <div class="mdui-textfield"><input id="answer-input" class="mdui-textfield-input" v-model="state.answerString" /></div>
      <button @click="uploadAnswerString" v-if="!state.isSubmitDisabled" id="submit-button" class="mdui-btn mdui-btn-raised mdui-ripple mdui-color-theme-accent">提交</button>
    </div>
  </div>
</template>
