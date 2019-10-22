<template>
  <el-date-picker
  v-model="Value"
  :type="type" 
  :placeholder="placeholder"
  :start-placeholder="startPlaceholder"
  :end-placeholder="endPlaceholder"
  :size="size ? size : 'mini'"
  :value-format="format ? format :'yyyy-MM'"
  :disabled="disabled"
  :readonly="readonly"
  :editable="editable"
  :picker-options="pickerOptions || {}" 
  @change="handleMouthPickerChange"
  />
</template>
<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import DataPicker from './DataPicker.vue'
@Component({ components: { } })
export default class MouthPicker extends DataPicker {

  private handleMouthPickerChange(value:Array<String>):void{
    let prop = Object.assign({},this.prop)
    let keys = Object.keys(this.prop)
    this.$emit("change",{[keys[0]]:value})
  }
}
</script>
