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
---

## ✨ คุณสมบัติของโปรแกรม
- แสดงผลเร็ว (ไม่ใช้การวาด pixel ทีละจุด)  
- ใช้การเข้าถึงหน่วยความจำโดยตรง (Direct Memory Access)  
- โค้ดมีการแบ่งโครงสร้างชัดเจน  

---

## 👥 ผู้จัดทำ
- สมาชิกในกลุ่ม 

- นาย ยศพัทธ์ พิมพ์ศรี 116810906016-4
- นาย สายฟ้า ลาพงษ์ 116810906003-2
- นาย พลกฤต กองแก้ว 116810906008-1
- นางสาว ปภัสสรา ปักกาเวสา 116810906043-8
- นางสาว รัชฎา แดงนวล 116810906025-5



## 📷 ตัวอย่างผลลัพธ์

<img width="1919" height="1079" alt="kuwait" src="https://github.com/user-attachments/assets/21469766-c23a-4c88-9aa2-297cbf00de8e" />





## 🧾 Source Code

```assembly
org 100h

mov ax, 13h
int 10h
mov ax, 0A000h
mov es, ax

mov di, 0
mov cx, 10720
mov ax, 0202h
rep stosw

mov cx, 10720    
mov ax, 0F0Fh
rep stosw

mov cx, 10560
mov ax, 0404h
rep stosw

mov di, 0
mov cx, 67
mov bx, 0
mov bp, 0
top_trap:
    push di
    push cx
    mov cx, bx
    jcxz skip_t
    mov al, 0
    rep stosb
skip_t:
    pop cx
    pop di
    add di, 320
    add bp, 80
calc_t:
    cmp bp, 67
    jl  next_t
    inc bx
    sub bp, 67
    jmp calc_t
next_t:
    loop top_trap

mov cx, 67
mid_trap:
    push di
    push cx
    mov cx, 80
    mov al, 0
    rep stosb
    pop cx
    pop di
    add di, 320
    loop mid_trap

mov cx, 66
mov bx, 80
mov bp, 0
bot_trap:
    push di
    push cx
    mov cx, bx
    jcxz skip_b
    mov al, 0
    rep stosb
skip_b:
    pop cx
    pop di
    add di, 320
    add bp, 80
calc_b:
    cmp bp, 66
    jl  next_b
    dec bx
    sub bp, 66
    jmp calc_b
next_b:
    loop bot_trap

mov ah, 00h
int 16h

mov ax, 03h
int 10h
mov ah, 4Ch
int 21h
