# TECHNICAL_SPEC.md

# Givie — Technical Specification

> สถานะเอกสาร: Approved for initial implementation
> ภาษา: ไทย
> โปรเจกต์: Givie — Giveaway Tracker

---

## 1. วัตถุประสงค์

เอกสารนี้กำหนดแนวทางด้านเทคนิคสำหรับการพัฒนา Givie เว็บแอปพลิเคชันสำหรับจัดเก็บและติดตาม Giveaway จากโพสต์บน Social Media โดยเน้นการใช้งานส่วนตัว การเข้าถึงได้จากหลายอุปกรณ์ และการจัดเก็บข้อมูลบนระบบออนไลน์

เป้าหมายของระบบ:

- ใช้งานได้ทั้ง PC, Tablet และ Mobile
- Sync ข้อมูลระหว่างอุปกรณ์ผ่านฐานข้อมูลออนไลน์
- จัดเก็บข้อมูลอย่างเป็นระบบและลดความเสี่ยงจากข้อมูลสูญหาย
- รองรับการเพิ่มและจัดการ Giveaway หลายรายการ
- โหลดหน้าเว็บและรูปภาพได้รวดเร็ว
- ใช้บริการ Free Tier เป็นหลักเพื่อลดค่าใช้จ่าย
- ดูแลรักษาและพัฒนาต่อได้ง่าย เหมาะกับการพัฒนาแบบ Vibe Coding

## 2. ขอบเขตทางเทคนิค

ระบบประกอบด้วยส่วนหลักดังต่อไปนี้

1. **Frontend** — ส่วนติดต่อผู้ใช้และการแสดงผล
2. **Backend / Database** — การประมวลผลและจัดเก็บข้อมูล
3. **Authentication** — การยืนยันตัวตนและควบคุมการเข้าถึง
4. **Image Storage** — การจัดเก็บรูปภาพ Giveaway
5. **External Data Fetching** — การพยายามดึงข้อมูลและรูปภาพจาก URL ต้นทาง
6. **Deployment** — การเผยแพร่เว็บแอปพลิเคชัน
7. **Data Recovery** — แนวทางป้องกันและรับมือกับข้อมูลสูญหาย

Givie ออกแบบสำหรับผู้ใช้คนเดียวเป็นหลัก ไม่จำเป็นต้องรองรับสมาชิกหลายคนหรือการทำงานร่วมกันในระยะแรก

## 3. Tech Stack

กำหนดเทคโนโลยีหลักสำหรับการพัฒนา Givie ดังนี้

| ส่วนประกอบ | เทคโนโลยี | หน้าที่ |
|---|---|---|
| Frontend | React + TypeScript | พัฒนา UI และจัดการการทำงานของเว็บ |
| Build Tool | Vite | Build และรันโปรเจกต์ระหว่างพัฒนา |
| Styling | Tailwind CSS | จัดการ UI และ Responsive Design |
| Backend / Database | Supabase | ให้บริการ Backend และจัดเก็บข้อมูล |
| Database | PostgreSQL | จัดเก็บข้อมูลและความสัมพันธ์ระหว่างข้อมูล |
| Authentication | Supabase Auth | ยืนยันตัวตนผู้ใช้ |
| Image Storage | Supabase Storage | จัดเก็บรูปภาพ Giveaway |
| Hosting | Vercel | Deploy เว็บ Frontend |
| Web App | PWA (พิจารณาในระยะพัฒนา) | รองรับการเพิ่มเว็บลงในหน้าจออุปกรณ์ |

เลือกใช้ Supabase และ Vercel โดยเน้น Free Tier เป็นหลัก และยอมรับข้อจำกัดด้านทรัพยากรและการใช้งานของบริการฟรี

### 3.1 เหตุผลในการเลือกเทคโนโลยี

**React + TypeScript + Vite**
- เหมาะกับเว็บแอปพลิเคชันที่มีหลายหน้าและหลายสถานะ
- TypeScript ช่วยลดข้อผิดพลาดจากการกำหนดและส่งต่อข้อมูล
- Vite ช่วยให้การพัฒนาและ Build ทำได้รวดเร็ว

**Tailwind CSS**
- ช่วยจัดการรูปแบบ UI ได้สะดวก
- รองรับ Responsive Design
- ปรับแต่งสีและองค์ประกอบให้เข้ากับ Design Direction ของ Givie ได้

