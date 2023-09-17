<template>
    <div class="selectionOptions" v-for="option in optionsData">
        <div class="option">
            <div><label>{{option.label}}</label></div>
            <div><LabeledCheckBox :ref="option.ref" :id="option.ref" @onClick="onClick"/></div>
        </div>
    </div>
</template>

<script>
import LabeledCheckBox from './LabeledCheckBox.vue';
var selectedOptions = [];
export default{
    props:{
        "optionsData":{required:true, InstanceType: Array}
    },
    emits:['selection-changed'],
    methods: {
        onClick(optionObject){
            if (optionObject.isChecked){
                const position = this.addSelectedOption(optionObject)
                optionObject.labelChecked = position
            }
            else{
                this.removeSelectedOption(optionObject)
            }
            this.$emit('selection-changed', this)
        },
        addSelectedOption(optionObject){
            selectedOptions.push(optionObject)
            return selectedOptions.length
        },
        removeSelectedOption(optionObject){
            const position = selectedOptions.indexOf(optionObject);
            selectedOptions.splice(position, 1)
            for (let i = 0; i < selectedOptions.length; i++){
                selectedOptions[i].labelChecked = i + 1;
            }
        },
        unselectAll(){
            for (option in selectedOptions){
                this.removeSelectedOption(option)
            }
        },
        getSelectedOptionsData(){
            const selectedids = selectedOptions.map((option) => option.id);
            return selectedids.map((id) => this.optionsData.find((option) => option.ref == id))
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