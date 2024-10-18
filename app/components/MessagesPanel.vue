<script setup lang="ts">
const { data: messages, refresh } = await useFetch('/api/messages')
const newMessage = ref('')

async function sendMessage (){
  if (!newMessage.value.trim()) return
  await $fetch('/api/messages', {
    method: 'POST',
    body: {
      text: newMessage.value
    }
  })
  newMessage.value = ''
  await refresh()
}
</script>

<template>
  <div>
    <h3>メッセージ</h3>
    <form @submit.prevent="sendMessage">
      <input v-model="newMessage" placeholder="メッセージを入力">
      <button type="submit">
        送信
      </button>
    </form>
    <p v-for="message of messages" :key="message.id">
      {{ message.text }} - {{ new Date(message.created_at).toLocaleString('fr-FR') }}
    </p>
    <p v-if="!messages?.length">
      メッセージはありません
    </p>
  </div>
</template>
