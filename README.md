# Denmark Flag - Assembly (EMU8086)

## 📌 คำอธิบายโปรเจค
โปรเจคนี้เป็นการเขียนโปรแกรมภาษา Assembly เพื่อวาด "ธงชาติเดนมาร์ก" โดยใช้โปรแกรม EMU8086  
แสดงผลในโหมดกราฟิก (Mode 13h) ขนาด 320x200 พิกเซล

---

## ⚙️ หลักการทำงาน
- ใช้ interrupt `int 10h` เพื่อเข้าสู่โหมดกราฟิก
- เขียนข้อมูลลง Video Memory (A000h) โดยตรง
- ใช้คำสั่ง `rep stosw` เพื่อเพิ่มความเร็วในการวาดภาพ
- วาดพื้นหลังสีแดง และวาดกากบาทสีขาวตามรูปแบบธงเดนมาร์ก

---

## 🚀 วิธีการรันโปรแกรม
1. เปิดโปรแกรม EMU8086  
2. เลือก COM template  
3. วางโค้ดลงในไฟล์  
4. กด Run ▶️  
5. โปรแกรมจะแสดงธงเดนมาร์กบนหน้าจอ  


## 🧾 Source Code

```asm
org 100h

mov ax, 13h
int 10h

mov ax, 0A000h
mov es, ax

mov di, 0
mov cx, 32000    
mov ax, 0404h    
rep stosw

mov di, 27200    
mov cx, 4800     
mov ax, 0F0Fh    
rep stosw

mov cx, 200      
mov di, 100      
mov ax, 0F0Fh    

draw_vertical:
    push cx
    mov cx, 15   
    rep stosw
    pop cx
    add di, 290  
    loop draw_vertical

mov ah, 00h
int 16h

mov ax, 03h
int 10h

mov ah, 4Ch
int 21h

---









## ✨ คุณสมบัติของโปรแกรม
- แสดงผลเร็ว (ไม่ใช้การวาด pixel ทีละจุด)  
- ใช้การเข้าถึงหน่วยความจำโดยตรง (Direct Memory Access)  
- โค้ดมีการแบ่งโครงสร้างชัดเจน  

---

## 👥 ผู้จัดทำ
- สมาชิกในกลุ่ม (ใส่ชื่อสมาชิกที่นี่)



## 📷 ตัวอย่างผลลัพธ์
<img width="1919" height="1028" alt="สกรีนช็อต 2026-03-29 215838" src="https://github.com/user-attachments/assets/796ce5ab-a3cb-4006-b092-73394664c317" />