**Supabase**
- มี PostgreSQL สำหรับข้อมูลที่มีความสัมพันธ์กัน
- มี Authentication สำหรับระบบ Login
- มี Storage สำหรับรูปภาพ
- ลดความจำเป็นในการสร้าง Backend และดูแล Infrastructure เองทั้งหมด

**Vercel**
- รองรับการ Deploy เว็บ Frontend
- เชื่อมต่อกับ GitHub ได้
- เหมาะกับเว็บแอปพลิเคชันขนาดเล็กที่ต้องการเริ่มต้นด้วย Free Tier

### 3.2 ข้อจำกัดของ Free Tier

บริการฟรีอาจมีข้อจำกัด เช่น พื้นที่จัดเก็บ Database และ Storage, จำนวน Request, ปริมาณการใช้งาน, การหยุดพักโปรเจกต์เมื่อไม่มีการใช้งาน และความสามารถในการสำรองข้อมูล

ควรตรวจสอบเงื่อนไขและปริมาณการใช้งานเป็นระยะ หากเกินขีดจำกัดให้ประเมินทางเลือกและค่าใช้จ่ายก่อนเปลี่ยนเป็นบริการแบบชำระเงิน

## 4. System Architecture

### 4.1 ภาพรวมการทำงาน

```text
              User
               |
               v
        Web Browser / PWA
               |
               v
       React + TypeScript
          /          \
         v            v
      Vercel       Supabase
                   /  |  \
                  v   v   v
             Database Auth Storage
```

ภาพรวมการทำงาน:

- ผู้ใช้เปิด Givie ผ่าน Web Browser
- Frontend ที่ Deploy บน Vercel แสดงผลและจัดการ UI
- ผู้ใช้ Login ผ่าน Supabase Auth
- Frontend ติดต่อ Supabase เพื่ออ่านและจัดการข้อมูลตามสิทธิ์ที่ได้รับ
- ข้อมูลหลักจัดเก็บใน PostgreSQL
- รูปภาพจัดเก็บใน Supabase Storage
- Database เก็บตำแหน่งไฟล์รูปภาพเพื่อเชื่อมโยงกับ Giveaway

### 4.2 หลักการออกแบบ

- แยกข้อมูลหลักออกจากไฟล์รูปภาพ
- ไม่เก็บข้อมูลสำคัญไว้เฉพาะใน Browser
- ใช้ Database เป็นแหล่งข้อมูลหลัก (Single Source of Truth)
- ใช้ Authentication และ Row Level Security (RLS) เพื่อจำกัดการเข้าถึง
- ไม่เปิดเผย Secret Key หรือ Service Role Key ใน Frontend
- แยกส่วนการแสดงผลออกจากส่วนจัดการข้อมูล เพื่อให้ง่ายต่อการแก้ไข

## 5. Database Design

ฐานข้อมูลออกแบบให้รองรับความสัมพันธ์ของข้อมูลและการเพิ่มฟีเจอร์ในอนาคต รายละเอียดชนิดข้อมูลและ Constraint จริงอาจปรับได้ระหว่างการสร้าง Schema โดยต้องไม่ขัดกับกฎธุรกิจใน FEATURE_SPEC.md

### 5.1 ตารางหลัก

#### 5.1.1 giveaways

เก็บข้อมูล Giveaway แต่ละรายการ

| Field | Type (เบื้องต้น) | รายละเอียด |
|---|---|---|
| id | UUID | Primary Key |
| user_id | UUID | เจ้าของข้อมูล อ้างอิงบัญชีผู้ใช้ |
| source_url | TEXT | URL ต้นทางของโพสต์ |
| title | TEXT, Nullable | ชื่อรายการ หากไม่ระบุให้สร้างชื่อแสดงผลอัตโนมัติ |
| image_path | TEXT, Nullable | ตำแหน่งไฟล์รูปภาพใน Storage |
| event_id | UUID, Nullable | Event ที่เชื่อมโยง |
| lifecycle_state | TEXT | สถานะที่จัดเก็บจริง เช่น INBOX, ACTIVE หรือ COLLECTED |
| is_favorite | BOOLEAN | สถานะ Favorite |
| created_at | TIMESTAMPTZ | วันที่สร้าง |
| updated_at | TIMESTAMPTZ | วันที่แก้ไข |

