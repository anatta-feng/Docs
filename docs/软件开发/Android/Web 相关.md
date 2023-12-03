# Web 相关

## WebSettings 常用方法

| setAllowFileAccess         | 启用或禁用WebView访问文件数据 |
| -------------------------- | ------------------ |
| setBlockNetworkImage       | 是否显示网络图像           |
| setBuiltInZoomControls     | 设置是否支持缩放           |
| setCacheMode               | 设置缓冲的模式            |
| setDefaultFontSize         | 设置默认的字体大小          |
| setDefaultTextEncodingName | 设置在解码时时候用的默认编码     |
| setFixedFontFamily         | 设置固定使用的字体          |
| setJavaScriptEnabled       | 设置是否支持Javascript   |
| setLayoutAlgorithm         | 设置布局方式             |
| setLightTouchEnabled       | 设置用鼠标激活被选项         |
| setSupportZoom             | 设置是否支持变焦           |

## WebViewClient 常用方法

| doUpdateVisitedHistory   | 更新历史记录              |
| ------------------------ | ------------------- |
| onFormResubmission       | 应用程序重新请求网页数据        |
| onLoadResource           | 加载指定地址提供的资源         |
| onPageFinished           | 网页加载完毕              |
| onPageStarted            | 网页开始加载              |
| onReceivedError          | 报告错误信息              |
| onScaleChanged           | WebView发生改变         |
| shouldOverrideUrlLoading | 控制新的连接在当前WebView中打开 |

