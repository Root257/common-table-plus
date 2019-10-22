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
  :value-format="format ? format :'yyyy-MM'"
  :editable="editable"
  :picker-options="pickerOptions || {}" 
  @change="handleMouthRangePickerChange"
  />
</template>
<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import DataPicker from './DataPicker.vue'
@Component({ components: { } })
export default class DataRangePicker extends DataPicker {

  private handleMouthRangePickerChange(value:Array<String>):void{
    let keys = Object.keys(this.prop)
    this.$emit("change",{[keys[0]]:value[0],[keys[1]]:value[1]})
  }
 
}
</script>
