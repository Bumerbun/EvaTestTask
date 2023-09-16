<template>
    <div>
        <div>
            <input type="text"/>
        </div>
        <div class="answer_buttons_list">
            <div v-for="(elem, key) in testElems" class="answer_buttons_list" v-bind:key="elem">
                <AnswerButton class="answer_button_block"/>
                <input type="button" class="answer_button_remove" value="x" v-on:click="removeAnswerButton(key)"/>
            </div>
            <input type="button" class="add_answer_button" :onClick="addAnswerButton"/>
            
        </div>
    </div>
</template>

<script>
import AnswerButton from './AnswerButton.vue';
export default {
    props: {},
    data(){
        return {
            testElems: [1,2,3,4,5]
        }
    },
    methods: {
        addAnswerButton(){
            this.testElems.push(1)
        },
        removeAnswerButton(key){
            this.testElems.splice(key,1)
        }
    }
}

</script>

<style scoped>
.answer_buttons_list {
    display: flex;
}
.answer_button_block {
    display: inline-block;
    width: 180px;
}
.answer_button_remove {
    display: inline-block;
    margin-bottom: auto;
}
.add_answer_button {
    display: inline-block;
    margin-top: auto;
    margin-bottom: auto;
}
</style>