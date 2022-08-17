---
title: ส่ง Request Signature ไปบน Postman
date: 2022-07-22T17:57:34+07:00
draft: false
summary: การสร้าง Request Signature ด้วย HMAC บน Postman
cover_image: send-request-signature-with-postman-cover.png
slug: send-request-signature-with-postman
---

ที่มาที่ไปคือกำลังทำ API ที่ต้องมีการ Verify Request ที่ส่งมาด้วย Digital Signature เพื่อให้ฝั่ง Server ยอมรับได้ว่าคนที่ส่ง Request นั้นคือต้นทางที่เชื่อถือได้จริง ๆ โดยในการส่ง Digital Signature มาที่ API นี้จะกำหนดให้ส่งค่า 2 ค่ามาใน Request Header ค่าแรกทำหน้าที่ระบุตัวผู้ส่ง สมมติให้ชื่อว่า “X-RequestSender” และอีกค่าหนึ่งทำหน้าที่เป็น Digital Signature สมมติให้ชื่อว่า “X-RequestSignature” โดยค่าของ “X-RequestSender” จะเป็น string ธรรมดาที่บอกว่าใครส่งมา ส่วน “X-RequestSignature” จะเกิดจากการคำนวณตามแต่ละ Request ซึ่งถ้า Request ที่เหมือนกัน แต่คนส่งต่างกัน ค่า “X-RequestSignature” ก็จะต่างกันไปด้วย

วิธีการสร้าง Digital Signature สำหรับ request ที่เลือกมาใช้ จะใช้ อัลกอริธึม HMAC SHA256 ซึ่งเป็นมาตรฐานที่ใช้กันทั่วไป

ตัวแปรที่ HMAC SHA256 ต้องการก็มีแค่ 2 อย่างคือ Key ที่ใช้ในการ Sign และตัว Content ที่ต้องการ Sign หลังจากตรงนี้ จะของเรียก Key ว่า AppSecret และ Content ว่า PlainText โดยแต่ละภาษาก็มีวิธีการเขียนที่แตกต่างกัน ประมาณนี้

**Golang**

```go
hm := hmac.New(sha256.New, []byte(appSecret))
hm.Write(plainText)
signature := []byte(base64.StdEncoding.EncodeToString(hm.Sum(nil)))
```

**.NET**

```csharp
String signature = string.Empty;
byte[] unicodeKey = Convert.FromBase64String(appSecret);
using (HMACSHA256 hmacSha256 = new HMACSHA256(unicodeKey)) {
    Byte[] dataToHmac = System.Text.Encoding.UTF8.GetBytes(plainText);
    signature = Convert.ToBase64String(hmacSha256.ComputeHash(dataToHmac)); }
```

เสร็จแล้วเราก็เอา Digital Signature ที่ได้ ไปใส่ใน Header ด้วย Key ที่ต้องการ และส่งไปได้เลย ประมาณนี้

```http
POST /api/some-api HTTP/1.1
X-RequestSender: app-name-1
X-RequestSignature: J4xWW48OrBmJAxWS3Cw4S6R/4fnoKnt2mn83+yx40hs= Content-Type: application/json
Host: somehostname
Content-Length: 41
{
    "bookingNumber": "B6505222000395"
}
```

ทีนี้ เรื่องของเรื่องคือ แล้วถ้าต้องการเอาไปใช้บน Postman ล่ะ ทำยังไง ต้องมานั่งคำนวณหา Digital Signature ใหม่ก่อนส่งทุกครั้งเหรอ เสียเวลาน่าดู

จริง ๆ แล้วบน Postman เองก็มีฟีเจอร์ Pre-request Script เพื่อให้เราสามารถปรับแต่ง Request ของเราก่อนส่งได้อยู่แล้ว ดังนั้น เราไม่จำเป็นต้องมานั่งทำทุกครั้งให้เสียเวลา ที่สำคัญ Postman เองก็มีการ import เอา Library ที่สามารถทำเรื่องนี้มารอไว้อยู่แล้ว นั่นคือ CryptoJS ดังนั้นที่เหลือก็ไม่ยากแล้ว

เขียนตามนี้ลงไปใน Pre-request Script ได้เลย

สมมติว่าเรามีการประกาศ Secret ของ App เอาไว้ใน environment ชื่อ appSecret และ App name ที่จะทำหน้าที่เป็น Request Sender ในชื่อ appName

```js
const appName = pm.environment.get(“appName”);
const appSecret = pm.environment.get(“appSecret”);
var signBytes = CryptoJS.HmacSHA256(pm.request.body.raw, appSecret);
var signBase64 = CryptoJS.enc.Base64.stringify(signBytes);
pm.request.headers.add(appName, “X-RequestSender”);
pm.request.headers.add(signBase64, “X-RequestSignature”);
```

**บรรทัด 1–2** จะเป็นการอ่าน *appName* และ *appSecret* จาก environment ที่เราประกาศไว้แล้ว
**บรรทัดที่ 3** ก็จะทำการสร้าง signature โดยการ sign ตัว request body ของเราด้วย *appSecret* ที่อ่านมา โดยใช้ HMAC SHA256
**บรรทัดที่ 4** ทำการ encode signature ให้อยู่ในรูป base64
**บรรทัดที่ 5–6** เอาค่าที่ได้ยัดเข้าไปให้กับ header ที่กำลังจะส่ง

เท่านี้เอง