# FixFast - Technical Request System

ระบบเรียกช่างออนไลน์ที่รองรับการทำงานผ่าน Mobile App และ Web Admin

## สารบัญ

1. [ภาพรวมระบบ](#ภาพรวมระบบ)
2. [สถาปัตยกรรมระบบ](#สถาปัตยกรรมระบบ)
3. [เทคโนโลยีที่ใช้](#เทคโนโลยีที่ใช้)
4. [โครงสร้างโปรเจค](#โครงสร้างโปรเจค)
5. [การติดตั้ง](#การติดตั้ง)
6. [เอกสารเพิ่มเติม](#เอกสารเพิ่มเติม)

## ภาพรวมระบบ

ระบบ FixFast เป็นแพลตฟอร์มที่เชื่อมโยงระหว่างลูกค้า (Customer) กับช่างเทคนิค (Technical) โดยมีผู้ดูแลระบบ (Admin) คอยควบคุมและจัดการระบบ

### บทบาทผู้ใช้งาน (User Roles)

1. **Customer (ลูกค้า)** - ใช้งานผ่าน Mobile App
   - สร้างคำขอซ่อม/บริการ
   - ติดตามสถานะงาน
   - ให้คะแนนและรีวิว
   - ชำระเงินผ่านระบบ

2. **Technical (ช่าง)** - ใช้งานผ่าน Mobile App
   - รับงานที่ตรงกับความเชี่ยวชาญ
   - อัพเดทสถานะงาน
   - แจ้งค่าใช้จ่าย
   - รับการชำระเงิน

3. **Admin (ผู้ดูแลระบบ)** - ใช้งานผ่าน Web Dashboard
   - จัดการผู้ใช้งานทั้งหมด
   - ติดตามและจัดการคำขอซ่อม
   - ดูรายงานและสถิติ
   - จัดการหมวดหมู่บริการ
   - จัดการการชำระเงิน

## สถาปัตยกรรมระบบ

```
┌─────────────────────────────────────────────────────────┐
│                    Internet/Cloud                        │
└─────────────────────────────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Customer   │  │  Technical   │  │    Admin     │
│  Mobile App  │  │  Mobile App  │  │Web Dashboard │
│  (Flutter)   │  │  (Flutter)   │  │  (Next.js)   │
└──────────────┘  └──────────────┘  └──────────────┘
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                           ▼
                ┌──────────────────┐
                │   Backend API    │
                │   (Node.js/      │
                │   Express)       │
                └──────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   MongoDB    │  │   Firebase   │  │   Payment    │
│   Database   │  │   (Auth,     │  │   Gateway    │
│              │  │   Storage)   │  │              │
└──────────────┘  └──────────────┘  └──────────────┘
```

## เทคโนโลยีที่ใช้

### Frontend
- **Customer App**: Flutter (iOS/Android)
- **Technical App**: Flutter (iOS/Android)
- **Admin Web**: Next.js 14+ with TypeScript

### Backend
- **API Server**: Node.js with Express.js
- **Real-time**: Socket.io for live updates
- **Authentication**: Firebase Authentication / JWT

### Database
- **Primary Database**: MongoDB
- **File Storage**: Firebase Storage / AWS S3
- **Cache**: Redis (optional)

### Additional Services
- **Maps**: Google Maps API
- **Push Notifications**: Firebase Cloud Messaging (FCM)
- **Payment**: Payment Gateway Integration (e.g., Stripe, Omise)

## โครงสร้างโปรเจค

```
app-technical-request-FixFast/
├── docs/                          # เอกสารทั้งหมด
│   ├── database/                  # Database schema
│   ├── api/                       # API documentation
│   ├── diagrams/                  # Flow & Sequence diagrams
│   └── architecture/              # System architecture
├── customer-app/                  # Flutter app สำหรับลูกค้า
├── technical-app/                 # Flutter app สำหรับช่าง
├── admin-web/                     # Next.js web dashboard
├── backend/                       # Backend API server
└── shared/                        # Shared code/types
```

## การติดตั้ง

### ข้อกำหนดระบบ

- Node.js 18+ (สำหรับ Backend และ Admin Web)
- Flutter 3.0+ (สำหรับ Mobile Apps)
- MongoDB 6.0+
- Firebase Account (สำหรับ Authentication และ Storage)

### ขั้นตอนการติดตั้ง

รายละเอียดการติดตั้งแต่ละส่วนดูได้ที่:
- [Backend Setup](./backend/README.md)
- [Customer App Setup](./customer-app/README.md)
- [Technical App Setup](./technical-app/README.md)
- [Admin Web Setup](./admin-web/README.md)

## เอกสารเพิ่มเติม

- [Database Schema](./docs/database/README.md) - โครงสร้างฐานข้อมูล MongoDB
- [API Documentation](./docs/api/README.md) - API Endpoints ทั้งหมด
- [System Flows](./docs/diagrams/flows.md) - Flow diagrams ของระบบ
- [Sequence Diagrams](./docs/diagrams/sequences.md) - Sequence diagrams สำหรับกระบวนการสำคัญ
- [Architecture Guide](./docs/architecture/README.md) - คู่มือสถาปัตยกรรมระบบ

## Features

### Customer App Features
- ✅ ลงทะเบียนและเข้าสู่ระบบ
- ✅ สร้างคำขอบริการ/ซ่อม
- ✅ เลือกประเภทบริการ
- ✅ แนบรูปภาพและรายละเอียด
- ✅ ติดตามสถานะงานแบบ Real-time
- ✅ แชทกับช่าง
- ✅ ให้คะแนนและรีวิว
- ✅ ประวัติการใช้บริการ
- ✅ การชำระเงิน

### Technical App Features
- ✅ ลงทะเบียนพร้อมข้อมูลความเชี่ยวชาญ
- ✅ ดูงานที่เปิดรับใกล้เคียง
- ✅ รับงาน/ปฏิเสธงาน
- ✅ อัพเดทสถานะงาน
- ✅ แชทกับลูกค้า
- ✅ แจ้งราคาและค่าใช้จ่าย
- ✅ Navigation ไปยังจุดหมาย
- ✅ ประวัติการทำงาน
- ✅ รายได้และสรุปการเงิน

### Admin Web Features
- ✅ Dashboard สรุปภาพรวม
- ✅ จัดการผู้ใช้งาน (Customer/Technical)
- ✅ จัดการคำขอบริการทั้งหมด
- ✅ ติดตามสถานะงานแบบ Real-time
- ✅ รายงานและสถิติ
- ✅ จัดการหมวดหมู่บริการ
- ✅ ระบบการชำระเงิน
- ✅ จัดการรีวิวและการให้คะแนน
- ✅ ระบบแจ้งเตือน

## License

MIT License

## Contact

For questions or support, please contact the development team.