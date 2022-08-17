---
title: เอารูปจากเว็บไปเก็บไว้ใน S3 ด้วย Python
date: 2022-03-03T15:04:00+07:00
draft: false
summary: เอารูปจากเว็บมา transform แล้วเอาไปเก็บไว้ใน Amazon S3 ด้วย Python
cover_image: migrate-images-to-amazon-s3-with-python-cover.png
slug: migrate-images-to-amazon-s3-with-python
---

ได้โจทย์ที่ต้องเอารูปจากเว็บไซต์ของลูกค้าไปเก็บไว้ใน S3 แล้วต้องทำด้วย python เพราะใช้ dagster โดยค่าที่ได้มาจะเป็น URL ของรูปเท่านั้น แถมอีกนิดคือ จะต้อง resize รูปให้เล็กลงด้วย เพราะรูปที่ได้มามันใหญ่มาก ไม่ได้อยากได้ใหญ่ขนาดนั้น เรามาลองดูวิธีกัน

### package ที่ต้องใช้

* requests สำหรับใช้ในการดึงรูปจาก URL
* boto3 สำหรับเอาของขึ้นไปวางไว้ใน S3
* Pillow สำหรับทำ Image processing

เราสามารถ install ผ่าน pip ได้เลย

```shell
pip3 install requests boto3 Pillow
```

### ขั้นตอนดึงรูปจาก URL

```python
response = request.get(image_url)
```

มีเท่านี้แหละ ง่าย ๆ ไม่ได้มีอะไรซับซ้อนครับ เราจะได้ response กลับมา ซึ่งสามารถเอาค่าที่เป็น binary กลับมาใช้ได้ผ่าน `response.content`

### ขั้นตอนการย่อรูปให้เล็กลง

ในขั้นนี้ เราจะไม่เสียเวลา save response ที่ได้จากขั้นตอนก่อนหน้าลงไฟล์ แต่เราจะจับยัดลงใน  `BytesIO`  แทน ซึ่งทำหน้าที่แทน File ได้เลย ในที่นี้เราเรียกว่า `File-like object`

```python
image = Image.open(io.BytesIO(response.content))
```

เราได้ image ที่เป็นภาพแล้ว ต่อไปก็ย่อมันลงไปที่ขนาดที่เราต้องการ สมมติว่าอยากย่อไปที่ขนาด 1024 x 768

```python
resized_image = image.resize((1024, 768))
```

พิมพ์ไม่ผิดครับ ใช้วงเล็บ 2 ชั้น เพราะ  `(1024, 768)`  หมายถึง  `Tuple`  ครับ

เสร็จแล้ว เรายัดกลับใส่ไฟล์อีกที เพื่อเตรียมเอาขึ้น S3 ครับ

```python
resized_image_file = io.BytesIO()
resized_image.save(resized_image_file, format="PNG")
resized_image_file.seek(0)
```

เราใช้  `io.BytesIO`  อีกแล้ว อย่าเสียเวลา save ไฟล์จริง ๆ จุดที่สำคัญอีกจุดคือคำสั่ง  `seek(0)`  เพราะหลังจากที่เราสั่ง  `.save()`  ในบรรทัดก่อนหน้า ไฟล์ cursor จะไปอยู่ท้ายสุดของไฟล์ ถ้าเราอัพโหลดต่อจากตรงนี้เลยโดยไม่ได้  `seek(0)`  **มันจะอัพโหลดได้ 0 Bytes เสมอ** ผมโดนมาแล้ว ไม่ได้ใส่  `seek(0)`  นั่งหาสาเหตุอยู่นาน (หมายเหตุ : การใส่  `seek(0)`  จะเป็นการย้ายไฟล์ cursor ไปที่ต้นไฟล์ **เพื่อทำการเขียนหรืออ่านต่อไป**)

### อัพโหลดขึ้น S3 กันเลย

ถึงตรงนี้ เราต้องไปเปิดใช้ S3 และทำการสร้าง Bucket ที่จะเอาไฟล์ไปวางแล้ว หลังจากที่เราทำแล้ว ตรงนี้เราจะต้องมีรายละเอียดที่สำคัญในมือแล้ว นั่นคือ

* ชื่อ region ของ Bucket ที่เราสร้าง
* AWS Access Key ID
* AWS Secret Access Key
* Bucket

เสร็จแล้วก็ตามนี้เลย

```python
s3_client = boto3.client(
    "s3",
    region_name="ชื่อ region เช่น ap-southeast-1",
    aws_access_key_id="AWS Access Key ID",
    aws_secret_access_key="AWS Secret Access Key"
)
try:
    with resized_image_file as data:
        s3_client.upload_fileobj(
            data,
            Bucket="ชื่อ Bucket",
            Key="ชื่อไฟล์ปลายทางที่จะไปวาง ถ้ามี directory ย่อยก็ใส่ด้วยเลย"
        )
except ClientError as e:
    print(e)
```

ที่เหลือก็อย่าลืมไปกำหนดค่า Permission ที่จะให้เข้าถึงไฟล์ให้ตรงกับที่ต้องใช้งาน เป็นอันจบวิธีการ