<template>
    <div class="selectionOptions">
        <div class="option">
            <div><label>ВКонтакте</label></div>
            <div><LabeledCheckBox ref="vk" @onClick="onClick" uncheckedText="what"/></div>
        </div>
        <div class="option">
            <div><label>WhatsApp</label></div>
            <div><LabeledCheckBox ref="whatsup"/></div>
        </div>
        <div class="option">
            <div><label>Telegram</label></div>
            <div><LabeledCheckBox ref="telegram"/></div>
        </div>
        <div class="option">
            <div><label>SMS</label></div>
            <div><LabeledCheckBox ref="sms"/></div>
        </div>
    </div>
</template>

<script>
import LabeledCheckBox from './LabeledCheckBox.vue';
var selectedOptions = [];
export default{
    methods: {
        onClick(optionObject){
            if (optionObject.isChecked){
                console.log("test")
                var position = this.addSelectedOption(optionObject)
                optionObject.labelChecked = position
            }
            else{
                this.removeSelectedOption(optionObject)
            }
        },
        addSelectedOption(optionObject){
            selectedOptions.push(optionObject)
            console.log("test1")
            return selectedOptions.length
        },
        removeSelectedOption(optionObject){
            var position = selectedOptions.indexOf(optionObject);
            selectedOptions.splice(position, 1)
            for (let i = 0; i < selectedOptions.length; i++){
                selectedOptions[i].labelChecked = i
            }
        }
    }

    }
</script>

<style>
.selectionOptions {
    display:grid;
}
.option {
    display:grid;
    grid-template-columns: 4fr 1fr;
}
</style>