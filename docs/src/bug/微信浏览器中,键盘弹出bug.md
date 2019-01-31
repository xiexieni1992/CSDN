##### 复现：

```
1、ios12微信H5页面里，输入框获取焦点弹起输入法，点击空白处或点击键盘的“完成”收起键盘后，页面没有下来，底部有一片灰色空白区域。。。
2、微信IOS 6.7.4 键盘弹起页面上滑，键盘收起页面不会回到原位置，导致弹框(css设置position为fixed会有问题，absolute不会有问题)后按钮响应区域错位
```

##### 解决方案：

```
if(_.isIOS()){
    window.addEventListener('focusout', function () {
    　　    //软键盘收起的事件处理
        setTimeout(()=>{
            window.scrollTo(0 ,document.documentElement.scrollTop 
            || document.body.scrollTop);
        })
    });
}
```

