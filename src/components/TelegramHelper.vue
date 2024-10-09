<script setup>
import {onMounted, ref} from "vue";

let botToken =  ref();
let webhookUrl =  ref();
let userId =  ref();
let result =  ref();

onMounted(() => {
  useLocalStorage();
})

function getWebhookInfo() {
  let url = 'https://api.telegram.org/bot' + botToken.value + '/getWebhookInfo';
  const requestOptions = {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
    },
  };
  fetch(url, requestOptions)
      .then(response => response.json())
      .then(data => result.value = data);
}

function setWebhookUrl() {

}

function sendMessage() {
  let text = Math.random().toString(36).slice(2);
  let url = 'https://api.telegram.org/bot' + botToken.value + '/sendMessage?chat_id=' + userId.value + '&text=' + text;
  const requestOptions = {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
    },
  };
  fetch(url, requestOptions)
      .then(response => response.json())
      .then(data => result.value = data);
}

function updateLocalStorage() {
  localStorage.setItem('botToken', botToken.value);
  localStorage.setItem('webhookUrl', webhookUrl.value);
  localStorage.setItem('userId', userId.value);
}

function useLocalStorage() {
  botToken.value = localStorage.botToken ?? '';
  webhookUrl.value = localStorage.webhookUrl ?? '';
  userId.value = localStorage.userId ?? '';
}

</script>

<template>
  <h1>TH</h1>

  <div class="card">
    <div class="text-field">
      <label class="text-field__label" for="webhook-url">webhookUrl</label>
      <input class="text-field__input" type="text" name="webhook-url" id="webhook-url" placeholder="webhookUrl"
             v-model="webhookUrl" @change="updateLocalStorage" disabled>
    </div>
    <div class="text-field">
      <label class="text-field__label" for="bot-token">botToken</label>
      <input class="text-field__input" type="text" name="bot-token" id="bot-token" placeholder="botToken"
             v-model="botToken" @change="updateLocalStorage">
    </div>
    <div class="text-field">
      <label class="text-field__label" for="user-id">userId</label>
      <input class="text-field__input" type="text" name="user-id" id="user-id" placeholder="userId" v-model="userId"
             @change="updateLocalStorage">
    </div>
  </div>
  <div>
    <button @click="getWebhookInfo(botToken)">getWebhookInfo</button>
    <button @click="setWebhookUrl(webhookUrl)">setWebhookUrl</button>
    <button @click="sendMessage(botToken, userId)">sendMessage</button>
  </div>
  <div>
    <pre v-show="result">{{ result }}</pre>
  </div>
</template>

<style scoped></style>
