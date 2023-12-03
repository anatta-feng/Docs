---
title: OkHttp 学习
date: 2018-01-03 11:05:25
categories: [笔记]
tags: [Android, OkHttp, 网络]
---

初次体验，感觉最突出的模块就是：

* Client


* Request
* Call
* CallBack
* Response

# Get 请求实例

```Kotlin
val client = OkHttpClient()
fun get() {
  val request = Request.Builder().url(url).build()
  val call = client.newCall(request)
  val callback = object : Callback {
    override fun onFailure(call: Call, e: IOException) {
      //..
    }
	@Throws(IOException::class)
    override fun onResponse(call: Call, response: Response) {
      //..
    }
  }
  call.enqueue(callback)
}
```

