jeecg前端vue多个附件上传

```javascript
edit (record) {
  this.form.resetFields();
  this.model = Object.assign({}, record);

  if(record.other){
    this.initFileList(record.other,'contractList');
  }else{
   this.contractList=[]
  }
  this.visible = true;
```

调用model编辑时，若字符串变量other为空，则list为空，若非空，则执行initFileList进行数组初始化

```javascript
initFileList(urlString,list){
  var tmp= new Array();//临时变量，保存分割字符串
  tmp = urlString.split(",");//按照"/"分割
  this[list] = tmp.map((item, index) => {
    var url = item;
    var filename = this.getFileName(url);
    var extend = this.getFileExtend(filename);
    var showFileName = filename.split("_")[0] + "." + extend;
    var param = {};
    param.uid = index;
    param.name = showFileName;
    param.status = 'done';
    param.url = this.url.imgerver + "/" + url;
    param.thumbUrl = this.url.imgerver + "/" + url;
    var response = {};
    response.code = 1;
    response.message = url;
    param.response = response;
    return param;
  });
},
```

将字符串按逗号分割，再用map接收，才能实现数据更新，（vue的双向绑定原理），否则拿到other，但附件数组不会变化。

![image-20191106112917181](/Users/gtkjf/Library/Application Support/typora-user-images/image-20191106112917181.png)