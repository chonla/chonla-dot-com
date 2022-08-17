---
title: "Content and Map of this Source is not available"
date: 2022-07-01T14:01:34+07:00
draft: false
summary: การแก้ปัญหา Content and Map of this Source is not available (only size() is supported) บน Angular
cover_image: webpack-sizeonlysource-error-cover.png
slug: webpack-sizeonlysource-error
tags:
    - angular
    - troubleshoot
---

ตอนจะรัน `ng serve` แล้วดันเจอปัญหา ขึ้น error ประมาณนี้

```shell
/Users/chonla/<redacted>/node_modules/webpack-sources/lib/SizeOnlySource.js:16
		return new Error(
		       ^

Error: Content and Map of this Source is not available (only size() is supported)
    at SizeOnlySource._error (/Users/chonla/<redacted>/node_modules/webpack-sources/lib/SizeOnlySource.js:16:10)
    at SizeOnlySource.buffer (/Users/chonla/<redacted>/node_modules/webpack-sources/lib/SizeOnlySource.js:30:14)
    at _isSourceEqual (/Users/chonla/<redacted>/node_modules/webpack/lib/util/source.js:21:51)
    at isSourceEqual (/Users/chonla/<redacted>/node_modules/webpack/lib/util/source.js:43:17)
    at Compilation.emitAsset (/Users/chonla/<redacted>/node_modules/webpack/lib/Compilation.js:4239:9)
    at /Users/chonla/<redacted>/node_modules/webpack/lib/Compiler.js:553:28
    at /Users/chonla/<redacted>/node_modules/webpack/lib/Compiler.js:1183:17
    at eval (eval at create (/Users/chonla/<redacted>/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:13:1)
    at processTicksAndRejections (node:internal/process/task_queues:96:5)
```

เลยไป google ดู ก็เจอว่า มันเป็นปัญหาของ webpack เองทีอาจจะ build อะไรค้างไว้ครึ่ง ๆ กลาง ๆ แล้วพัง

## วิธีแก้ไข

ลบ path `.angular` ออกจาก project แล้ว run ใหม่ก็ใช้ได้แล้ว

แบบนี้

```shell
rm -rf .angular
```

เป็นอันจบ