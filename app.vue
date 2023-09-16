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
        {'label': "ВК", 
        'ref': "vk",
        'limitations': {
          'maxTextSize' : 4069,
          'customKeyboardSupport': true,
          'standardKeyboardSupport': true,
          'standardKeyboardLimitations': {
            'maxButtonCount': 40,
            'maxButtonTextSize' : -1,
            'linkButtonSupport': true,
            'maxLinkButtonCount': -1
          },
          'inlineKeyboardSupport': true,
          'inlineKeyboardLimitations':  {
            'maxButtonCount': 10,
            'maxButtonTextSize' : -1,
            'linkButtonSupport': true,
            'maxLinkButtonCount': -1
          },
        }
      },
        {
          'label': "WhatsApp", 
          'ref': "whatsapp",
          'limitations': {
            'maxTextSize' : 1000,
            'customKeyboardSupport': true,
            'standardKeyboardSupport': true,
            'standardKeyboardLimitations': {
              'maxButtonCount': 10,
              'maxButtonTextSize' : 20,
              'linkButtonSupport': false,
              'maxLinkButtonCount': 0
            },
            'inlineKeyboardSupport': true,
            'inlineKeyboardLimitations':  {
              'maxButtonCount': 3,
              'maxButtonTextSize' : 20,
              'linkButtonSupport': true,
              'maxLinkButtonCount': 1  
          },
        }
      },
        {
          'label': "Telegram", 
          'ref': "tg",
          'limitations': {
            'maxTextSize' : 4096,
            'customKeyboardSupport': true,
            'standardKeyboardSupport': true,
            'standardKeyboardLimitations': {
              'maxButtonCount': -1,
              'maxButtonTextSize' : -1,
              'linkButtonSupport': false,
              'maxLinkButtonCount': 0

            },
            'inlineKeyboardSupport': true,
            'inlineKeyboardLimitations':  {
              'maxButtonCount': -1,
              'maxButtonTextSize' : 64,
              'linkButtonSupport': true,
              'maxLinkButtonCount': -1
          },
        }
        },
        {
          'label': "SMS", 
          'ref': "sms",
          'limitations': {
            'maxTextSize' : 1000,
            'customKeyboardSupport': true,
            'standardKeyboardSupport': true,
            'standardKeyboardLimitations': {
              'maxButtonCount': 10,
              'maxButtonTextSize' : 20,
              'linkButtonSupport': false
            },
            'inlineKeyboardSupport': true,
            'inlineKeyboardLimitations':  {
              'maxButtonCount': 3,
              'maxButtonTextSize' : 20,
              'linkButtonSupport': true,
              'maxLinkButtonCount': 1  
          },
        }
        },
      ],
    }
  }
}
</script>
