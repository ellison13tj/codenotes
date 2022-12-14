<!--
 * @Author: ellison13tj@gmail.com
 * @Date: 2022-12-14 11:49:34
 * @LastEditTime: 2022-12-14 11:50:28
 * @LastEditors: ellison13tj@gmail.com
 * @FilePath: /docs/vue3plugin/js-request-action.md
 * @Description: 
-->
# JS请求并带参数
```javascript
var ids=['037c11bb9bdc4c','037eb9f3278f40609cc0282'];
var idstr = JSON.stringify(ids);
var confName="任务jjj";
var jobCron="0 * 0 1 * ?";
var type="指定";
var url = "http://ip:port/dgov/pc/addConf"
var params = "jobId="+"&confName="+confName+"&jobCron="+jobCron+"&type="+type+"&ids="+idstr;;
 
var xhr = new XMLHttpRequest();
xhr.open("POST", url, true);
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
xhr.onload = function (e) {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(xhr.responseText);
    } else {
      console.error(xhr.statusText);
    }
  }
};
xhr.onerror = function (e) {
  console.error(xhr.statusText);
};
 
xhr.send(params);
```