หลักการ:
- `source_url` ใช้สำหรับตรวจสอบ URL ซ้ำ
- `title` ไม่จำเป็นต้องกรอก
- `event_id` เป็น NULL ได้ เช่น รายการใน Inbox หรือรายการที่เก็บไว้โดยไม่ระบุ Event
- `lifecycle_state` ใช้เก็บสถานะที่ผู้ใช้หรือระบบต้องบันทึกจริง ส่วน TO_COLLECT และ EXPIRED คำนวณจาก Event ผ่าน Database View
- การเปลี่ยนสถานะต้องไม่ทำให้ข้อมูลหลักของ Giveaway สูญหาย

#### 5.1.2 events

เก็บข้อมูล Event

| Field | Type (เบื้องต้น) | รายละเอียด |
|---|---|---|
| id | UUID | Primary Key |
| user_id | UUID | เจ้าของข้อมูล |
| name | TEXT | ชื่อ Event |
| location | TEXT, Nullable | สถานที่ |
| start_date | DATE | วันเริ่มต้น |
| end_date | DATE | วันสิ้นสุด |
| created_at | TIMESTAMPTZ | วันที่สร้าง |
| updated_at | TIMESTAMPTZ | วันที่แก้ไข |

ความสัมพันธ์:
- Event หนึ่งรายการมี Giveaway ได้หลายรายการ
- Giveaway หนึ่งรายการเชื่อมโยงกับ Event ได้ไม่เกินหนึ่งรายการ
- เมื่อไม่มี Event ให้ใช้ค่า `event_id = NULL`

#### 5.1.3 categories

เก็บรายการ Category ที่ผู้ใช้สร้างขึ้น

| Field | Type (เบื้องต้น) | รายละเอียด |
|---|---|---|
| id | UUID | Primary Key |
| user_id | UUID | เจ้าของข้อมูล |
| name | TEXT | ชื่อ Category |
| created_at | TIMESTAMPTZ | วันที่สร้าง |

#### 5.1.4 people_tags

เก็บชื่อบุคคลหรือชื่อคู่ชิปที่ผู้ใช้สร้างขึ้น

| Field | Type (เบื้องต้น) | รายละเอียด |
|---|---|---|
| id | UUID | Primary Key |
| user_id | UUID | เจ้าของข้อมูล |
| name | TEXT | ชื่อ Tag |
| created_at | TIMESTAMPTZ | วันที่สร้าง |

People Tags เป็น Tag อิสระ ไม่สร้างความสัมพันธ์ระหว่างชื่อบุคคลกับชื่อคู่ชิปโดยอัตโนมัติ

#### 5.1.5 giveaway_categories

ตารางเชื่อมความสัมพันธ์ระหว่าง Giveaway และ Category

| Field | Type (เบื้องต้น) | รายละเอียด |
|---|---|---|
| giveaway_id | UUID | Foreign Key |
| category_id | UUID | Foreign Key |

กำหนด Composite Primary Key จาก `giveaway_id` และ `category_id`

#### 5.1.6 giveaway_people

ตารางเชื่อมความสัมพันธ์ระหว่าง Giveaway และ People Tags

| Field | Type (เบื้องต้น) | รายละเอียด |
|---|---|---|
| giveaway_id | UUID | Foreign Key |
| people_tag_id | UUID | Foreign Key |

กำหนด Composite Primary Key จาก `giveaway_id` และ `people_tag_id`

ตารางเชื่อมต้องมี Foreign Key และต้องป้องกันการเชื่อมข้อมูลข้ามเจ้าของบัญชี

### 5.2 ความสัมพันธ์ของข้อมูล

```text
Events (1) -------- (Many) Giveaways
Giveaways (Many) -- (Many) Categories
Giveaways (Many) -- (Many) People Tags
```

Giveaway หนึ่งรายการสามารถมีหลาย Category และหลาย People Tags ได้ โดยไม่สร้าง Giveaway ซ้ำ

### 5.3 Data Integrity

- Primary Key ของแต่ละตารางต้องไม่ซ้ำกัน
- Foreign Key ต้องอ้างอิงข้อมูลที่มีอยู่จริง
- ป้องกัน Category หรือ People Tag ที่มีชื่อซ้ำโดยไม่ตั้งใจ
- การลบ Event ต้องจัดการ Giveaway ที่เชื่อมโยงอยู่ก่อนตามกฎใน FEATURE_SPEC.md
- การลบ Category หรือ People Tag ต้องไม่ทำให้ Giveaway สูญหาย
- การเปลี่ยนสถานะต้องไม่ทำให้ข้อมูลเดิมสูญหาย
- ความสัมพันธ์ระหว่างข้อมูลต้องไม่ข้ามเจ้าของบัญชี

## 6. Authentication & Security

### 6.1 Authentication

