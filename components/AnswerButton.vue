<template>
    <div class="answer_button">
        <div class="button_text">
            <label>Текст</label>
            <input type="text" ref="textInput" v-model="text" :maxlength="maxTextSize" class="button_text_input"/>
        </div>
        <div class="button_link" v-if="linksSupport">
            <label>Ссылка</label>
            <input type="checkbox" ref="linkCheckbox" class="button_link_checkbox" v-model="linkEnabled" :change="onChange"/>
            <input type="text" ref="linkInput" v-if="linkEnabled" v-model="link" class="button_link_input"/>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        maxTextSize: {default: -1},
        linksSupport: {default: true}
    },
    data(){
        return {
            text: "",
            link: "",
            linkEnabled: false,
        }
    },
    computed: {
        linkInputVisiability(){
            return this.linksSupport ? "visible" : "hidden";
        },
        linkVisiability(){
            return this.linksSupport ? "visible" : "hidden";
        }
    },
    methods: {
        onChange(event){

        }
    }
}
</script>

<style scoped>
</style>