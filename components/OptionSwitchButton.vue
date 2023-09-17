<template>
<div class="option_switch_button">
    <input type="button" v-on:click="switchOption" :value="currentOption"/>
</div>
</template>

<script>

export default{
    props:{
        "switch-options": {required: true}
    },
    data(){
        return {
            selectedIndex: -1,
            currentOption: "null",
        }
    },
    mounted(){
        setTimeout(() => {
            this.currentOption = this.getNextOption();
        }, 1);
    },
    created(){

    },
    methods:{
        switchOption(){
            this.currentOption = this.getNextOption();
            this.$emit("on-option-change", this.currentOption)
        },
        getNextOption(){2
            if (this.switchOptions.length == 0){
                return "null"
            }
            this.selectedIndex += 1
            if (this.switchOptions.length - 1 < this.selectedIndex){
                this.selectedIndex = 0
            }
            return this.switchOptions.at(this.selectedIndex)
        }
    }
}
</script>