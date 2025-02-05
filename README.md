# Complaint Text Classification in Traffy Fondue Using WangchanBERTa for Accurate Forwarding to Relevant Agencies (The model is currently being improved)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1izXxj1T65N7MWDrcYEWKyhc3RGfaab3J?usp=sharing)

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
<p align="center">
  <img src="https://github.com/user-attachments/assets/13b094fc-3f95-4890-835b-3acd26a46cf9">
</p>

# 2.  วิธีการดำเนินงาน

## 2.1 ชุดข้อมูล

ใช้ชุดข้อมูลจากแพลตฟอร์ม Traffy Fondue โดยในเว็บไซต์ มีให้ขอข้อมูลในระบบเพื่อไปเพื่อไปจัดทำสถิติต่างๆ

## 2.2 การสำรวจชุดข้อมูล

ข้อมูลชุดนี้มี 781,683 แถว มี 17 คุณลักษณะ มีหน่วยงานที่แตกต่างกันถึง 747 หน่วยงาน (รวมหน่วยงานที่มีการแบ่งเป็นเขตและพื้นที่)

<p align="center">
  <img width = 700 heigth =700 src="https://github.com/user-attachments/assets/3d4f51b5-d5af-4cd8-8667-e17d091d714f">
</p>

## 2.3 การเตรียมข้อมูล

มีการเลือกข้อมูลที่ไม่ผ่านการส่งต่อหรือไม่มีสถานะ “ส่งต่อ” เลย มีการ Grouping ในคลาส โดยการ Group หน่วยงานที่แบ่งเขตการทำงานให้เป็นชื่อเดียวกันตัวอย่างเช่น “เทศกิจเขตบางนา”ก็จะได้ “เทศกิจ” และมีการใช้ Library Pythainlp สำหรับการจัดการภาษาไทยอย่างเช่น ลบสัญลักษณ์ ช่องว่าง ตัวเลข อักขระเดี่ยว และ ตรวจคำผิด

<p align="center">
  <img width = 650 heigth =650 src="https://github.com/user-attachments/assets/3df24ab6-a52b-477f-ba88-4ae0add7994a">
</p>

## 2.4 การฝึกโมเดล

ในบทความนี้ใช้โมเดล WangchanBERTa สำหรับการ Text classification ซึ่งเป็นโมเดลภาษาไทย Pre-trained โดยชุดข้อมูลที่ใช้ในการเทรนโมเดลมีขนาด 78.5 GB ที่มีสถาปํตยกรรมมาจาก BERT 

การตั้งค่านี้ใช้ Fine-tuning โมเดล wangchanberta-base-att-spm-uncased  โดย:

- ฝึก 5 Epochs หรือ 500 Steps

- Batch Size = 2 (สะสม Gradient ทุก 16 Steps)

- Learning Rate = 3e-5 พร้อม Warmup 75 Steps

- บันทึก Log ทุก 5 Steps และประเมินผลทุก 50 Steps

- ใช้ F1-micro เป็นเกณฑ์เลือกโมเดลที่ดีที่สุด

# 3. ผลลัพธ์

| Category                 | Precision | Recall | F1-score | Support |
|--------------------------|-----------|--------|---------|---------|
| ฝ่ายการคลัง             | 0.83      | 1.00   | 0.91    | 5       |
| ฝ่ายการศึกษา           | 0.93      | 0.97   | 0.95    | 40      |
| ฝ่ายทะเบียน             | 0.97      | 0.93   | 0.95    | 40      |
| ฝ่ายปกครอง             | 0.90      | 0.68   | 0.77    | 40      |
| ฝ่ายพัฒนาชุมชน        | 0.80      | 0.93   | 0.86    | 40      |
| ฝ่ายรักษาความสะอาดฯ | 0.81      | 0.85   | 0.83    | 40      |
| ฝ่ายรายได้             | 0.97      | 0.95   | 0.96    | 40      |
| ฝ่ายสิ่งแวดล้อม       | 0.76      | 0.80   | 0.78    | 40      |
| ฝ่ายเทคนิค             | 0.66      | 0.68   | 0.67    | 40      |
| ฝ่ายโยธา               | 0.82      | 0.80   | 0.81    | 40      |



### **Summary**
| Metric | Score |
|----------------|------|
| **Accuracy** | 0.84 |
| **Macro Avg** | 0.85 / 0.86 / 0.85 |
| **Weighted Avg** | 0.85 / 0.84 / 0.84 |




Data : https://drive.google.com/drive/u/1/folders/1PqMQlDy_WQ-3xxrgBcySryAATYEibqfo

