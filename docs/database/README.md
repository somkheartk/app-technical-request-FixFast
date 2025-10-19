# Database Schema - MongoDB

โครงสร้างฐานข้อมูล MongoDB สำหรับระบบ FixFast

## Collections Overview

1. **users** - ข้อมูลผู้ใช้งานทั้งหมด
2. **service_requests** - คำขอบริการ/ซ่อม
3. **categories** - หมวดหมู่บริการ
4. **reviews** - รีวิวและคะแนน
5. **chats** - ข้อความแชท
6. **payments** - ข้อมูลการชำระเงิน
7. **notifications** - การแจ้งเตือน
8. **technician_profiles** - ข้อมูลเฉพาะช่าง

---

## 1. Users Collection

เก็บข้อมูลผู้ใช้งานทุกประเภท (Customer, Technical, Admin)

```javascript
{
  _id: ObjectId,
  firebaseUid: String,              // Firebase Authentication UID
  email: String,                    // อีเมล (unique)
  phoneNumber: String,              // เบอร์โทรศัพท์
  role: String,                     // "customer", "technical", "admin"
  profile: {
    firstName: String,
    lastName: String,
    displayName: String,
    avatarUrl: String,
    dateOfBirth: Date,
    gender: String                  // "male", "female", "other"
  },
  address: {
    street: String,
    district: String,
    city: String,
    province: String,
    postalCode: String,
    country: String,
    location: {
      type: "Point",
      coordinates: [Number, Number]  // [longitude, latitude]
    }
  },
  status: String,                   // "active", "inactive", "suspended"
  verified: Boolean,                // ยืนยันตัวตนแล้วหรือไม่
  createdAt: Date,
  updatedAt: Date,
  lastLoginAt: Date
}

// Indexes
db.users.createIndex({ "firebaseUid": 1 }, { unique: true })
db.users.createIndex({ "email": 1 }, { unique: true })
db.users.createIndex({ "phoneNumber": 1 })
db.users.createIndex({ "role": 1 })
db.users.createIndex({ "address.location": "2dsphere" })
```

---

## 2. Technician Profiles Collection

ข้อมูลเฉพาะสำหรับช่างเทคนิค

```javascript
{
  _id: ObjectId,
  userId: ObjectId,                 // Reference to users collection
  specializations: [String],        // หมวดหมู่ที่ถนัด
  experience: Number,               // จำนวนปีประสบการณ์
  certifications: [{
    name: String,
    issuer: String,
    issueDate: Date,
    expiryDate: Date,
    documentUrl: String
  }],
  availability: {
    isAvailable: Boolean,           // พร้อมรับงานหรือไม่
    workingHours: [{
      day: String,                  // "monday", "tuesday", etc.
      startTime: String,            // "09:00"
      endTime: String               // "18:00"
    }],
    maxRadius: Number               // รัศมีการรับงาน (กิโลเมตร)
  },
  rating: {
    average: Number,                // คะแนนเฉลี่ย
    count: Number                   // จำนวนรีวิว
  },
  statistics: {
    totalJobs: Number,              // จำนวนงานทั้งหมด
    completedJobs: Number,          // จำนวนงานที่เสร็จสิ้น
    cancelledJobs: Number,          // จำนวนงานที่ยกเลิก
    responseTime: Number            // เวลาตอบกลับเฉลี่ย (นาที)
  },
  documents: {
    idCardUrl: String,
    idCardNumber: String,
    idCardVerified: Boolean
  },
  bankAccount: {
    bankName: String,
    accountNumber: String,
    accountName: String
  },
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.technician_profiles.createIndex({ "userId": 1 }, { unique: true })
db.technician_profiles.createIndex({ "specializations": 1 })
db.technician_profiles.createIndex({ "rating.average": -1 })
db.technician_profiles.createIndex({ "availability.isAvailable": 1 })
```

---

## 3. Categories Collection

หมวดหมู่บริการต่างๆ

```javascript
{
  _id: ObjectId,
  name: {
    th: String,
    en: String
  },
  description: {
    th: String,
    en: String
  },
  icon: String,                     // URL หรือชื่อไอคอน
  parentId: ObjectId,               // สำหรับหมวดหมู่ย่อย (null ถ้าเป็นหมวดหมู่หลัก)
  isActive: Boolean,
  sortOrder: Number,
  estimatedPrice: {
    min: Number,
    max: Number,
    unit: String                    // "job", "hour", "item"
  },
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.categories.createIndex({ "isActive": 1, "sortOrder": 1 })
db.categories.createIndex({ "parentId": 1 })
```

---

## 4. Service Requests Collection

คำขอบริการ/ซ่อม