ใช้ Supabase Authentication สำหรับระบบ Login

Givie เป็นเว็บส่วนตัวสำหรับผู้ใช้คนเดียว จึงไม่เปิดระบบสมัครสมาชิกสาธารณะ

ข้อกำหนด:
- ต้อง Login ก่อนเข้าถึงข้อมูลภายในระบบ
- จำกัดบัญชีที่ได้รับอนุญาตให้ใช้งานเฉพาะเจ้าของระบบ
- สร้างหรือเชิญบัญชีเจ้าของผ่านวิธีที่ผู้ดูแลกำหนด ไม่เปิดให้บุคคลทั่วไปสมัครเอง
- รองรับ Logout และจัดการ Session
- เมื่อยังไม่ได้ Login ให้แสดงหน้า Login แทนข้อมูลภายในระบบ

### 6.2 Database Security

ใช้ Row Level Security (RLS) เพื่อควบคุมการเข้าถึงข้อมูล

- เปิดใช้งาน RLS ในตารางที่มีข้อมูลส่วนตัว
- อนุญาตให้เฉพาะบัญชีเจ้าของระบบอ่าน เพิ่ม แก้ไข และลบข้อมูลได้
- ใช้ `user_id` และตรวจสอบกับ `auth.uid()` ใน Policies
- ตารางเชื่อมต้องตรวจสอบสิทธิ์ของ Giveaway และข้อมูลอ้างอิงที่เกี่ยวข้องด้วย
- ป้องกันการเข้าถึงข้อมูลผ่าน API โดยไม่ผ่านสิทธิ์ที่กำหนด
- กำหนด Policy สำหรับ Storage ให้เฉพาะเจ้าของระบบเข้าถึงไฟล์ได้ตามความเหมาะสม

### 6.3 Secret Management

- ห้ามนำ Service Role Key ไปใส่ใน Frontend
- ใช้เฉพาะ Publishable Key หรือ Anon Key ที่ออกแบบมาให้ใช้ฝั่ง Client โดยต้องมี RLS ควบคุม
- เก็บ Secret ที่จำเป็นไว้ใน Environment Variables
- ไม่ Commit ไฟล์ `.env` หรือข้อมูลรับรองเข้าสู่ GitHub
- จัดเตรียม `.env.example` ที่ไม่มี Secret จริง

## 7. Image Storage & Processing

### 7.1 การจัดเก็บรูปภาพ

รูปภาพ Giveaway จัดเก็บใน Supabase Storage โดย Database เก็บเฉพาะตำแหน่งไฟล์หรือ Storage Path

รูปแบบไฟล์ที่รองรับ:
- PNG
- JPG / JPEG
- GIF

หนึ่ง Giveaway มีรูปภาพตัวแทนได้หนึ่งรูป

### 7.2 Image Processing

ก่อนจัดเก็บรูปภาพ ระบบควร:
1. ตรวจสอบชนิดไฟล์
2. ตรวจสอบขนาดไฟล์
3. ปรับขนาดภาพให้เหมาะกับการแสดงผล
4. บีบอัดภาพเพื่อลดพื้นที่จัดเก็บ
5. จัดเก็บไฟล์ใน Storage
6. บันทึก Storage Path ลง Database

ระบบไม่จำเป็นต้องเก็บรูปภาพความละเอียดสูง เพราะผู้ใช้สามารถเปิด URL ต้นฉบับเพื่อดูภาพเต็มได้

สำหรับ GIF ควรพยายามรักษาภาพเคลื่อนไหวไว้ หากเทคโนโลยีและพื้นที่จัดเก็บรองรับ โดยไม่แปลงเป็นภาพนิ่งโดยไม่จำเป็น

### 7.3 Image Fallback

ระบบพยายามดึงรูปภาพจาก URL ต้นทางก่อน หากไม่สำเร็จให้ผู้ใช้อัปโหลดรูปเองได้ หากไม่มีรูปภาพให้แสดง Placeholder ที่เข้ากับ Category หรือคาแรกเตอร์ของ Givie แทนข้อความ Error

### 7.4 File Management

- ใช้ชื่อไฟล์ที่ไม่ซ้ำกัน เช่น UUID
- ไม่ใช้ชื่อไฟล์จากผู้ใช้เป็นชื่อไฟล์จริงโดยตรง
- เมื่อลบหรือเปลี่ยนรูปภาพ ต้องจัดการไฟล์เก่าที่ไม่ใช้งานแล้ว
- การลบไฟล์ต้องไม่ทำให้ข้อมูล Giveaway สูญหาย
- ตรวจสอบชนิดไฟล์และขนาดไฟล์ทั้งฝั่ง Client และฝั่งที่รับไฟล์

