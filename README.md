# html导出为pdf

利用开源软件【html2pdf.bundle.js】把html导出为pdf，采用异步形式，支持批量导出


## 引入 html2pdf.bundle.js 文件
 ``` javascript

     ---本实例采用的是本地版本，请自行替换
    <script src="./dist/html2pdf.bundle.js"></script>
```
## 自定义的css演示

``` css
      .button {
            background-color: #3296FA;
            border: none;
            color: white;
            padding: 15px 32px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin: 4px 2px;
            cursor: pointer;
            position: fixed;
            right: 3%;
            bottom: 50%;
        }

        .btnOne {
            bottom: 30% !important;
        }

        #Form1 {
            width: 95%;
            margin: 0 auto;
        }

        .main_p_print {
            padding-left: 9px;
            width: 95%;
            margin: 0 auto;
        }

        .PopTip {
            position: fixed;
            top: 50px;
            right: 38px;
            font-size: 16px;
        }

        .PopTip span {
            color: red;
            font-size: inherit;
            display: inline-block;
            padding: 0 5px;
        }
```

## html dom元素

```html
<div class="PopTip">
  请用非<span>IE浏览器</span>打印。<br>
  建议使用Goolge Chrome <br> 或者 火狐浏览器（Firefox）
</div>

```



 ## JavaScript源码案列

``` javascript

  async function exportPDFs() {
    if (!confirm("是否确认导出？批量导出时请耐心等待。")) {
      return false;
    }

    var main_d = document.getElementsByClassName('main_d');
    var index = 0;

    async function exportNextPDF() {
      if (index < main_d.length) {
        var element = main_d[index];
        element.classList.add('main_p_print');
        var _index = index < 10 ? "0" + index : index;
        var pdf_name = document.getElementById('ctl' + _index + '_stuname').innerText;
        var pdf_claname = document.getElementById('ctl' + _index + '_claname').innerText;
        var pdfFilename = pdf_claname + '--' + pdf_name;
        await PrintToPdf(element, pdfFilename);

        index++;
        setTimeout(exportNextPDF, 2000); // 设置2秒间隔时间
      }
    }

    await exportNextPDF();
  }

  function PrintToPdf(element, _name) {
    return new Promise((resolve) => {
      var mode = 'avoid-all';
      var pagebreak = (mode === 'specify') ?
        { mode: '', before: '.before', after: '.after', avoid: '.avoid' } :
        { mode: mode };

      html2pdf().from(element).set({
        margin: [-4, -8, 2, 1],
        filename: _name + '.pdf',
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        pagebreak: pagebreak,
        jsPDF: { orientation: 'landscape', unit: 'mm', format: 'a4', compressPDF: true }
      }).save().then(() => {
        // 处理完成后的回调函数
        resolve();
      });
    });
  }

  var btn = document.getElementById('btn');
  btn.addEventListener('click', exportPDFs);

```


    
