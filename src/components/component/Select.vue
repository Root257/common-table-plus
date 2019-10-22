<template>
  <el-select
    v-model="Value"
    style="width:100%"
    :multiple="Options.multiple"
    :filterable="Options.filterable"
    :collapse-tags="Options.collapseTags"
    :remote="Options.remote"
    :disabled="Options.disabled"    
    :remote-method="handleRemoteMethod"
    :loading="Loading"
    :placeholder="Options.placeholder"
    :clearable="Options.clearable"
    @change="handleChange"
  >
    
    <el-option 
      v-for="(item,index) in Options.data"
      :key="index"
      :label="item[Options.nameKey]"
      :value="item[Options.valueKey]">
      {{item[Options.nameKey]}}
    </el-option>
  </el-select>
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
// interface OptionsType {
//    data:Array<any>,
//    autoLoad:Boolean,
//    remoteMethod:any,
// }


@Component({ components: { } })
export default class Select extends Vue {

  //表单配置项
  @Prop({type: Object,default(){return {}},required: false})
  private options:Object;
  
  @Watch("options", { immediate: true, deep: true })
  onDataChanged(newV: Object, oldV: Object) { 
    this.Options =  Object.assign(this.Options,newV)
  }
  
  private Loading:Boolean=false
  
  mounted () {
    if(this.Options.autoLoad===true&&this.Options.remoteMethod){
      this.Loading = true
      this.Options.remoteMethod().then((result:any)=>{
        this.Options.data = result
        this.Loading = false
      })
    }
  }
  private handleRemoteMethod(query:String){
    if(this.Options.remoteMethod){
      this.Loading = true
      this.Options.remoteMethod(query).then((result:any)=>{
        this.Options.data = result
        this.Loading = false
      })
    }
  }

  private Options:any=  { nameKey:'name',
                          valueKey:'value',
                          disabled:false,
                          filterable:true,
                          clearable:false,
                          placeholder:"请选择",
                          multiple:true,
                          remote:true,
                          data:[],
                          autoLoad:false,
                          remoteMethod:null,
                          collapseTags:true
                          };
  
  private Value:String|Number =''
 
    

  @Prop({type: Object,default(){return{}},required: true})
  protected prop: Object;

  private handleChange(value:any):void{
    let prop = Object.assign({},this.prop)
    let keys = Object.keys(this.prop)
    let res:any=null
    if(this.Options.multiple=true){
      res = value.join(',')
    }
    this.$emit("change",{[keys[0]]:res})
  }
 
}
</script>