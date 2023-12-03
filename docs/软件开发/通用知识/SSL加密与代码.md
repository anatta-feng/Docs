---
title: SSL加密与代码
date: 2017-08-2 12:15:00
categories: [Java] 
tags: [Java,网络请求]
---

> 最近项目使用 Https 双端验证，需要 SSL 相关知识

原理什么的先放下，先会用，再看原理，会通顺很多

# Demo 原型

一个 Socket 服务器与客户端

## 服务器

```java
package com.ssl.server;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class Server extends Thread {

    private Socket mSocket;

    public Server(Socket mSocket) {
        this.mSocket = mSocket;
    }

    @Override
    public void run() {
        BufferedReader reader = null;
        PrintWriter writer = null;
        try {
            reader = new BufferedReader(new InputStreamReader(mSocket.getInputStream()));
            writer = new PrintWriter(mSocket.getOutputStream());
            String data = reader.readLine();
            System.out.println(data);
            writer.println(data);
            writer.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                }
                if (writer != null) {
                    writer.close();
                }
                if (mSocket != null) {
                    mSocket.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }

    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8080);
        while (true) {
            new Server(serverSocket.accept()).start();
            System.out.println("a client connect");
        }
    }
}
```

## 客户端

```java
package com.ssl.client;

import java.io.*;
import java.net.Socket;

public class Client {
    public static void main(String[] args) throws IOException {
        Socket mSocket = new Socket("localhost", 8080);

        PrintWriter writer = new PrintWriter(mSocket.getOutputStream());
        BufferedReader reader = new BufferedReader(new InputStreamReader(mSocket.getInputStream()));

        writer.println("hello");

        writer.flush();

        System.out.println(reader.readLine());

        mSocket.close();
    }
}
```

# 使用 keytool 生成证书

在 Java 环境下使用 keytool 生成证书是比较方便的

首先生成服务器的证书

```shell
keytool -genkey -v -alias bluedash-ssl-demo-server -keyalg RSA -keystore ./server_ks -dname"CN=localhost,OU=cn,O=cn,L=cn,ST=cn,C=cn" -storepass server -keypass 123123
```

改造服务端代码

```java
public class Server extends Thread {

    private Socket mSocket;

    public Server(Socket mSocket) {
        this.mSocket = mSocket;
    }

    @Override
    public void run() {
        BufferedReader reader = null;
        PrintWriter writer = null;
        try {
            reader = new BufferedReader(new InputStreamReader(mSocket.getInputStream()));
            writer = new PrintWriter(mSocket.getOutputStream());
            String data = reader.readLine();
            System.out.println(data);
            writer.println(data);
            writer.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
			...
        }
    }

  	private static final String SERVER_KEY_STORE = "/Users/fxc/server_ks";
  	private static final String SERVER_KEY_PASSWD = "123123";
  
    public static void main(String[] args) throws IOException {
        System.setProperty("javax.net.ssl.trustStore", SERVER_KEY_STORE);
      	SSLContext ctx = SSLContext.getInstance("TLS");
      
      	KeyStore ks = KeyStore.getInstance("jceks");
      	ks.load(new FileInputStream(SERVER_KEY_STORE), null);
    }
}
```

