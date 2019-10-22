<template>
  <component :is="getItem()" :type="type" v-bind="$attrs"  @change="handleChange" ></component>
  <!-- <component :is="getItem()" :type="type"  @change="handleChange" ></component> -->
</template>
<script lang="ts">

import Input from '../component/Input.vue'
import DataPicker from '../component/DataPicker.vue'
import MouthPicker from '../component/MouthPicker.vue'
import DataRangePicker from '../component/DataRangePicker.vue'
import MouthRangePicker from '../component/MouthRangePicker.vue'
import Select from '../component/Select.vue'
import { Component, Vue, Prop, Watch } from "vue-property-decorator";

const ComponentMap = [
  { type:"input",component: "Input" },
  { type:"select",component: "Select" },
  { type:"cascader",component: "el-cascader" },
  { type:"date",component: "DataPicker" },
  { type:"daterange",component: "DataRangePicker" },
  { type:"month",component: "MouthPicker"},
  { type:"monthrange",component: "MouthRangePicker"},
]

@Component({ components: {Input,DataPicker,DataRangePicker,Select,MouthPicker,MouthRangePicker} })
export default class ComponentFactory extends Vue {

  @Prop({type: String,default:'',required: true})
  private type: String;

  // @Prop({type: Object,default(){return {}},required: true})
  // private prop: Object;

  // @Prop({type: Object,default(){return {} },required: true})
  // private options: Object;

  private getItem():String{
    let item:any = ComponentMap.find((item)=>{
      return item.type === this.type
    })
    if(item!== undefined){
      return item.component
    }else{
      return ""
    }
  }
  private handleChange(value:any):void{
    this.$emit("change",value)
  }
}
</script>