```javascript
{
  _id: ObjectId,
  requestNumber: String,            // เลขที่คำขอ (unique, auto-generated)
  customerId: ObjectId,             // Reference to users
  technicianId: ObjectId,           // Reference to users (null ถ้ายังไม่มีช่างรับงาน)
  categoryId: ObjectId,             // Reference to categories
  
  details: {
    title: String,                  // หัวข้อ
    description: String,            // รายละเอียด
    images: [String],               // URLs ของรูปภาพ
    urgency: String                 // "low", "medium", "high"
  },
  
  location: {
    address: String,
    details: String,                // รายละเอียดที่อยู่เพิ่มเติม
    coordinates: {
      type: "Point",
      coordinates: [Number, Number] // [longitude, latitude]
    }
  },
  
  schedule: {
    preferredDate: Date,
    preferredTime: String,          // "morning", "afternoon", "evening"
    isFlexible: Boolean
  },
  
  status: String,                   // "pending", "accepted", "in_progress", "completed", "cancelled"
  statusHistory: [{
    status: String,
    timestamp: Date,
    note: String,
    updatedBy: ObjectId            // Reference to users
  }],
  
  pricing: {
    estimatedCost: Number,
    quotedCost: Number,             // ราคาที่ช่างเสนอ
    finalCost: Number,              // ราคาจริงที่เสร็จงาน
    currency: String                // "THB"
  },
  
  payment: {
    status: String,                 // "pending", "paid", "refunded"
    method: String,                 // "cash", "credit_card", "bank_transfer"
    transactionId: String,
    paidAt: Date
  },
  
  timeline: {
    requestedAt: Date,
    acceptedAt: Date,
    startedAt: Date,
    completedAt: Date,
    cancelledAt: Date
  },
  
  cancellation: {
    cancelledBy: String,            // "customer", "technician", "admin"
    reason: String,
    timestamp: Date
  },
  
  assignmentAttempts: [{
    technicianId: ObjectId,
    offeredAt: Date,
    respondedAt: Date,
    response: String                // "accepted", "declined", "expired"
  }],
  
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.service_requests.createIndex({ "requestNumber": 1 }, { unique: true })
db.service_requests.createIndex({ "customerId": 1 })
db.service_requests.createIndex({ "technicianId": 1 })
db.service_requests.createIndex({ "status": 1 })
db.service_requests.createIndex({ "categoryId": 1 })
db.service_requests.createIndex({ "location.coordinates": "2dsphere" })
db.service_requests.createIndex({ "createdAt": -1 })
db.service_requests.createIndex({ "timeline.requestedAt": -1 })
```

---

## 5. Reviews Collection

รีวิวและคะแนน

```javascript
{
  _id: ObjectId,
  serviceRequestId: ObjectId,      // Reference to service_requests
  reviewerId: ObjectId,             // ผู้รีวิว (customer)
  revieweeId: ObjectId,             // ผู้ถูกรีวิว (technician)
  
  rating: {
    overall: Number,                // 1-5
    quality: Number,                // คุณภาพงาน
    punctuality: Number,            // ตรงเวลา
    professionalism: Number,        // ความเป็นมือโปร
    communication: Number           // การสื่อสาร
  },
  
  comment: String,
  images: [String],                 // รูปภาพประกอบรีวิว
  
  response: {                       // คำตอบจากช่าง
    comment: String,
    respondedAt: Date
  },
  
  isVisible: Boolean,               // แสดงรีวิวหรือไม่
  isFlagged: Boolean,               // ถูกรายงานหรือไม่
  
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.reviews.createIndex({ "serviceRequestId": 1 }, { unique: true })
db.reviews.createIndex({ "revieweeId": 1, "createdAt": -1 })
db.reviews.createIndex({ "rating.overall": 1 })
db.reviews.createIndex({ "isVisible": 1 })
```

---

## 6. Chats Collection

ระบบแชทระหว่างลูกค้าและช่าง

```javascript
{
  _id: ObjectId,
  serviceRequestId: ObjectId,      // Reference to service_requests
  participants: [{
    userId: ObjectId,
    role: String,                   // "customer", "technician"
    joinedAt: Date,
    lastReadAt: Date
  }],
  
  messages: [{
    messageId: String,              // Unique message ID
    senderId: ObjectId,
    content: String,
    type: String,                   // "text", "image", "location", "system"
    attachments: [{
      type: String,                 // "image", "document"
      url: String,
      thumbnailUrl: String,
      size: Number
    }],
    isRead: Boolean,
    isEdited: Boolean,
    timestamp: Date
  }],
  
  status: String,                   // "active", "archived"
  
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.chats.createIndex({ "serviceRequestId": 1 })
db.chats.createIndex({ "participants.userId": 1 })
db.chats.createIndex({ "messages.timestamp": -1 })
```