## 8. External Data Fetching

### 8.1 การดึงข้อมูลจาก URL

ผู้ใช้เป็นผู้ Paste URL ของโพสต์ต้นทางด้วยตัวเอง ระบบพยายามดึงข้อมูลที่จำเป็น เช่น รูปภาพตัวแทนของโพสต์ โดยไม่รับประกันว่าจะดึงข้อมูลได้จากทุกแพลตฟอร์ม

X (Twitter) อาจมีข้อจำกัดด้าน API, Authentication, Rate Limit และการเข้าถึงข้อมูล จึงต้องมี Fallback สำหรับกรณีที่ไม่สามารถดึงรูปภาพได้

### 8.2 URL Validation

ก่อนบันทึก URL ระบบควรตรวจสอบว่า:
- URL อยู่ในรูปแบบที่ถูกต้อง
- เป็น URL ที่ระบบอนุญาตให้จัดเก็บ
- ไม่ใช่ URL ที่เป็นอันตรายหรือใช้โจมตีระบบ
- สามารถตรวจสอบ URL ซ้ำใน Database ได้

### 8.3 Duplicate URL Handling

เมื่อพบ URL ที่มีอยู่แล้ว ระบบต้องแจ้งเตือนและให้ผู้ใช้เลือกได้ 3 วิธี:
1. ยกเลิกการเพิ่ม
2. ยืนยันเพิ่มรายการซ้ำ
3. อัปเดตรายการเดิม

การอัปเดตรายการเดิมต้องมี Preview เพื่อเปรียบเทียบข้อมูลก่อนยืนยัน ระบบไม่ควรเขียนทับ Category, People, Event, Status และ Favorite โดยไม่ได้รับความยินยอมจากผู้ใช้

### 8.4 Security Considerations

การดึงข้อมูลจาก URL ภายนอกต้องระมัดระวัง:
- URL ที่ชี้ไปยัง Internal Network
- URL ที่ Redirect ไปยังปลายทางที่ไม่ปลอดภัย
- ไฟล์ที่มีขนาดใหญ่ผิดปกติ
- Content Type ที่ไม่ตรงกับชนิดไฟล์
- การดึงข้อมูลที่ใช้เวลานานหรือไม่ตอบสนอง

หากใช้ Backend เพื่อดึงข้อมูล ต้องตรวจสอบ URL และปลายทางก่อน Request ทุกครั้ง เพื่อป้องกันความเสี่ยงประเภท SSRF

## 9. Status Management และการคำนวณสถานะ

### 9.1 สถานะที่ผู้ใช้เห็น

| Status | ความหมาย |
|---|---|
| INBOX | รายการที่เพิ่มเข้ามาแล้ว แต่ยังไม่ได้จัดข้อมูล |
| TO_COLLECT | รายการที่ยังไม่ได้รับและ Event ยังไม่สิ้นสุด |
| COLLECTED | รายการที่ได้รับแล้ว หรือรายการที่ผู้ใช้เลือกเก็บไว้ในคลัง |
| EXPIRED | รายการที่ Event สิ้นสุดแล้ว แต่ยังไม่ได้รับ |

### 9.2 แนวทางที่เลือก: Database View

ใช้ Database View เพื่อคำนวณสถานะที่ผู้ใช้เห็นจากข้อมูล Giveaway และวันสิ้นสุดของ Event แทนการบันทึกสถานะ TO_COLLECT และ EXPIRED ลงในตารางหลัก

แนวทางนี้ไม่ต้องใช้ Cron Job, Scheduled Task หรือ Background Service เพื่อเปลี่ยนสถานะทุกวัน จึงช่วยลดงานเบื้องหลังและค่าใช้จ่ายเพิ่มเติม

### 9.3 หลักการคำนวณ

ตาราง `giveaways` เก็บ `lifecycle_state` สำหรับสถานะที่ต้องบันทึกจริง:
- `INBOX` — รายการที่ยังไม่ได้จัดข้อมูล
- `ACTIVE` — รายการที่อยู่ระหว่างรอรับของและผูกกับ Event
- `COLLECTED` — รายการที่ได้รับแล้วหรือผู้ใช้เลือกเก็บไว้ในคลัง

