<template>
    <div>
        <input class="checkbox" type="checkbox" :id="actualid" @change="updateInput"/>
        <label :for="actualid">{{labelText}}</label>
    </div>
</template>

<script>
import { generateShortUID } from '~/scripts/shortUIDGenerator'

export default{
    props: {
        "id" : {default: null},
        "uncheckedText" : {default: null},
        "checkedText" : {default: null}
    },
    data(){
        return {
            labelText: null,
            actualid: null,
            labelUnchecked: this.uncheckedText,
            labelChecked: this.checkedText,
            isChecked: false
        }
    },
    watch: {
        labelChecked(val){
            this.updateTextLabel();
        }
    },
    mounted(){
        if (!this.id){
            this.actualid = "checkbox_" + generateShortUID()
        } 
        else {
            this.actualid = this.id
        }
        this.labelText = this.labelUnchecked;
        setTimeout(() => {
        this.setUnselected();
        }, 1);
    },
    methods: {
        updateInput(event){
            this.isChecked = event.target.checked;
            this.$emit("onClick", this);
            this.updateTextLabel();
        },
        updateTextLabel(){
            if (!this.isChecked){
                this.labelText = this.labelUnchecked;
            }
            else{
                this.labelText = this.labelChecked;
            }
        },
        setUnselected(){
            this.$el.querySelector(`#${this.actualid}`).checked = false;
            this.isChecked = false;
            this.updateTextLabel();
        },
        setSelected(){
            this.$el.querySelector(`#${this.actualid}`).checked = true;
            this.isChecked = true;
            this.updateTextLabel();
        },
    }
    
}
</script>

<style>
input[type="checkbox"]:not(:checked), 
input[type="checkbox"]:checked {
  position: absolute;
  visibility: hidden;
}
input[type="checkbox"] + label {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
  width: 35px;
  height: 35px;
  cursor: pointer;
  border: 2px solid black;
  border-radius: 10px;
  color: black;
  background-color: white;
  margin-bottom: 10px;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

input[type="checkbox"]:checked + label {
  border: 1px solid white;
  color: white;
  background-color: black;
}
</style>