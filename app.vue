<template>
  <div>
    <MessengerSelection :optionsData="optionsData"/>
  </div>
</template>

<script>
import MessengerSelection from './components/MessengerSelection.vue'
export default {
  data(){
    return {
      optionsData: [
        {'label': "ВК", 'ref': "vk"},
        {'label': "WhatsApp", 'ref': "whatsapp"},
        {'label': "Telegram", 'ref': "tg"},
        {'label': "SMS", 'ref': "sms"},
      ]
    }
  }
}
</script>