Database View เช่น `giveaway_status_view` คำนวณสถานะที่แสดงผล:
- หาก `lifecycle_state = INBOX` ให้แสดง INBOX
- หาก `lifecycle_state = COLLECTED` ให้แสดง COLLECTED
- หาก `lifecycle_state = ACTIVE` และมี Event ที่ยังไม่ผ่านวันสิ้นสุด ให้แสดง TO_COLLECT
- หาก `lifecycle_state = ACTIVE` และวันปัจจุบันผ่านวันสิ้นสุด Event แล้ว ให้แสดง EXPIRED
- หากข้อมูล ACTIVE ไม่มี Event ให้จัดการตามกฎข้อมูลที่กำหนด โดยไม่ปล่อยให้เกิดสถานะที่ไม่ชัดเจน

การเปรียบเทียบวันให้ใช้เขตเวลา `Asia/Bangkok` อย่างสม่ำเสมอ เช่น คำนวณวันที่ปัจจุบันจาก `now() AT TIME ZONE 'Asia/Bangkok'` ก่อนเปรียบเทียบกับ `end_date`

### 9.4 การใช้งาน View

- ทุกหน้าที่แสดงสถานะต้องอ้างอิง View หรือ Logic กลางเดียวกัน
- Filtering ตาม Status ต้องใช้สถานะที่คำนวณแล้ว
- Dashboard และ Event View ต้องนับจำนวนจากสถานะที่คำนวณแล้ว
- เมื่อวันเปลี่ยนผ่านวันสิ้นสุด Event สถานะที่ Query ครั้งถัดไปต้องเปลี่ยนได้โดยไม่ต้องมี Job มาอัปเดตข้อมูล
- ต้องกำหนดสิทธิ์การเข้าถึง View ให้สอดคล้องกับ RLS ของตารางต้นทาง โดยใช้รูปแบบ View ที่ไม่ข้ามข้อกำหนดด้านความปลอดภัย

### 9.5 Re-Collect

เมื่อผู้ใช้เลือก Re-Collect จาก EXPIRED ระบบจะสร้าง Giveaway รายการใหม่ โดยต้องเลือก Event หรือวันที่ใหม่ก่อน รายการเดิมยังคงอยู่ใน EXPIRED และไม่ถูกเขียนทับ

## 10. Filtering & Query

ระบบเน้นการกรองข้อมูลมากกว่าการค้นหาด้วยข้อความ

ตัวกรองที่รองรับ:
- URL
- Category
- People
- Event
- Status
- Favorite

### 10.1 Filtering Rules

- สามารถใช้ตัวกรองหลายกลุ่มร่วมกันได้
- เมื่อเลือกหลาย Category ต้องแสดงรายการที่ตรงกับ Category ทุกค่าที่เลือก (AND)
- เมื่อเลือกหลาย People Tags ต้องแสดงรายการที่ตรงกับ People Tags ทุกค่าที่เลือก (AND)
- ตัวกรองคนละกลุ่มต้องนำมาจำกัดผลลัพธ์ร่วมกัน
- การกรองต้องไม่ทำให้ข้อมูลต้นฉบับเปลี่ยนแปลง
- การกรอง Status ต้องใช้สถานะจาก Database View เพื่อให้ตรงกับ Dashboard และหน้าอื่น

### 10.2 Performance

- ใช้ Database Query สำหรับการกรองข้อมูลหลัก
- เพิ่ม Index ให้กับ Field ที่ใช้กรองบ่อยตามความเหมาะสม
- โหลดข้อมูลเท่าที่จำเป็น
- หากจำนวนข้อมูลเพิ่มขึ้น ให้พิจารณา Pagination หรือ Infinite Scroll

## 11. Data Recovery

ในเวอร์ชันแรกจะเน้นให้ระบบบันทึกและจัดการข้อมูลได้อย่างถูกต้องก่อน ยังไม่พัฒนาหน้า Export / Import ภายในเว็บ

แนวทาง:
- ตรวจสอบผลการบันทึกข้อมูลหลังการเพิ่ม แก้ไข และลบ
- ป้องกันการสูญหายของข้อมูลจากการเปลี่ยนสถานะ
- จัดการข้อผิดพลาดจาก Network หรือ Database โดยไม่แสดงว่าบันทึกสำเร็จหากยังไม่สำเร็จจริง
- แจ้งผู้ใช้เมื่อการบันทึกหรืออัปโหลดล้มเหลว
- ไม่ลบข้อมูลต้นฉบับก่อนยืนยันว่าการดำเนินการที่เกี่ยวข้องสำเร็จ
- วางโครงสร้างข้อมูลให้สามารถเพิ่ม Export / Import ในอนาคตได้

