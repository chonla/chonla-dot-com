---
title: chonla.com
date: 2022-07-01T09:20:34+07:00
draft: false
summary: เรื่องราวเกี่ยวกับการสร้าง chonla.com
cover_image: all-new-chonla-dot-com-cover.png
slug: all-new-chonla-dot-com
---

แรกเริ่มเลย chonla.com เคยใช้เป็น self-hosted wordpress ที่มีทุกอย่างเป็น WYSIWYG (What you see is what you get) ทำให้การสร้าง blog ทำได้ง่ายมาก แค่เปิด editor แล้วก็พิมพ์ แล้วก็ publish ก็เป็นอันจบ

พอเริ่มมีการย้ายเนื้อหาบางส่วนไปลอง publish บน medium ก็พบว่า ตัว WYSIWYG editor ของ medium มันไม่เหมือน wordpress โดยฟีเจอร์ของ medium มีให้น้อยกว่า wordpress อยูู่พอสมควร ทำให้ตอนปรับหน้าตาเนื้อหาให้มันไปใช้ได้บน medium โดยให้เพี้ยนจาก wordpress น้อยที่สุดนี่ไม่ง่ายเลย ยิ่งถ้าของเดิมใส่อะไรที่มันไม่มีใน medium นี่คือต้องมาคิดเรื่องโครงสร้างเนื้อหากันใหม่ อันนี้จะเป็นหัวข้อเลเวลไหนดีนะ มันตีกับหัวข้อที่แล้วไหมนะ ตารางนี่จะใส่ยังไงดี เปลี่ยนสีตัวอักษรไม่ได้ จะทำไงดี

เลยกลับมานั่งนึก ๆ ดูอีกที ก็พบว่าถ้าเราเริ่มต้นแบบ less is more น่าจะดี คือ ใช้ให้น้อยตั้งแต่แรกเลย และทำยังไงก็ได้ให้พร้อมที่จะไปต่อได้ถึงแม้เทคโนโลยีจะเปลี่ยน เลยนึกถึง markdown format ขึ้นมา ซึ่งก็คุ้นเคยอยู่แล้ว แล้วก็เลยเถิดไปถึงเรื่องการหา CMS ที่มันไม่ต้องต่อฐานข้อมูลได้ไหม เอาแบบวางไฟล์เฉย ๆ ก็ไปหาดู ก็เจออยู่หลายตัว เช่น Jekyll, Hugo แต่สุดท้ายก็มาลอง Hugo เพราะ Hugo เองใช้ภาษา Golang ในการพัฒนา และตัวตัว template ของหน้าจอที่ใช้ ใช้เป็น go template ก็พอคุ้นเคยอยู่ (ส่วน Jekyll เป็น ruby)

ท้ายที่สุดก็ได้ออกมาเป็น TO-DO List ประมาณนี้

### To-DO List

* ~~export เนื้อหาทั้งหมดบน wordpress ได้มาเป็น sql file~~ (เดี๋ยวค่อยหาวิธีแปลงเป็น markdown ทีหลัง)
* ~~ย้าย domain chonla.com ออกจาก hosting เจ้าเดิม~~
* ~~สร้าง site ใหม่ เป็น static ไว้ที่ netlify~~
* ~~สร้าง automated deployment ที่ github กับ netlify~~
* ~~redirect chonla.com ไปที่ static site ที่ netlify~~
* สร้าง theme ใหม่
* ทำ Tag support
* ย้าย content บน wordpress เดิม (sql file) มาใส่ hugo
* ทำ Search support
