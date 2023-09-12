<script setup lang="ts">
import { onMounted } from 'vue'
import { LuckyExcel } from '../luckyexcel/main'


onMounted(() => {
  window.appSettings = window.appSettings || {};
  // console.log('window.appSettings', window.appSettings)
  const name = window.appSettings.excelname
  const value = window.appSettings.excelurl
  LuckyExcel.transformExcelToLuckyByUrl(value, name, (exportJson, _luckysheetfile) => {
    if (exportJson.sheets == null || exportJson.sheets.length == 0) {
      alert('Failed to read the content of the excel file, currently does not support xls files!')
      return
    }
    // console.log('exportJson', exportJson)
    // jsonData.value = exportJson

    // isMaskShow.value = false

    isFunction(window?.luckysheet?.destroy) && window?.luckysheet.destroy()

    window?.luckysheet.create({
      container: 'luckysheet', //luckysheet is the container id
      data: exportJson.sheets,
      title: exportJson.info.name,
      userInfo: exportJson.info.creator,
      lang: "zh", // 设定表格语言 国际化设置，允许设置表格的语言，支持中文("zh")和英文("en")
      allowCopy: false, // 是否允许拷贝
      showtoolbar: false, // 是否显示工具栏
      showinfobar: false, // 是否显示顶部信息栏
      editMode: false,
      //showsheetbar: false, // 是否显示底部sheet页按钮
      showstatisticBar: false, // 是否显示底部计数栏
      sheetBottomConfig: false, // sheet页下方的添加行按钮和回到顶部按钮配置
      allowEdit: false, // 是否允许前台编辑
      enableAddRow: false, // 允许增加行
      enableAddCol: false, // 允许增加列
      // showRowBar: false, // 是否显示行号区域
      // showColumnBar: false, // 是否显示列号区域
      sheetFormulaBar: false, // 是否显示公式栏
      // enableAddBackTop: false,//返回头部按钮
      // rowHeaderWidth: 0,//纵坐标
      // columnHeaderHeight: 0,//横坐标
      // showstatisticBarConfig: {
      //   count: false,
      //   view: false,
      //   zoom: false,
      // },
      // showsheetbarConfig: {
      //   add: false, //新增sheet
      //   menu: false, //sheet管理菜单
      //   sheet: false, //sheet页显示
      // },
      // hook: {
      //   cellMousedown: this.cellMousedown,//绑定鼠标事件
      // },
    })
  })
})

function isFunction(val?: any) {
  return getType(val) === 'Function'
}

function getType(val?: any) {
  return Object.prototype.toString.call(val).slice(8, -1)
}
</script>

<template>
  <div id="luckysheet"></div>
</template>

<style scoped>
#luckysheet {
  margin: 0px;
  padding: 0px;
  position: absolute;
  width: 100%;
  left: 0px;
  top: 0px;
  bottom: 0px;
}
</style>
