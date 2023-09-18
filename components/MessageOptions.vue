<template>
    <div>
        <label>{{this.optionData.label}}</label>
        <div>
            <label>Текст сообщения:</label>
            <input type="text" v-model="messageText" :maxlength="this.optionData.limitations.maxTextSize"/>
        </div>
        <div v-show="switchOptions.length">
            <div>
                <label>Режим отображения:</label>
                <OptionSwitchButton :switch-options="switchOptions" ref="optionSwitch" @on-option-change="onAnswerButtonsModeSwitch"/>
            </div>
            <div class="answer_buttons_list">
                <div v-for="(elem, key) in testElems" ref="answerButtonsList" class="answer_buttons_list" v-bind:key="elem.id">
                    <AnswerButton class="answer_button_block" :answerButtonLimitations="elem.limitations"  ref="answerOption" @on-change="onAnswerLinkCheck"/>
                    <input type="button" class="answer_button_remove" value="x" v-on:click="removeAnswerButton(key)"/>
                </div>
                <input type="button" class="add_answer_button" value="+" ref="addAnswerButton" :onClick="addAnswerButton" v-show="currentKeyboardLimitations.maxButtonCount == -1 ? true : testElems.length < currentKeyboardLimitations.maxButtonCount"/>
                
            </div>
    
        </div>
    </div>
</template>

<script>
import AnswerButton from './AnswerButton.vue';
import OptionSwitchButton from './OptionSwitchButton.vue';
import { generateShortUID } from '~/scripts/shortUIDGenerator'
export default {
    props: {
        "optionData": {required: true}
    },
    data(){
        return {
            testElems: [],
            messageText: "",
            switchOptions: [],
            currentKeyboardLimitations: this.optionData.limitations.standardKeyboardLimitations,
            showAddAnswerButton: true
            
        }
    },
    mounted(){
        if (this.optionData.limitations.standardKeyboardSupport){
            this.switchOptions.push("standard")
        }
        if (this.optionData.limitations.inlineKeyboardSupport){
            this.switchOptions.push("inline")
        }
    },
    methods: {
        async addAnswerButton(){

            var buttonid = generateShortUID()
            this.testElems.push({
                "id": buttonid,
                "limitations": {... this.currentKeyboardLimitations}})
            await nextTick()
            this.validateButtonCount()
            this.answerLinkCountValidation()
        },
        async removeAnswerButton(key){
            this.testElems.splice(key,1)
            await nextTick()
            this.validateButtonCount()
            this.answerLinkCountValidation()
        },
        async validateButtonCount(){
            if (this.testElems.length <= this.currentKeyboardLimitations.maxButtonCount || this.currentKeyboardLimitations.maxButtonCount == -1){
                return
            }
            this.testElems.splice(this.currentKeyboardLimitations.maxButtonCount, this.testElems.length - this.currentKeyboardLimitations.maxButtonCount)
            await nextTick()

        },
        onAnswerLinkCheck(answerButton){
            this.answerLinkCountValidation()
        },
        async answerLinkCountValidation(){
            if (!this.$refs.answerOption){
                return
            }
            const answerButtons = this.$refs.answerOption
            const answerLinksCount = answerButtons.map((button) => button.linkEnabled)
                .reduce((accumulator, currentValue) => {return accumulator + currentValue},0);
            var isTooManyLinks;
            if (this.currentKeyboardLimitations.maxLinkButtonCount < 0){
                isTooManyLinks = false;
            }
            else{
                isTooManyLinks = this.currentKeyboardLimitations.maxLinkButtonCount <= answerLinksCount
            }
            for (var i = answerButtons.length - 1; i >= 0; i--){
                if (answerButtons[i].linkEnabled){
                    continue
                }
                answerButtons[i].answerButtonLimitations.linkButtonSupport = !isTooManyLinks

            }
            await nextTick()
        },
        async onAnswerButtonsModeSwitch(selectedOption){
            switch(selectedOption){
                case "standard":
                    this.currentKeyboardLimitations = this.optionData.limitations.standardKeyboardLimitations;
                    break;
                case "inline":
                    this.currentKeyboardLimitations = this.optionData.limitations.inlineKeyboardLimitations;
                    break
                case _:
                    this.currentKeyboardLimitations = this.optionData.limitations.standardKeyboardLimitations;
                    break;
            }
            for (var i = 0; i < this.testElems.length; i++){
                this.testElems[i].limitations = {... this.currentKeyboardLimitations}
            }
            await nextTick()
            this.validateButtonCount()
            this.answerLinkCountValidation()
        },
        getSelectedData(){
            var finalAnswerButtonsData = []
            if (this.testElems.length != 0){
                for(var i = 0; i < this.testElems.length; i++){
                    finalAnswerButtonsData.push({
                        "text" :this.$refs.answerOption[i].text,
                        "link" :this.$refs.answerOption[i].link,
                    })
                }
            }
            const currentKeyboardOption = this.$refs.optionSwitch.currentOption
            console.log(currentKeyboardOption)
            return {
                text: this.messageText,
                channel: this.optionData.label,
                keyboardType: currentKeyboardOption,
                answerButtons: finalAnswerButtonsData
            }
        }
        
    }
}

</script>

<style scoped>
.answer_buttons_list {
    display: flex;
    flex-wrap: wrap;
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