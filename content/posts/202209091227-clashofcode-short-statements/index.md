---
title: โค้ด python แบบสั้น ๆ
date: 2022-09-09T12:27:00+07:00
draft: false
summary: เอาไว้เล่น codingame clash of code แบบ shortest mode
cover_image: clashofcode-short-statements-cover.png
slug: clashofcode-short-statements
tags:
    - clashofcode
    - codingame
---

เข้าไปเล่น Clash of code บน CodingGame แล้วเจอ clash ที่เป็นเขียน code ให้สั้นที่สุดเท่าที่จะเป็นไปได้หลายครั้ง เลยรวบรวมไว้เสียหน่อย

# Python3

## รับ input เข้ามาเป็น array แล้วแปลงทั้งหมดเป็นตัวเลข

```python
b=map(int,open(0))
```

## รับ input เข้ามา n ตัว แล้วรับ input n ตัวที่เหลือเป็นตัวเลข

```python
a,*b=map(int,open(0))
```