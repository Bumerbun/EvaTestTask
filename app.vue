<template>
  <div>
    <dialog ref="saveNotification">
        <label>Данные успешно сохранены</label>
        <input type="button" value="ок" v-on:click="closeDialog"/>
    </dialog>
    <MessengerSelection :optionsData="optionsData" @selection-changed="optionsSelectionChanged"/>
    <div v-for="option in selectedOptions" :key="option.ref">
        <MessageOptions :optionData="option" ref="optionsData"/>
    </div>
    <input type="button" v-on:click="saveButtonClick" value="Сохранить"/>
  </div>
</template>

<script>
import MessengerSelection from './components/MessengerSelection.vue';
import MessageOptions from './components/MessageOptions.vue'
export default {
    data() {
        return {
            optionsData: [
                { 'label': "ВК",
                    'ref': "vk",
                    'limitations': {
                        'maxTextSize': 4096,
                        'customKeyboardSupport': true,
                        'standardKeyboardSupport': true,
                        'standardKeyboardLimitations': {
                            'maxButtonCount': 40,
                            'maxButtonTextSize': -1,
                            'linkButtonSupport': true,
                            'maxLinkButtonCount': -1
                        },
                        'inlineKeyboardSupport': true,
                        'inlineKeyboardLimitations': {
                            'maxButtonCount': 10,
                            'maxButtonTextSize': -1,
                            'linkButtonSupport': true,
                            'maxLinkButtonCount': -1
                        },
                    }
                },
                {
                    'label': "WhatsApp",
                    'ref': "whatsapp",
                    'limitations': {
                        'maxTextSize': 1000,
                        'customKeyboardSupport': true,
                        'standardKeyboardSupport': true,
                        'standardKeyboardLimitations': {
                            'maxButtonCount': 10,
                            'maxButtonTextSize': 20,
                            'linkButtonSupport': false,
                            'maxLinkButtonCount': 0
                        },
                        'inlineKeyboardSupport': true,
                        'inlineKeyboardLimitations': {
                            'maxButtonCount': 3,
                            'maxButtonTextSize': 20 ,
                            'linkButtonSupport': true,
                            'maxLinkButtonCount': 2
                        },
                    }
                },
                {
                    'label': "Telegram",
                    'ref': "tg",
                    'limitations': {
                        'maxTextSize': 4096,
                        'customKeyboardSupport': true,
                        'standardKeyboardSupport': true,
                        'standardKeyboardLimitations': {
                            'maxButtonCount': -1,
                            'maxButtonTextSize': -1,
                            'linkButtonSupport': false,
                            'maxLinkButtonCount': 0
                        },
                        'inlineKeyboardSupport': true,
                        'inlineKeyboardLimitations': {
                            'maxButtonCount': -1,
                            'maxButtonTextSize': 64,
                            'linkButtonSupport': true,
                            'maxLinkButtonCount': -1
                        },
                    }
                },
                {
                    'label': "SMS",
                    'ref': "sms",
                    'limitations': {
                        'maxTextSize': -1,
                        'customKeyboardSupport': false,
                        'standardKeyboardSupport': false,
                        'standardKeyboardLimitations': {
                            'maxButtonCount': 0,
                            'maxButtonTextSize': 0,
                            'linkButtonSupport': false,
                            'maxLinkButtonCount': 0
                        },
                        'inlineKeyboardSupport': false,
                        'inlineKeyboardLimitations': {
                            'maxButtonCount': 0,
                            'maxButtonTextSize': 0,
                            'linkButtonSupport': false,
                            'maxLinkButtonCount': 0
                        },
                    }
                },
            ],
            selectedOptions: []
        };
    },
    mounted(){

    },
    methods:{
        async optionsSelectionChanged(selector){
            this.selectedOptions = selector.getSelectedOptionsData()
            await nextTick()
        },
        async saveButtonClick(){
            const channelhash = {}
            const channelsData = await ((await fetch("http://localhost:3333/channel", {method:"GET"})).json())
            for (var i = 0; i < channelsData.length; i++){
                channelhash[`${channelsData[i]['name']}`] = channelsData[i]['id']
            }
            const keyboardTypehash = {}
            const keyboardTypeData = await ((await fetch("http://localhost:3333/keyboardType", {method:"GET"})).json())
            for (var i = 0; i < keyboardTypeData.length; i++){
                keyboardTypehash[`${keyboardTypeData[i]['name']}`] = keyboardTypeData[i]['id']
            }
            var finalData = this.$refs.optionsData.map((component) => component.getSelectedData())
            console.log(finalData)
            for (var i = 0; i < finalData.length; i++){
                finalData[i].channel = channelhash[`${finalData[i].channel}`]
                finalData[i].keyboardType = keyboardTypehash[`${finalData[i].keyboardType}`]
                //console.log(finalData[i])
                await fetch("http://localhost:3333/channelMessages", {
                    method: "POST",
                    body: JSON.stringify(finalData[i]),
                    headers: {
                        "Content-type": "application/json; charset=UTF-8"
                    }
                })
            }
            this.$refs.saveNotification.showModal()
        },
        closeDialog(){
            this.$refs.saveNotification.close()
        }
    }
}
</script>
