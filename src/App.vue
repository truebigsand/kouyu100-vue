<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Homework from './components/Homework.vue'
import { reactive } from 'vue';

const BASE_API_URL = 'http://106.15.92.146:5000'
const state = reactive({ authToken: '', homeworkList: [] })

function getHomeworkList() {
  fetch(BASE_API_URL + `/getHomeworkList?authToken=${state.authToken}`)
    .then(response => {
      return response.json()
    })
    .then(data => {
      console.log(data)
      state.homeworkList = data['data']
    })
}
</script>
<script>
  
</script>
<template>
  <div class="mdui-container">
    <div class="mdui-textfield">
      <label class="mdui-textfield-label">authToken</label>
      <input v-model="state.authToken" id="authToken-input"
        placeholder="njjlzxhx.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx" class="mdui-textfield-input" type="text" />
    </div>
    <button @click="getHomeworkList" id="get-homework-list-btn"
      class="mdui-btn mdui-btn-raised mdui-ripple mdui-color-theme-accent">获取作业列表</button>
    <div id="homework-list-panel" class="mdui-panel" mdui-panel>
      <Homework v-for="homework in state.homeworkList" :homework="homework" :authToken="state.authToken" />
    </div>
  </div>
  <!--<Homework msg="Hello world!" />-->
</template>