---

## 7. Payments Collection

ข้อมูลการชำระเงิน

```javascript
{
  _id: ObjectId,
  serviceRequestId: ObjectId,      // Reference to service_requests
  customerId: ObjectId,
  technicianId: ObjectId,
  
  amount: {
    subtotal: Number,
    serviceFee: Number,             // ค่าธรรมเนียมแพลตฟอร์ม
    tax: Number,
    discount: Number,
    total: Number,
    currency: String
  },
  
  paymentMethod: String,            // "cash", "credit_card", "bank_transfer", "wallet"
  paymentProvider: String,          // "stripe", "omise", etc.
  
  transaction: {
    externalId: String,             // ID จาก payment gateway
    status: String,                 // "pending", "processing", "completed", "failed", "refunded"
    paidAt: Date,
    refundedAt: Date,
    failureReason: String
  },
  
  payout: {                         // การจ่ายเงินให้ช่าง
    amount: Number,
    status: String,                 // "pending", "processing", "completed", "failed"
    scheduledAt: Date,
    completedAt: Date,
    transactionId: String
  },
  
  receipt: {
    receiptNumber: String,
    receiptUrl: String,
    issuedAt: Date
  },
  
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.payments.createIndex({ "serviceRequestId": 1 })
db.payments.createIndex({ "customerId": 1 })
db.payments.createIndex({ "technicianId": 1 })
db.payments.createIndex({ "transaction.status": 1 })
db.payments.createIndex({ "createdAt": -1 })
```

---

## 8. Notifications Collection

การแจ้งเตือน

```javascript
{
  _id: ObjectId,
  userId: ObjectId,                 // Reference to users
  type: String,                     // "request_update", "new_job", "payment", "review", "system"
  
  title: String,
  message: String,
  
  data: {                           // ข้อมูลเพิ่มเติมตามประเภท
    serviceRequestId: ObjectId,
    action: String,                 // URL หรือ action ที่จะทำเมื่อกดแจ้งเตือน
    metadata: Object
  },
  
  priority: String,                 // "low", "medium", "high"
  
  delivery: {
    push: {
      sent: Boolean,
      sentAt: Date,
      fcmMessageId: String
    },
    email: {
      sent: Boolean,
      sentAt: Date
    }
  },
  
  isRead: Boolean,
  readAt: Date,
  
  createdAt: Date,
  expiresAt: Date                   // แจ้งเตือนหมดอายุ
}

// Indexes
db.notifications.createIndex({ "userId": 1, "createdAt": -1 })
db.notifications.createIndex({ "isRead": 1 })
db.notifications.createIndex({ "expiresAt": 1 }, { expireAfterSeconds: 0 })
```

---

## Database Relationships

```
users (Customer) ─── 1:N ──→ service_requests
users (Technical) ─── 1:N ──→ service_requests (as technician)
users (Technical) ─── 1:1 ──→ technician_profiles
service_requests ─── N:1 ──→ categories
service_requests ─── 1:1 ──→ reviews
service_requests ─── 1:1 ──→ chats
service_requests ─── 1:N ──→ payments
users ─── 1:N ──→ notifications
```

---

## Data Validation Rules

### User Role Validation
- Email must be unique and valid format
- Phone number must be valid Thai format
- Role must be one of: "customer", "technical", "admin"

### Service Request Validation
- Status transitions must follow valid flow
- Customer can only create requests
- Technician can only accept/reject requests
- Admin can modify any status

### Payment Validation
- Amount must be positive
- Payment status must follow valid flow
- Refund amount cannot exceed original amount

---

## Backup and Retention Policy

- **Full Backup**: Daily at 2:00 AM (UTC+7)
- **Incremental Backup**: Every 6 hours
- **Retention**: 
  - Active data: Indefinite
  - Completed requests: 2 years
  - Logs: 90 days
  - Soft-deleted data: 30 days before permanent deletion

---

## Performance Considerations

1. **Geospatial Queries**: Use 2dsphere indexes for location-based searches
2. **Caching**: Cache frequently accessed data (categories, user profiles)
3. **Pagination**: Implement cursor-based pagination for large datasets
4. **Archiving**: Move old completed requests to archive collection
5. **Indexing**: Create compound indexes for common query patterns

---

## Security Best Practices

1. **Data Encryption**: Encrypt sensitive data at rest and in transit
2. **Access Control**: Implement role-based access control (RBAC)
3. **PII Protection**: Mask sensitive personal information in logs
4. **Audit Trail**: Log all critical data modifications
5. **Data Anonymization**: Anonymize data for analytics and testing
