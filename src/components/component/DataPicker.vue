<template>
  <el-date-picker
  v-model="Value"
  :type="type" 
  :placeholder="placeholder"
  :start-placeholder="startPlaceholder"
  :end-placeholder="endPlaceholder"
  :size="size ? size : 'mini'"
  :disabled="disabled"
  :readonly="readonly"
  :value-format="type==='datetime'?'yyyy-MM-dd HH:mm:ss':'yyyy-MM-dd'"
  :editable="editable"
  :picker-options="pickerOptions || {}" 
  @change="handleChange"
  />
</template>
<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";

@Component({ components: { } })
export default class DataPicker extends Vue {

  @Prop({type: String,default:'',required: true})
  protected type: String;

  @Prop({type: Object,default(){return{}},required: true})
  protected prop: Object;

  @Prop({type: String,default:'mini',required: false})
  private size: String;

  @Prop({type: Boolean,default:false,required: false})
  private readonly: Boolean;

  @Prop({type: Boolean,default:true,required: false})
  private editable: Boolean;

  @Prop({type: Boolean,default:false,required: false})
  private disabled: Boolean;  

  @Prop({type: String,default:'',required: false})
  private placeholder: String;


  @Prop({type: String,default:null,required: false})
  private format: String;

  @Prop({type: String,default:'',required: false})
  private startPlaceholder: String;

  @Prop({type: String,default:'',required: false})
  private endPlaceholder: String;

  @Prop({type:Object,default(){return{}},required: false})
  private pickerOptions: Object;

  private Value:any=''

  private handleChange(value:String|Number):void{
    let prop = Object.assign({},this.prop)
    let keys = Object.keys(this.prop)
    
    this.$emit("change",{[keys[0]]:value})
  }
 
}
</script>


