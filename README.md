# Complaint Text Classification in Traffy Fondue Using WangchanBERTa for Accurate Forwarding to Relevant Agencies (The model is currently being improved)


<p align="center">
  <img width = 200 heigth =200 src="https://storage.googleapis.com/traffy_public_bucket/traffy_logo/Image_fondue_logo.png">
</p>

# เกี่ยวกับ Traffy Fondue

Traffy Fondue เป็น 1 ในวิธีการแก้ปัญหาเส้นเลือดฝอย ของนายชัชชาติ สิทธิพันธุ์ (ผู้ว่าฯ กทม.) โดยให้ประชาชนเสนอเรื่องร้องหรือเสนอแนะเข้ามาในระบบเพื่อให้หน่วยงานที่เกี่ยวข้องดำเนินการแก้ไข


# 1.  บทนำ

โปรเจ็คนี้มีวัตถุประสงค์เพื่อจำแนกข้อความร้องเรียนในแพลตฟอร์มทราฟฟี่ฟองดูว์เพื่อตามหน่วยงานที่เกี่ยวข้อง 

ตัวอย่าง

```
input_text = "มีคนขี่รถมอเตอร์ไซต์บนฟุตบาต"
```

ผลลัพธ์
```
ฝ่ายเทศกิจ
```
![download (14)](https://github.com/user-attachments/assets/3df24ab6-a52b-477f-ba88-4ae0add7994a)

# 2.  วิธีการดำเนินงาน

## 2.1 ชุดข้อมูล

ใช้ชุดข้อมูลจากแพลตฟอร์ม Traffy Fondue โดยในเว็บไซต์ มีให้ขอข้อมูลในระบบเพื่อไปเพื่อไปจัดทำสถิติต่างๆ

## 2.2 การสำรวจชุดข้อมูล

ข้อมูลชุดนี้มี 781,683 แถว มี 17 คุณลักษณะ มีหน่วยงานที่แตกต่างกันถึง 747 หน่วยงาน (รวมหน่วยงานที่มีการแบ่งเป็นเขตและพื้นที่)

## 2.3 การเตรียมข้อมูล

มีการเลือกข้อมูลที่ไม่ผ่านการส่งต่อหรือไม่มีสถานะ “ส่งต่อ” เลย มีการ Grouping ในคลาส โดยการ Group หน่วยงานที่แบ่งเขตการทำงานให้เป็นชื่อเดียวกันตัวอย่างเช่น “เทศกิจเขตบางนา”ก็จะได้ “เทศกิจ” และมีการใช้ Library Pythainlp สำหรับการจัดการภาษาไทยอย่างเช่น ลบสัญลักษณ์ ช่องว่าง ตัวเลข อักขระเดี่ยว และ ตรวจคำผิด

## 2.4 การฝึกโมเดล

ในบทความนี้ใช้โมเดล WangchanBERTa สำหรับการ Text classification ซึ่งเป็นโมเดลภาษาไทย Pre-trained โดยชุดข้อมูลที่ใช้ในการเทรนโมเดลมีขนาด 78.5 GB ที่มีสถาปํตยกรรมมาจาก BERT 

การตั้งค่านี้ใช้ Fine-tuning โมเดล wangchanberta-base-att-spm-uncased  โดย:

- ฝึก 5 Epochs หรือ 500 Steps

- Batch Size = 2 (สะสม Gradient ทุก 16 Steps)

- Learning Rate = 3e-5 พร้อม Warmup 75 Steps

- บันทึก Log ทุก 5 Steps และประเมินผลทุก 50 Steps

- ใช้ F1-micro เป็นเกณฑ์เลือกโมเดลที่ดีที่สุด

# 3. ผลลัพธ์




Data : https://drive.google.com/drive/u/1/folders/1PqMQlDy_WQ-3xxrgBcySryAATYEibqfo

colab : [https://colab.research.google.com/drive/13ql881pE2NKnVeBlrS9U6KHqUe_OLcDE?usp=sharing](https://colab.research.google.com/drive/1izXxj1T65N7MWDrcYEWKyhc3RGfaab3J?usp=sharing)
