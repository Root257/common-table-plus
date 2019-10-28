
<template>
  <div>
    <Search  v-bind="$attrs"   ref="Search"   v-if="formOptions.visible"  :FormOptions="formOptions"   @search="handleSearch" :loading="Loading"   >
      <template v-for="Item in formOptions.Items">
        <slot  v-if="Item.type ==='custom'&&Item.slotName"   :slot="Item.slotName"  :name="Item.slotName"   ></slot>
      </template>
    </Search>
    <slot name="buttons"/>
    <el-table v-loading.lock="Loading"
      ref="table"
      element-loading-text="拼命加载中"
      v-bind="$attrs" 
      :border="true"
      :data="Data"
      @select="(selection, row) => emitEventHandler('select', selection, row)"
      @select-all="selection => emitEventHandler('select-all', selection)"
      @selection-change="handleSelectionChange"
      @cell-mouse-enter="(row, column, cell, event) => emitEventHandler('cell-mouse-enter', row, column, cell, event)"
      @cell-mouse-leave="(row, column, cell, event) => emitEventHandler('cell-mouse-leave', row, column, cell, event)"
      @cell-click="(row, column, cell, event) => emitEventHandler('cell-click', row, column, cell, event)"
      @cell-dblclick="(row, column, cell, event) => emitEventHandler('cell-dblclick', row, column, cell, event)"
      @row-click="(row,column,event) => emitEventHandler('row-click', row,column,event)"
      @row-dblclick="(row, event) => emitEventHandler('row-dblclick', row, event)"
      @row-contextmenu="(row, event) => emitEventHandler('row-contextmenu', row, event)"
      @header-click="(column, event) => emitEventHandler('header-click', column, event)"
      @sort-change="args => emitEventHandler('sort-change', args)"
      @filter-change="filters=>handleFilterChange(filters)"
      @current-change="(currentRow, oldCurrentRow) => emitEventHandler('current-change', currentRow, oldCurrentRow)"
      @header-dragend="(newWidth, oldWidth, column, event) => emitEventHandler('header-dragend', newWidth, oldWidth, column, event)"
      @expand-change="(row, expanded) => emitEventHandler('expand-change', row, expanded)"
      >
      <slot name="prepend" />    
        <TableColumn   v-for="(column, columnIndex) in columns"  :key="columnIndex"  :column="column"  >
          <slot  v-if="column.slotName"   slot-scope="scope"  :row="scope.row" :$index="scope.$index"  :slot="column.slotName"  :name="column.slotName"   ></slot>
        </TableColumn>
      <slot name="append" />
    </el-table>
    <span v-if="ExportOptions.Visible" style="float:left;margin-top: 10px;">
      <el-select v-model="ExportSelect" placeholder="请选择导出数据范围">
        <el-option    v-for="(option, index) in ExportOptions.Options"
          :key="index"
          :label="option.label"
          :value="option.value">
        </el-option>
      </el-select>
      <el-button  @click="handleExport" icon="el-icon-download">导出</el-button>
    </span>
    <div v-if="PaginationOptions.Visible" style="margin-top: 10px;">
      <el-pagination background style="float: right;"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="Pagination.currentPage"
        :page-size="Pagination.pageSize"
        :layout="PaginationOptions.Layout"
        :total="Total">
        <slot>  <el-button  @click="handleExport" plain>查看全部</el-button>  <span class="el-pagination__total" >选中{{SelectionCount}}条</span></slot>
      </el-pagination>
      <el-button  style="float: right;" icon="el-icon-refresh-right" type="success" @click="handleRefresh">刷新</el-button>
    </div>
  </div>
</template>

<script lang="tsx">
import Search from './search/index.vue'
import TableColumn from './Table/TableColumn.vue'
import { Component, Vue, Prop, Watch } from "vue-property-decorator";


@Component({ components: {Search,TableColumn} })
export default class Index extends Vue {

  //是否显示导出
  @Prop({type: Object,default(){return {Items:[]}},required: false})
  private formOptions: any;

