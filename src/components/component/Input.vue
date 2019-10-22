      
<template>
  <el-input 
     v-model="Value"
    :size="size ? size : 'mini'"
    :readonly="readonly"
    :disabled="disabled"
    :placeholder="placeholder"
    :show-password="showPassword"
    @change="handleChange"
     />
</template>
<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";

@Component({ components: { } })
export default class Input extends Vue {

  @Prop({type: String,default:'',required: true})
  private type: String;

  @Prop({type: Object,required: false})
  private prop: object;

  // @Prop({type: String,default:"",required: true})
  // private prop: String;

  @Prop({type: String,default:'mini',required: false})
  private size: String;

  @Prop({type: Boolean,default:false,required: false})
  private readonly: Boolean;

  @Prop({type: Boolean,default:false,required: false})
  private showPassword: Boolean;

  @Prop({type: Boolean,default:false,required: false})
  private disabled: Boolean;  

  @Prop({type: String,default:'',required: false})
  private placeholder: String;

  private Value:String=''

  private handleChange(value:String|Number):void{
    let keys = Object.keys(this.prop)
    this.$emit("change",{[keys[0]]:value})
  }
 
}
</script>
