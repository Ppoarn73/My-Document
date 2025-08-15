---
title: The Twelve-Factor App สำหรับ Microservices
description: หลักการออกแบบและพัฒนาแอปพลิเคชันแบบ Microservices ตาม Twelve-Factor App
layout: default
---

# 📚 The Twelve-Factor App สำหรับ Microservices

Twelve-Factor App เป็นหลักการออกแบบและพัฒนาแอปพลิเคชันที่ช่วยให้ระบบมีความยืดหยุ่น ขยายได้ง่าย และพร้อมทำงานบน Cloud หรือรูปแบบ Microservices

## สารบัญ
1. [Codebase](#1-codebase)
2. [Dependencies](#2-dependencies)
3. [Config](#3-config)
4. [Backing Services](#4-backing-services)
5. [Build, Release, Run](#5-build-release-run)
6. [Processes](#6-processes)
7. [Port Binding](#7-port-binding)
8. [Concurrency](#8-concurrency)
9. [Disposability](#9-disposability)
10. [Dev/Prod Parity](#10-devprod-parity)
11. [Logs](#11-logs)
12. [Admin Processes](#12-admin-processes)
13. [Reference](#13-reference)

---

## 1. Codebase

แอปพลิเคชันควรมีเพียงหนึ่ง codebase ที่ถูกติดตามผ่านระบบ version control เช่น Git และสามารถ deploy ไปยังหลายๆ environment ได้

### แนวทางปฏิบัติ
- ใช้ Git repository เดียวสำหรับทุก environment
- ใช้ branch เพื่อแยกการพัฒนาในแต่ละ environment
- ทำการ merge code อย่างสม่ำเสมอเพื่อป้องกันความแตกต่างระหว่าง environment

### ประโยชน์
- ลดความซับซ้อนในการจัดการ codebase
- ง่ายต่อการติดตามและควบคุมการเปลี่ยนแปลง

---

## 2. Dependencies

ควรระบุ dependencies ทั้งหมดอย่างชัดเจนและแยกออกจาก codebase

### แนวทางปฏิบัติ
- ใช้ dependency manager เช่น npm, pip, Maven เพื่อจัดการ dependencies
- ระบุเวอร์ชันของแต่ละ dependency อย่างชัดเจน
- ใช้ containerization เช่น Docker เพื่อแยกสภาพแวดล้อมของแต่ละ service

### ประโยชน์
- ลดความเสี่ยงจากการเปลี่ยนแปลงของ dependencies
- ทำให้การพัฒนาและการ deploy เป็นไปอย่างราบรื่น

---

## 3. Config

ควรเก็บ configuration ไว้ใน environment variables แทนการ hard-code ไว้ใน source code

### แนวทางปฏิบัติ
- ใช้ environment variables เพื่อเก็บค่าคอนฟิกที่แตกต่างกันระหว่าง environment
- ใช้ tools เช่น dotenv เพื่อโหลดค่าคอนฟิกในระหว่างการพัฒนา

### ประโยชน์
- ง่ายต่อการปรับเปลี่ยน configuration โดยไม่ต้องแก้ไข source code
- ป้องกันการเปิดเผยข้อมูลสำคัญใน source code

---

## 4. Backing Services

ควรถือว่า backing services เช่น database, cache, queue เป็น resources ที่สามารถเชื่อมต่อได้

### แนวทางปฏิบัติ
- ใช้ connection string หรือ URL เพื่อเชื่อมต่อกับ backing services
- ปรับปรุงการเชื่อมต่อกับ backing services โดยไม่ต้องแก้ code

### ประโยชน์
- เพิ่มความยืดหยุ่นในการเปลี่ยนแปลง backing services
- ง่ายต่อการทดสอบและพัฒนา

---

## 5. Build, Release, Run

ควรแยกขั้นตอนการ build, release และ run ออกจากกันอย่างชัดเจน

### แนวทางปฏิบัติ
- ใช้ CI/CD pipeline เพื่อแยกขั้นตอนการ build, release และ run
- ทำการทดสอบในแต่ละขั้นตอนอย่างสม่ำเสมอ

### ประโยชน์
- ลดความซับซ้อนในการ deploy
- เพิ่มความมั่นใจในคุณภาพของแอปพลิเคชัน

---

## 6. Processes

แอปพลิเคชันควรทำงานในรูปแบบของ stateless processes

### แนวทางปฏิบัติ
- เก็บข้อมูลที่จำเป็นใน backing services แทนการเก็บใน memory ของ process
- ทำให้แต่ละ process สามารถทำงานได้อย่างอิสระ

### ประโยชน์
- ง่ายต่อการ scale แอปพลิเคชัน
- ลดความซับซ้อนในการจัดการ state

---

## 7. Port Binding

ควร export services ผ่านการ binding กับ port

### แนวทางปฏิบัติ
- ใช้ HTTP server หรือ framework ที่สามารถ binding กับ port ได้
- ทำให้แอปพลิเคชันสามารถรับ request ผ่าน port ที่กำหนด

### ประโยชน์
- ง่ายต่อการเชื่อมต่อกับระบบอื่นๆ
- เพิ่มความยืดหยุ่นในการ deploy

---

## 8. Concurrency

ควรใช้ process model ในการ scale แอปพลิเคชัน

### แนวทางปฏิบัติ
- ใช้ process pool หรือ worker pool เพื่อจัดการกับงานที่ต้องการประมวลผล
- ปรับจำนวน process หรือ worker ตามความต้องการ

### ประโยชน์
- เพิ่มประสิทธิภาพในการประมวลผล
- ง่ายต่อการ scale แอปพลิเคชัน

---

## 9. Disposability

ควรออกแบบแอปพลิเคชันให้สามารถเริ่มต้นและหยุดทำงานได้อย่างรวดเร็ว

### แนวทางปฏิบัติ
- ออกแบบ process ให้สามารถ start และ stop ได้อย่างรวดเร็ว
- จัดการกับ signal และ error อย่างเหมาะสม

### ประโยชน์
- ง่ายต่อการจัดการและ monitor
- ลด downtime ในระหว่างการ deploy

---

## 10. Dev/Prod Parity

ควรทำให้ development, staging และ production environment มีความคล้ายคลึงกันมากที่สุด

### แนวทางปฏิบัติ
- ใช้ containerization เช่น Docker เพื่อจำลองสภาพแวดล้อมที่เหมือนกัน
- ใช้ CI/CD pipeline เพื่อทดสอบและ deploy อย่างสม่ำเสมอ

### ประโยชน์
- ลดความเสี่ยงจากปัญหาที่เกิดขึ้นเฉพาะในบาง environment
- ง่ายต่อการทดสอบและพัฒนา

---

## 11. Logs

ควรถือว่า logs เป็น event streams และไม่ควรพึ่งพา log files

### แนวทางปฏิบัติ
- เขียน logs ออกไปยัง stdout และ stderr
- ใช้ tools เช่น ELK stack หรือ Prometheus เพื่อจัดการกับ logs

### ประโยชน์
- ง่ายต่อการ monitor และวิเคราะห์
- ลดความซับซ้อนในการจัดการ logs

---

## 12. Admin Processes

ควรจัดการกับงาน admin เช่น database migration หรือ one-off tasks เป็น process แยกต่างหาก

### แนวทางปฏิบัติ
- ใช้ command-line tools หรือ scripts เพื่อจัดการกับงาน admin
- ทำให้สามารถรันงาน admin ได้อย่างอิสระจาก application

### ประโยชน์
- ง่ายต่อการจัดการและทดสอบ
- ลดความซับซ้อนในการ deploy

---

## 13. Reference
1. Adam Wiggins, *The Twelve-Factor App*, [https://12factor.net/](https://12factor.net/)
2. Sakul Montha, *The Twelve-Factor App ในการทำ Microservices*, [Medium](https://iamgique.medium.com/the-twelve-factor-app-%E0%B9%83%E0%B8%99%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%97%E0%B8%B3-microservice-cfcd70fa106a)