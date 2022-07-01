---
title: "ย้ายบ้าน"
date: 2022-07-01T09:20:34+07:00
draft: true
---

ก่อนหน้านี้ chonla.com เคยใช้เป็น self-hosted wordpress ซึ่งจริง ๆ แล้วมันง่ายนะ ทุกอย่างก็ทำผ่านเว็บทั้งหมด จัดหน้าอะไรก็ง่าย เพราะทุกอย่างเป็น WYSIWYG (What you see is what you get) editor

แต่พอได้ลองเอา content บางส่วนไปวางไว้บน medium ก็พบว่าตอนปรับหน้าตา content ให้มันไปใช้ได้บน medium โดยให้เพี้ยนจาก wordpress น้อยที่สุดนี่ไม่ง่ายเลย ถึงแม้ medium จะใช้ WYSIWYG editor เหมือนกัน แต่ฟีเจอร์มันมีน้อยกว่ามาก ทำให้ layout บางอย่างมันทำบน medium ไม่ได้

เลยกลับมานั่งนึก ๆ ดูอีกที ก็พบว่าถ้าเราเริ่มต้นแบบ less is more น่าจะดี คือ ใช้ให้น้อยตั้งแต่แรกเลย และทำยังไงก็ได้ให้พร้อมที่จะ move on ถึงแม้เทคโนโลยีจะเปลี่ยน เลยนึกถึง markdown ขึ้นมา หลังจากนั้นก็ได้แต่คิด ยังไม่ได้มีเวลามาสร้างไซต์ใหม่เลย แล้วก็ hosting เก่าก็หมดอายุ เลยเก็บไว้แต่ domain เฉย ๆ และ export เนื้อหาเดิมมาเก็บไว้ จนเวลาล่วงเลยไปหลายปี ...

## Requirements

หลัก ๆ มีเป้าหมายที่มองไว้คือ ทำยังไงที่จะทำให้ไม่ต้องหา editor ให้วุ่นวาย หรือหา editor ตัวไหนดีที่จะทำงานร่วมกันได้ง่าย ก็มองไว้เป็น Markdown file ซึ่งจะช่วยลดปัญหาการใช้ database ลงไปได้ด้วย

ก็เลยมอง CMS ที่เป็น Markdown เลย ก็เจอตัวที่น่าสนใจหลายตัว ตัวที่หยิบมาเล่นก็จะเป็น Jekyll กับ Hugo

ลองเล่น Jekyll แล้ว เป็น tool ที่รันบน ruby ก็เจอปัญหาการสร้าง site ใหม่ งม ๆ อยู่แป๊บนึงเลยลองตัดสินใจไปเล่น Hugo ก่อนดีกว่า

## Hugo

Hugo เป็น tool ที่เขียนด้วย golang และเคลมว่าโคตรเร็ว เท่าที่ลอง เออ มันเร็วจริง และใช้ไม่ยากอะไร เอาเป็นว่า ตัดสินใจเร็ว ๆ เลยละกัน เอา Hugo นี่แหละ อย่าคิดนาน First impression matters จริง ๆ

## Netlify

ใช้ง่ายดี มี action ใน github ให้ใช้ด้วย ตามนั้น