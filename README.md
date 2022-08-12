# javascript-common

javascript通用方法汇总

## 方法

#### 加载css与js文件

```
function asyncLoadJs(url) {
    return new Promise((resolve, reject) => {
        let srcArr = document.getElementsByTagName("script");
        let hasLoaded = false;
        for (let i=0;i<srcArr.length;i++){//判断当前js是否加载上
            hasLoaded = (srcArr[i].src === url);
        }

        if (hasLoaded) {
            resolve();
            return;
        }

        // 创建
        let script = document.createElement("script");
        script.type = "text/javascript";
        script.src = url;
        document.body.appendChild(script);
        script.onload = () => {
            resolve();
        }
        script.onerror = () => {
            reject();
        }
    });
}

function loadCss(url) {
    let css = document.createElement("link");
    css.href = url;
    css.rel = "stylesheet";
    css.type = "text/css";
    document.head.appendChild(css);
}
```

#### 字节转化单位显示

```
function bytesToSize(bytes) {
    if (bytes === 0) return '0 B';
    var k = 1024;
    let sizes = ['B', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'];
    let i = Math.floor(Math.log(bytes) / Math.log(k));
    return (bytes / Math.pow(k, i)).toFixed(2) + ' ' + sizes[i];
}
```

## 工具类

#### 窗口大小改变触发事件管理工具

```
class ResizeTool {
    constructor() {
        this.onresize = [];
    }

    // 启动
    doit() {
        let $this = this;
        window.onresize = function () {
            $this.onresize.forEach(r => {
                r.fn();
            })
        }
        return this;
    }

    addOnResize(fn,name) {
        this.onresize.push({fn,name: name || `__${new Date().getTime()}__`});
        return this;
    }

    removeOnResize(name) {
        for (let i = 0;i < this.onresize.length;i++) {
            if (this.onresize[i].name === name) {
                this.onresize.splice(i,1);
                return this;
            }
        }
        return this;
    }
}
```