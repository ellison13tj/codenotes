<!--
 * @Author: ellison13tj@gmail.com
 * @Date: 2022-12-09 18:57:11
 * @LastEditTime: 2022-12-09 19:43:26
 * @LastEditors: ellison13tj@gmail.com
 * @FilePath: /docs/vue3plugin/element-plus-for-inport-and-export-excel.md
 * @Description: 
-->
# 基于Element Plus 表格导入和导出excel文件
## 安装依赖
```bash
npm install --save xlsx file-saver
//也可以使用cnpm安装
```
  
查看package.json是否安装成功，以下代表安装成功
```javascript
"file-saver": "^2.0.5",
"xlsx": "^0.18.5"
```
  
页面中引入：
```javascript
import FileSaver from 'file-saver'
import * as XLSX from 'xlsx'; //这里务必用“* as XLSX” 而不是直接“XLSX”，具体什么原因不清楚，待研究
```
  
## 导入功能
1、添加导入按钮，样式后期可自行设计
```html
<div class="m-btn">
    导入表格
    <input type="file" accept=".xls, .xlsx" class="upload-file" @change="changeExcel($event)" />
</div>
```
  
2、change时间函数
```javascript
const tableData = ref([]); //表格数据
const changeExcel = (e) => {
  const file = e.target.files;
  if(files.length <= 0) {
    return false
  } else if(!/\.(xls|xlsx)$/.test(files[0].name.toLowerCase())) {
    return false
  }
  // 读取表格数据
  const fileReader = new FileReader();
  fileReader.onload = ev => {
    const workbook = XLSX.read(ev.target.result, {
      type: "binary"
    })
    const wsname = workbook.SheetName[0];
    const ws = XLSX.utils.sheet_to_json(workbook.Sheets[wsname]);
    // 上面装换的数据格式键名是中文，下面再进行一次装换
    dealExcel(ws); //调用第3步的函数转换数据格式
    tableData.value = ws; //赋值
  }
  fileReader.readAsBinaryString(file[0])
}
```
  
3、转换数据格式
```javascript
const dealExcel = (ws) => {
  let keymap = {
    "姓名": "name",
    "年龄": "age",
    "班级": "class",
    "学号": "studentId",
  }
  ws.forEach(sourceObj => {
    Object.keys(sourceObj).map(key => {
      let newKey = keymap[keys];
      if (newKey) {
        sourceObj[newKey] = sourceObj[keys]
        delete sourceObj[keyss]
      }
    })
  })
}
```
  
  
## 导出功能
1、添加导出按钮以及点击事件
```html
<el-button type="primary" round @click="exportClick ">导出表格</el-button>
```
  
2、在table表格中添加id，导出的时候需要用到
```html
<!-- 添加id 这一步很重要 -->
<el-table 
  id="out-table" 
  :data="PowerList" border  
  style="width: 100%" 
  class="tableBox">
</el-table>
```
  
3、写导出事件的方法函数
```javascript
const exportClick = () => {
  /* 从表生成工作簿对象 */
	let wb = XLSX.utils.table_to_book(document.querySelector('#my-table')); //关联dom节点，项目中可以换Vue的方式写
	/* 获取二进制字符串作为输出 */
	let wbout = XLSX.write(wb, {
		bookType: 'xlsx',
		bookSST: true,
		type: 'array'
	})
	try {
		FileSaver.saveAs(
      //Blob 对象表示一个不可变、原始数据的类文件对象。
      //Blob 表示的不一定是JavaScript原生格式的数据。
      //File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。
      //返回一个新创建的 Blob 对象，其内容由参数中给定的数组串联组成。
      new Blob([wbout], {
			  type: 'application/octet-stream'
		  }), '学生表.xlsx') //自定义文件名
	} catch (e) {
		if (typeof console !== 'undefined') console.log(e, wbout);
	}
	return wbout
};
```
  
> 总结：如果报错的话，引入“XLSX”对象时一定记得采用“* as XLSX”，具体原因未知
> 
> 参考资料：
> + <https://blog.csdn.net/qq_44793507/article/details/126651218>
> + <https://blog.csdn.net/m0_64397933/article/details/125479568>