  // @Watch("FormOptions", { immediate: true, deep: true })
  // onFormItemsChanged(newV: any, oldV: Object) {
  //   this.FormItemsSlot =  newV.Items.filter((item:any)=>{
  //     return 
  //   })
  // }
     

  private ExportSelect:String|Number = "导出当前"
  // 导出配置项
  @Prop({type: Object,default(){return {}},required: false})
  private exportOptions: Array<any>;

  @Watch("exportOptions", { immediate: true, deep: true })
  onExportOptionsChanged(newV: Object, oldV: Object) { 
    this.ExportOptions =  Object.assign(this.ExportOptions,newV)
  }
 //导出配置项
  private ExportOptions:any={
    Visible:false,
    Key:"export",
    Options:[{label: "导出当前",value: "exportCurrent",},{label: "导出全部",value: "exportAll",}]
  }

  private Selection:Array<any>=[]
  private SelectionCount:Number=0

  //分页配置项
  @Prop({type: Object,default(){return {}},required: false})
  private paginationOptions: Array<any>;

  @Watch("paginationOptions", { immediate: true, deep: true })
  onPaginationOptionsChanged(newV: Object, oldV: Object) { 
    this.PaginationOptions =  Object.assign(this.PaginationOptions,newV)
  }
 //分页配置项
  private PaginationOptions:any={
    Visible:true,
    IndexKey:"currentPage",
    PageSizeKey:"pageSize",
    Layout:'total,slot,prev, pager, next, jumper, sizes',
    PageSizes:[10, 20, 50, 100, 500, 1000]
  }
  handleSelectionChange(selection:Array<any>){
    this.Selection = selection
    this.SelectionCount = selection.length
  }
  //返回已经选择的行数据
  public getSelectionRows():Array<any>{
    return this.Selection
  }

  //选择行
  public toggleSelection(rows:Array<any>) {
    if (rows) {
        rows.forEach(row => {
          let ref:any = this.$refs.table
          ref.toggleRowSelection(row)
        })
      }
      this.init()
    }




  // @Prop({type: Array,default(){return },required: false})
  // private PageSizes: Array<any>;

  // @Prop({type: Boolean, default: true, required: false })
  // private ShowPagination: Boolean;
  
  // @Prop({ type:String, default:, required: false })
  // private PaginationLayout:String;

  // @Prop({ type:String, default:'currentPage', required: false })
  // private PageIndexKey:string;

  // @Prop({ type:String, default:'pageSize', required: false })
  // private PageSizeKey:string;

  // //导出选项的key
  // @Prop({ type:String, default:'export', required: false })
  // private ExportKey:string;
  
  //数据对象中数据项的key
  // @Prop({ type:String, default:'items', required: false })
  // private DataKey:string;
  //数据对象中数据总数量的key
  // @Prop({ type:String, default:'total', required: false })
  // private TotalKey:string;





   //表格配置项
  @Prop({type: Object,default(){return {}},required: false})
  private tableOptions: Object;

  @Watch("tableOptions", { immediate: true, deep: true })
  onTableOptionsChanged(newV: Object, oldV: Object) { 
    this.TableOptions =  Object.assign(this.TableOptions,newV)
  }
 //表格配置项
  private TableOptions: any={
    DataKey:"items",
    TotalKey:"total",
    Remote:true,
    AutoLoad:true,
    RemoteMethod:null
  };




 


  //表格的列数据
  @Prop({type: Array,default(){return []},required: false})
  private columns: Array<any>;



   //表格数据
  @Prop({type: Object,default(){return {}},required: false})
  private data: Object;

  @Watch("data", { immediate: true, deep: true })
  onDataChanged(newV: Array<any>, oldV: Array<any>) {  
    this.Data = newV
  }
   //表格数据
  private  Data:Array<any> = []


  //外部参数
  @Prop({type: Object,default(){return {}},required: false})
  private params: Object;

  // private FormItemsSlot:Array<any> = []
  
  // private  ColumnsFilter:Array<any>=[];

