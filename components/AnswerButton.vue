<template>
    <div class="answer_button">
        <div class="button_text">
            <label>Текст</label>
            <input type="text" ref="textInput" v-model="text" :maxlength="answerButtonLimitations.maxButtonTextSize" class="button_text_input"/>
        </div>
        <div class="button_link" v-show="answerButtonLimitations.linkButtonSupport">
            <label>Ссылка</label>
            <input type="checkbox" ref="linkCheckbox" class="button_link_checkbox" v-model="linkEnabled" v-on:change="$emit('on-change', this)"/>
            <input type="text" ref="linkInput" v-show="linkEnabled" v-model="link" class="button_link_input"/>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        answerButtonLimitations: {
            default: {
                "maxTextSize": -1,
                "linkButtonSupport": true
            }
        }
    },
    data(){
        return {
            text: "",
            link: "",
            linkEnabled: false,
        }
    }
}
</script>
