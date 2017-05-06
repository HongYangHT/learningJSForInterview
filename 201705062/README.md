### 文件上传补充学习

* 文件上传
```javascript
<input type="file" name="upload" id="upload" multiple> /* 多文件 multiple*/
```

  * 文件提交方式使用[FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/FormData)以及[FormData的使用](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects)对象
  * 上传文件进度条，使用onprogress 时间监听，再利用其他样式进行进度条的描述
  ```javascript
  xhr.upload.onprogress = function(event) {
    if (event.lengthComputable) {
        var percentComplete = (event.loaded / event.total) * 100;
        // 对进度进行处理
    }
  }
  ```

  * 多文件上传
  ```javascript
  var fileInput = document.getElementById("myFile");
  var files = fileInput.files;
  var formData = new FormData();

  for(var i = 0; i < files.length; i++) {
    var file = files[i];
    formData.append('files[]', file, file.name);
  }
  ```
* 图片预览
  1. HTML5的FileReader API, 直接用img 显示
  ```javascript
  function handleImageFile(file) {
       var previewArea = document.getElementById('previewArea');
       var img = document.createElement('img');
       var fileInput = document.getElementById("myFile");
       var file = fileInput.files[0];
       img.file = file;
       previewArea.appendChild(img);

       var reader = new FileReader();
       reader.onload = (function(aImg) {
            return function(e) {
                 aImg.src = e.target.result;
            }
       })(img);
       reader.readAsDataURL(file);
}
  ```
 2. HTML5的FileReader API, 直接用img 显示 但是利用的是canvas画出来
 
* 文件拖拽
  * HTML5的drag & drop
  ```javascript
    var dropArea;

    dropArea = document.getElementById("dropArea");
    dropArea.addEventListener("dragenter", handleDragenter, false);
    dropArea.addEventListener("dragover", handleDragover, false);
    dropArea.addEventListener("drop", handleDrop, false);

    // 阻止dragenter和dragover的默认行为，这样才能使drop事件被触发
    function handleDragenter(e) {
        e.stopPropagation();
        e.preventDefault();
    }

    function handleDragover(e) {
        e.stopPropagation();
        e.preventDefault();
    }

    function handleDrop(e) {
        e.stopPropagation();
        e.preventDefault();

        var dt = e.dataTransfer;
        var files = dt.files;

        // handle files ...
    }
  ```

ps：做成Vue组件，可通过一个拖拽指令v-drag 来实现拖拽

