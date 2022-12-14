<!--
 * @Author: ellison13tj@gmail.com
 * @Date: 2022-12-14 11:55:04
 * @LastEditTime: 2022-12-14 21:32:06
 * @LastEditors: ellison13tj@gmail.com
 * @FilePath: /docs/vue3plugin/element-plus-for-export-pdf.md
 * @Description:
-->

# 基于 Element Plus 导出 PDF 文件

> 使用 html2Canvas 和 JsPDF 库，转化为图片后保存 PDF。

1.安装 html2Canvas 库

```bash
npm install html2canvas
```

2.安装 JsPDF 库

```bash
npm install jspdf
```

3.封装成工具，例如取名 htmlToPdf.js，后续使用时组件中引用即可

```javascript
import html2Canvas from 'html2canvas';
import JsPDF from 'jspdf';
const htmlToPdf = {
  getPdf(title) {
    html2Canvas(document.querySelector('#pdfDom'), {
      allowTaint: true,
    }).then((canvas) => {
      const contentWidth = canvas.width;
      const contentHeight = canvas.height;
      const pageHeight = (contentWidth / 592.28) * 841.89;
      let leftHeight = contentHeight;
      let position = 0;
      const imgWidth = 595.28;
      const imgHeight = (592.28 / contentWidth) * contentHeight;
      const pageData = canvas.toDataURL('image/jpeg', 1.0);
      const PDF = new JsPDF('', 'pt', 'a4');
      if (leftHeight < pageHeight) {
        PDF.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight);
      } else {
        while (leftHeight > 0) {
          PDF.addImage(pageData, 'JPEG', 0, position, imgWidth, imgHeight);
          leftHeight -= pageHeight;
          position -= 841.89;
          if (leftHeight > 0) {
            PDF.addPage();
          }
        }
      }
      PDF.save(title + '.pdf');
    });
  },
};
export default htmlToPdf;
```

4.在 VUE 组件中引用并使用封装的工具

````javascript
// 引用封装的工具
import htmlToPdf from './htmlToPdf.js'

// 使用工具（绑定到XX事件或按钮上）
exportPdf() {
  htmlToPdf.getPdf('导出的PDF')
}
  ```
````
