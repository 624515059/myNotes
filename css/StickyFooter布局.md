### 概念

页面内容不够长的时候，页脚块粘贴在视窗底部；

如果内容足够长时，页脚块会被内容向下推送；

### 实现

#### flex布局

flex布局对视窗高度进行分割。footer的flex设为0，这样footer获得其固有的高度;
content的flex设为1，这样它会充满除去footer的其他部分。

布局方式结构简单，代码量少.

``` html
<body>
　　<div class="content"></div>
　　<div class="footer"></div>
</body>
```

``` css
body { 
    display: flex; 
    flex-direction: column;
    min-height:100%;;
}
.content {
    flex: 1; 
}
.footer{
    flex: 0;
    /* height: 100px;  */
}
```

#### 负margin布局方式

兼容性好，必须知道footer高度，结构相对复杂。

``` html
<div class="wrapper clearfix">
    <div class="content">
      // 这里是页面内容
    </div>  
</div>
<div class="footer">
    // 这里是footer的内容
</div>
```

``` css
.wrapper {
    min-height: 100%;
}
.wrapper .content{
    padding-bottom: 50px; /* footer区块的高度 */
}
.footer {
    position: relative;
    margin-top: -50px;  /* 使footer区块正好处于content的padding-bottom位置 */
    height: 50px;
    clear: both;
}
.clearfix::after {
    display: block;
    content: ".";
    height: 0;
    clear: both;
    visibility: hidden;
}
```