  // 加载状态
  // @Prop({type: Boolean, default: true, required: false })
  private Loading: Boolean =false;


  private FiltersParams:Object = {}


  
 

   //表格数据
  private  Total:Number = 0

  private Pagination:any = {
    currentPage: 1,
    pageSize: (() => {
      if (this.PaginationOptions.PageSizes.length > 0) {
        return this.PaginationOptions.PageSizes[0]
      }
      return 20
    })()
  }
 
  mounted () {
    this.init()
  }
  init(){
    if (this.TableOptions.Remote&&this.TableOptions.AutoLoad&&this.TableOptions.RemoteMethod) {
      let Params = this.getParams()
      this.getRemoteData(Params)
      // this.init()
    }
  }

  //获取远程数据
  private  getRemoteData(Params:Object){
    this.Loading =true
    this.TableOptions.RemoteMethod(Params).then((result:any)=>{
      console.log(result)
      this.Data = result[this.TableOptions.DataKey]
      this.Total = result[this.TableOptions.TotalKey]
      this.Loading =false
    }).catch((err:any)=>{
      this.Data = []
      this.Total = 0
      this.Loading =false
    })

    

    // this.Loading = true
    // this.RemoteMethod(Params).then((result:any)=>{
    //   console.log(result)
    //   this.Data = result[this.DataKey]
    //   this.Total = result[this.TotalKey]
    // })
    // if(res.code === 20000){
     
    //   // this.Total = res.data.total
    //   this.Loading = false
    // }
    // this.$emit("RemoteMethod",Params)
  }

  // private handleReset(){
  //   this.FiltersParams = {}
  // }

  private handleSearch(SearchParams:Object){
    let  Params = {}
    Params = Object.assign(Params,SearchParams,this.getPageParams(),this.FiltersParams,this.params)
    this.getRemoteData(Params)
  }

  //获取筛选条件参数
  private getSearchParams():Object {
    if(this.formOptions.visible === true){
      let ref:any = this.$refs.Search
      let SearchParams:Object = ref.getParams()
      return  SearchParams
    }else{
       return  {}
    }
  }

  //获取分页条件参数
  private getPageParams():Object {
    let PageParams = {}
    if (this.PaginationOptions.Visible) {
        PageParams  = Object.assign(PageParams, {
        [this.PaginationOptions.IndexKey]: this.Pagination.currentPage,
        [this.PaginationOptions.PageSizeKey]: this.Pagination.pageSize
      })
    }
    return  PageParams
  }
  //获取查询参数
  private getParams():Object {
    let Params = {}
    Params =  Object.assign(Params,this.getSearchParams(),this.getPageParams(),this.FiltersParams,this.params)
    return  Params
  }

  private handleRefresh(){
    this.refresh()
  }

  public refresh() {
    let Params = this.getParams()
    this.getRemoteData(Params)
  }

  private handleSizeChange(size:Number) {
    this.Pagination.pageSize = size
    let Params = this.getParams()
    this.getRemoteData(Params)
  }
  private handleCurrentChange(currentPage:Number) {
    this.Pagination.currentPage = currentPage
    let Params = this.getParams()
    this.getRemoteData(Params)
  }

  private handleExport() {
    let ExportParams={[this.ExportOptions.Key]:this.ExportSelect}
    let Params = Object.assign({},this.getParams(),ExportParams)
    this.$emit("export", Params)
  }

  private handleFilterChange(filters:any){

    let keys = Object.keys(filters)
    let value:String = filters[keys[0]].join(',')
    this.FiltersParams = Object.assign(this.FiltersParams,{[keys[0]]:value})
    console.log(this.FiltersParams)

    let Params = this.getParams()
    // Params = Object.assign(Params,this.FiltersParams)
    this.getRemoteData(Params)    
  }

  private emitEventHandler(event:any) {
    // if (event === 'select' || event === 'select-all') {
    //   this.$set(this, 'selectData', ...Array.from(arguments).slice(1))
    // }
    this.$emit(event, ...Array.from(arguments))
  }

}
</script>