การสำรองข้อมูลผ่านเครื่องมือของผู้ให้บริการและการสำรองข้อมูลภายนอกยังเป็นสิ่งที่ควรพิจารณา เนื่องจาก Free Tier อาจมีข้อจำกัดด้าน Backup

## 12. Deployment & Environment

### 12.1 Environment

แยกค่าการตั้งค่าตาม Environment:
- Development — ใช้สำหรับพัฒนาและทดสอบ
- Production — ใช้งานจริง

ข้อมูลและ Secret ของแต่ละ Environment ต้องไม่ปะปนกัน

### 12.2 Deployment

ใช้ GitHub เป็น Repository หลัก และ Vercel สำหรับ Deploy Frontend

แนวทาง:
1. พัฒนาและทดสอบใน Local Environment
2. Push Code ไปยัง GitHub
3. ตรวจสอบ Build และ Automated Checks
4. Deploy ไปยัง Production
5. ทดสอบการใช้งานหลัง Deploy

### 12.3 Environment Variables

ตัวอย่าง Environment Variables สำหรับ Frontend:

```env
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

ไฟล์ `.env` ต้องไม่ถูก Commit ขึ้น GitHub หากมี Backend Function หรือบริการเพิ่มเติมที่ต้องใช้ Secret ให้จัดเก็บใน Environment ที่เหมาะสมและไม่เปิดเผยให้ Frontend เข้าถึง

## 13. Responsive Design & PWA

ระบบต้องรองรับ:
- Desktop
- Tablet
- Mobile

Layout ต้องเหมาะกับขนาดหน้าจอ โดยไม่ทำให้ฟังก์ชันหลักหายไปเมื่อใช้งานบน Mobile

PWA สามารถใช้เพื่อให้ผู้ใช้เพิ่ม Givie ลงในหน้าจอหลักได้ โดยในระยะแรกเน้นการเข้าถึงเว็บได้สะดวก ไม่จำเป็นต้องรองรับ Offline Data Synchronization ทั้งนี้รายละเอียดการทำ PWA สามารถกำหนดในช่วงพัฒนาได้

## 14. Performance & Reliability

แนวทาง:
- ใช้ Lazy Loading สำหรับรูปภาพ
- ปรับขนาดและบีบอัดรูปภาพก่อนอัปโหลด
- โหลดข้อมูลเท่าที่จำเป็น
- ใช้ Database Index กับ Field ที่เหมาะสม
- แยกการประมวลผลรูปภาพออกจากการโหลดหน้าหลักเมื่อทำได้
- จัดการ Loading, Empty State และ Error State อย่างเหมาะสม
- หาก External Service ใช้งานไม่ได้ ระบบหลักยังต้องใช้งานได้
- หากดึงรูปจาก URL ไม่สำเร็จ ต้องยังสามารถเพิ่ม Giveaway ด้วยตนเองได้
- แสดงผลการบันทึกข้อมูลตามผลลัพธ์จริงจาก Database

## 15. Testing Strategy

### 15.1 Unit Testing

ทดสอบ Logic ที่แยกจาก UI ได้ เช่น:
- การคำนวณสถานะตามวัน Event และเขตเวลาไทย
- การตรวจสอบ URL
- การจัดการ Duplicate URL
- การกรอง Category และ People Tags
- การจัดการ Re-Collect
- การตรวจสอบข้อมูลก่อนบันทึก

### 15.2 Integration Testing

ทดสอบการทำงานร่วมกัน เช่น:
- Frontend กับ Database
- Authentication กับ RLS
- การเพิ่มและแก้ไข Giveaway
- การอัปโหลดและลบรูปภาพ
- การสร้างและลบ Event
- Database View สำหรับคำนวณสถานะ
- การกรองและนับจำนวนจากสถานะที่คำนวณแล้ว

### 15.3 End-to-End Testing

ทดสอบ User Journey ที่สำคัญ:
1. Login เข้าสู่ระบบ
2. เพิ่ม Giveaway ด้วย URL
3. จัด Category และ People Tags
4. ผูก Giveaway กับ Event
5. กรองข้อมูลจากหลายเงื่อนไข
6. ทำเครื่องหมายว่าได้รับของแล้ว
7. ตรวจสอบการคำนวณเป็น EXPIRED เมื่อ Event ผ่านวันสิ้นสุด
8. Re-Collect รายการที่หมดอายุ
9. ลบ Event และจัดการ Giveaway ที่เกี่ยวข้อง
10. Logout และตรวจสอบว่าไม่สามารถเข้าถึงข้อมูลส่วนตัวได้
11. ใช้งานผ่านอุปกรณ์ที่มีขนาดหน้าจอต่างกัน

## 16. Cost Management

เนื่องจากต้องการให้ Givie ใช้งานได้โดยไม่มีค่าใช้จ่ายประจำเป็นหลัก จึงเลือกบริการ Free Tier และควบคุมการใช้งาน

แนวทาง:
- ใช้ Supabase และ Vercel Free Tier เป็นหลัก
- ลดขนาดรูปภาพก่อนอัปโหลด
- ไม่เก็บไฟล์ที่ไม่จำเป็น
- หลีกเลี่ยงการ Request External Service ซ้ำโดยไม่จำเป็น
- ใช้ Database View คำนวณสถานะแทน Scheduled Task ในระยะแรก
- ตรวจสอบปริมาณ Database และ Storage เป็นระยะ
- ประเมินค่าใช้จ่ายก่อนเปลี่ยนเป็นบริการแบบชำระเงิน

## 17. Development Priority

### Phase 1: Core System
- ตั้งค่า Frontend และ Backend
- ตั้งค่า Database
- ตั้งค่า Authentication และ Login
- ตั้งค่า RLS
- สร้างระบบ CRUD สำหรับ Giveaway, Event, Category และ People Tags
- รองรับการบันทึกและจัดการสถานะหลัก
- ตรวจสอบการ Sync ข้อมูลระหว่างอุปกรณ์

### Phase 2: Image & Data Management
- Image Upload
- Image Processing
- Image Placeholder
- External Image Fetching
- Duplicate URL Handling

### Phase 3: Collection Features
- Filtering
- Event View
- People Collection
- Favorite
- Quick Checklist
- Re-Collect
- Batch Add
- Database View สำหรับคำนวณ TO_COLLECT และ EXPIRED

### Phase 4: Reliability & Deployment
- Responsive Testing
- พิจารณา PWA
- Production Deployment
- ตรวจสอบความปลอดภัยและสิทธิ์การเข้าถึง
- ตรวจสอบการจัดเก็บข้อมูลและแนวทางกู้คืน
- พิจารณา Export / Import ในอนาคต

## 18. Open Decisions

| หัวข้อ | ข้อสรุป / แนวทาง | สถานะ |
|---|---|---|
| Frontend | React + TypeScript + Vite | เลือกแล้ว |
| Styling | Tailwind CSS | เลือกแล้ว |
| Backend / Database | Supabase + PostgreSQL | เลือกแล้ว |
| Authentication | Supabase Auth และจำกัดบัญชีเฉพาะเจ้าของ | เลือกแล้ว |
| Image Storage | Supabase Storage | เลือกแล้ว |
| Hosting | Vercel | เลือกแล้ว |
| PWA | พิจารณาระหว่างพัฒนา ไม่จำเป็นต้องมี Offline Sync | ยังไม่ล็อก |
| External Image Fetching | ตรวจสอบความเป็นไปได้และข้อจำกัดของ X | รอตรวจสอบ |
| Automatic Expiration | Database View คำนวณสถานะเมื่อ Query ไม่ใช้ Scheduled Task | เลือกแล้ว |
| Export / Import | ยังไม่ทำในเวอร์ชันแรก | เลื่อนไปอนาคต |
| Backup | พิจารณาวิธีสำรองข้อมูลที่เหมาะกับ Free Tier | รอตรวจสอบ |

---

## 19. Related Documents

- `PRODUCT_SPEC.md` — เป้าหมายและขอบเขตของผลิตภัณฑ์
- `FEATURE_SPEC.md` — รายละเอียดการทำงานและ Business Rules
- `UX_DESIGN.md` — โครงสร้างหน้าเว็บและแนวทางการออกแบบ UI
- `README.md` — ภาพรวมโปรเจกต์และวิธีใช้งาน

---

**หมายเหตุ:** เอกสารนี้กำหนดแนวทางทางเทคนิคของ Givie ตามการตัดสินใจของเจ้าของโปรเจกต์ รายละเอียดเชิง Implementation เช่น Schema จริง, RLS Policies และกระบวนการประมวลผลรูปภาพ สามารถกำหนดเพิ่มเติมในระหว่างการพัฒนาได้ โดยต้องไม่ขัดกับข้อกำหนดหลักของโปรเจกต์
