# Http Server

## 介紹

1. Http Method:

   - Get
   - Post
   - Put
   - Delete

2. Request
   ```
   GET /index.html HTTP/1.1
   HOST: "URL"
   <BLANK LINE>
   ```
3. Response

## 利用 Node.js 實作

### 1. 處理 Request/Response Object

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/html; charset=utf-8" });
  res.write("你好！ Welcome to NodeJS");
  res.end();
});

server.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

- 在 createServer() 裡的 callback function 操作，最後 res.end() 代表回應結束。
- 因為文字包含中文，我們就需要在 http header 設定 charset。

### 2. 針對不同路徑處理

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/html; charset=utf-8" });
  if (req.url == "/") res.write("你好！ Welcome to NodeJS");
  else if (req.url == "/another") res.write("Another page");
  else res.write("Not found");
  res.end();
});

server.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

- req.url

### 3. 回應一個 HTML 檔案

```js
const http = require("http");
const fs = require("fs");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/html; charset=utf-8" });
  if (req.url == "/") {
    res.write("你好！ Welcome to NodeJS");
    res.end();
  } else if (req.url == "/another") {
    res.write("Another page");
    res.end();
  } else if (req.url == "/index") {
    fs.readFile("index.html", (e, data) => {
      if (e) {
        res.write("File not found");
        res.end();
      } else {
        res.write(data);
        res.end();
      }
    });
  } else res.write("Not found");
});

server.listen(3000, () => {
  console.log("Listening on port 3000");
});
```
