<template>
  <el-form  v-if="Fresh"  :model="FormData" :inline="FormOptions.inline" ref="form" @submit.native.prevent="searchHandler()"
      v-bind="FormOptions"  >
      <el-form-item v-for="(Item, index) in FormOptions.Items" :key="index" :required="Item.required"
        :label="Item.label" 
        :rules="Item.rules || []"
        :label-width="Item.labelWidth ? (Item.labelWidth + 'px') : ''"
        style="text-align:left">
         <!-- <ComponentFactory v-bind="Item"  @change="handleChange"></ComponentFactory> -->
         <template v-if="Item.type==='custom'&&Item.slotName">
            <slot   :name="Item.slotName" ></slot>
         </template>
         <template v-else> 
           <ComponentFactory   v-bind="Item"  @change="handleChange"></ComponentFactory>
         </template>
       
         

      </el-form-item> 

       <!-- <el-form-item v-for="(Item, index) in FormItems" :key="index" :required="Item.required"
        :label="Item.label" 
        :rules="Item.rules || []"
        :label-width="Item.labelWidth ? (Item.labelWidth + 'px') : ''"
        style="text-align:left">
         
       </el-form-item>  -->


       <el-form-item label="" style="text-align:left">
          <el-button v-if="FormOptions.showSearchBtn"
            type="primary"
            :size="size"
            @click.stop
            svgIcon="search"
            @click="handlerSearch"
            :loading="loading">
            {{ FormOptions.submitBtnText?FormOptions.submitBtnText:"查询"}}
          </el-button>
          <el-button type="primary" :plain="true"
            :size="size" v-if="FormOptions.showResetBtn"
            @click="handlerReset"
             svgIcon = "reset"
            @click.stop
            >
            {{ FormOptions.resetBtnText?FormOptions.resetBtnText:"重置"}}
          </el-button>
          <slot name="formbutton"></slot>
        </el-form-item>
  </el-form>
</template>

<script lang="tsx">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import ComponentFactory from './ComponentFactory.vue'

@Component({ components: { ComponentFactory} })
export default class Index extends Vue {

  private FormData: Object = {};
  //加载状态
  @Prop({type: Boolean, default: false, required: false })
  private loading: Boolean;

  //表单配置项
  @Prop({type: Object,default(){return {Items:[]}},required: false})
  private FormOptions: any;

 //自定义表单组件
  @Prop({type: Array,default(){return []},required: false})
  private FormItems:Array<any>;
  

  private Fresh:Boolean = true

  mounted () {
    this.FormOptions.Items.forEach((item:any) => {
      this.FormData = Object.assign(this.FormData,item.prop)
    });
  }
  public getParams():Object {
    return this.FormData
  }
  //搜索方法
  private handlerSearch(value:Object){
    this.$emit("search",this.FormData)
  }
  //重置方法
  private handlerReset(){
    this.Fresh= false
    this.$nextTick(()=>{ this.Fresh = true })
    this.$emit("reset")
  }
  //参数变更方法
  private handleChange(value:Object){
    this.FormData = Object.assign(this.FormData,value)
  }
}
</script>
