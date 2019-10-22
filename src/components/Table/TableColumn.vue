
<template>
  <el-table-column  
    :column-key="column.columnKey"
    :prop="column.prop"
    :type="column.colType"
    :label="column.label"
    :fixed="column.fixed"
    :width="column.width"
    :min-width="column.minWidth"
    :render-header="column.renderHeader"
    :sortable="column.sortable"
    :sort-method="column.method"
    :resizable="column.resizable"
    :formatter="column.formatter"
    :show-overflow-tooltip="column.showOverflowTooltip"
    :align="column.align"
    :header-align="column.headerAlign || column.align"
    :class-name="column.className"
    :label-class-name="column.labelClassName"
    :selectable="column.selectable"
    :reserve-selection="column.reserveSelection"
    :filters="column.filters"
    :filter-placement="column.filterPlacement"
    :filter-multiple="column.filterMultiple"
    :filter-method="column.filterMethod"
    :filtered-value="column.filteredValue"
  > 
     <template   slot-scope="scope" >   
       <slot  v-if="column.slotName"  :name="column.slotName" :row="scope.row"   :$index="scope.$index"  />
       <span v-else >{{ column.render ? column.render(scope.row) : scope.row[column.prop] }}</span>
     </template>
    <TableColumn v-for="(item, index) in column.children" :key="index" :column="item"></TableColumn>  
  </el-table-column>
</template>

<script lang="tsx">

import { Component, Vue, Prop, Watch } from "vue-property-decorator";

@Component({ components: {} })
export default class TableColumn extends Vue {
  //列数据
  @Prop({type: Object,default(){return {}},required: false})
  private column: Object;
}
</script>
