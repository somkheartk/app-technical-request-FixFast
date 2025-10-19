# Sequence Diagrams - FixFast

Sequence diagrams แสดงการทำงานระหว่างส่วนต่างๆ ของระบบ

## สารบัญ

1. [User Authentication Sequence](#1-user-authentication-sequence)
2. [Service Request Creation Sequence](#2-service-request-creation-sequence)
3. [Job Assignment Sequence](#3-job-assignment-sequence)
4. [Real-time Chat Sequence](#4-real-time-chat-sequence)
5. [Payment Processing Sequence](#5-payment-processing-sequence)
6. [Review Submission Sequence](#6-review-submission-sequence)
7. [Admin Dashboard Sequence](#7-admin-dashboard-sequence)

---

## 1. User Authentication Sequence

### Customer Login with Firebase

```
Customer App    Firebase Auth    Backend API      MongoDB
    |                |               |              |
    |--Login Request->|               |              |
    |   (email/pwd)  |               |              |
    |                |               |              |
    |                |--Verify------->|              |
    |                |  Credentials   |              |
    |                |               |              |
    |                |<--Auth Token---|              |
    |                |    (JWT)       |              |
    |                |               |              |
    |<--Token--------|               |              |
    |                |               |              |
    |--Verify Token------------------>|              |
    |  & Get Profile                 |              |
    |                                |              |
    |                                |--Query------>|
    |                                |   User       |
    |                                |              |
    |                                |<--User Data--|
    |                                |              |
    |<--User Profile & Token---------|              |
    |                                |              |
    |--Store Token------------------>|              |
    |   (Local)                      |              |
    |                                |              |
```

### Technician Registration with Verification

```
Technical App  Firebase Auth  Backend API    MongoDB      Admin Web
    |               |             |            |              |
    |--Register---->|             |            |              |
    |   Request     |             |            |              |
    |               |             |            |              |
    |               |--Create---->|            |              |
    |               |   Account   |            |              |
    |               |             |            |              |
    |               |<--Auth------|            |              |
    |               |   Token     |            |              |
    |               |             |            |              |
    |<--Token-------|             |            |              |
    |               |             |            |              |
    |--Submit Profile------------->|            |              |
    |  & Documents                |            |              |
    |                             |            |              |
    |                             |--Save----->|              |
    |                             |  User +    |              |
    |                             |  Profile   |              |
    |                             |  (status:  |              |
    |                             |  pending)  |              |
    |                             |            |              |
    |                             |<--Saved----|              |
    |                             |            |              |
    |<--Registration Pending------|            |              |
    |   (Awaiting Approval)       |            |              |
    |                             |            |              |
    |                             |--Notify------------------->|
    |                             |  New Tech                 |
    |                             |  Registration             |
    |                             |            |              |
    |                             |            |              |
    |                             |<--Admin Approves----------|
    |                             |   Technician              |
    |                             |            |              |
    |                             |--Update--->|              |
    |                             |  Status:   |              |
    |                             |  active    |              |
    |                             |            |              |
    |<--Notification--------------|            |              |
    |   "Account Approved"        |            |              |
    |                             |            |              |
```

---

## 2. Service Request Creation Sequence

```
Customer App    Backend API     MongoDB    Notification    Technician App
                                           Service
    |               |             |            |                |
    |--Create------>|             |            |                |
    |  Request      |             |            |                |
    |               |             |            |                |
    |               |--Validate-->|            |                |
    |               |  Request    |            |                |
    |               |             |            |                |
    |               |--Generate-->|            |                |
    |               |  Request#   |            |                |
    |               |             |            |                |
    |               |--Save------>|            |                |
    |               |  Request    |            |                |
    |               |             |            |                |
    |               |<--Request---|            |                |
    |               |   Created   |            |                |
    |               |             |            |                |
    |<--Success-----|             |            |                |
    |  + Request#   |             |            |                |
    |               |             |            |                |
    |               |--Find Nearby------------>|                |
    |               |  Technicians|            |                |
    |               |  (Geospatial|            |                |
    |               |   Query)    |            |                |
    |               |             |            |                |
    |               |<--List of---|            |                |
    |               |  Matched    |            |                |
    |               |  Technicians|            |                |
    |               |             |            |                |
    |               |--Send Push-------------->|                |
    |               |  Notifications           |                |
    |               |             |            |                |
    |               |             |            |--Push--------->|
    |               |             |            |  "New Job"    |
    |               |             |            |                |
    |               |             |            |<--Delivered----|
    |               |             |            |                |
    |               |<--Sent------|            |                |
    |               |             |            |                |
    |--Subscribe--->|             |            |                |
    |  to Updates   |             |            |                |
    |  (WebSocket)  |             |            |                |
    |               |             |            |                |
```

---

## 3. Job Assignment Sequence

```
Technician App  Backend API    MongoDB    WebSocket    Customer App
                                          Server
    |               |             |           |             |
    |--View Job---->|             |           |             |
    |  Details      |             |           |             |
    |               |             |           |             |
    |               |--Query----->|           |             |
    |               |  Request    |           |             |
    |               |             |           |             |
    |               |<--Details---|           |             |
    |               |             |           |             |
    |<--Job Data----|             |           |             |
    |               |             |           |             |
    |--Accept Job-->|             |           |             |
    |               |             |           |             |
    |               |--Check----->|           |             |
    |               |  Status     |           |             |
    |               |  (not taken)|           |             |
    |               |             |           |             |
    |               |<--Available-|           |             |
    |               |             |           |             |
    |               |--Update---->|           |             |
    |               |  Request:   |           |             |
    |               |  -technician|           |             |
    |               |  -status:   |           |             |
    |               |   accepted  |           |             |
    |               |             |           |             |
    |               |<--Updated---|           |             |
    |               |             |           |             |
    |<--Success-----|             |           |             |
    |  Assigned     |             |           |             |
    |               |             |           |             |
    |               |--Emit------>|           |             |
    |               |  Event:     |           |             |
    |               |  "job_      |           |             |
    |               |  accepted"  |           |             |
    |               |             |           |             |
    |               |             |---------->|             |
    |               |             |  Broadcast|             |
    |               |             |           |             |
    |               |             |           |--Notify---->|
    |               |             |           |  "Tech      |
    |               |             |           |  Found!"    |
    |               |             |           |             |
    |               |             |           |<--UI--------|
    |               |             |           |  Updated    |
    |               |             |           |             |
    |--Create------>|             |           |             |
    |  Chat Room    |             |           |             |
    |               |             |           |             |
    |               |--Create---->|           |             |
    |               |  Chat       |           |             |
    |               |             |           |             |
    |               |<--Created---|           |             |
    |               |             |           |             |
    |<--Chat Ready--|             |           |             |
    |               |             |           |             |
```

### Race Condition Handling (Multiple Accepts)

```
Tech 1 App   Tech 2 App   Backend API    MongoDB
    |            |             |            |
    |--Accept--->|             |            |
    |  (t=0ms)   |             |            |
    |            |--Accept---->|            |
    |            |  (t=10ms)   |            |
    |            |             |            |
    |            |             |--Atomic--->|
    |            |             |  Update    |
    |            |             |  with      |
    |            |             |  Condition:|
    |            |             |  status=   |
    |            |             |  pending   |
    |            |             |            |
    |            |             |<--Success--|
    |            |             |  (Tech 1)  |
    |            |             |            |
    |<--Success--|             |            |
    |            |             |            |
    |            |             |--Atomic--->|
    |            |             |  Update    |
    |            |             |  Attempt   |
    |            |             |            |
    |            |             |<--Failed---|
    |            |             |  (Already  |
    |            |             |  accepted) |
    |            |             |            |
    |            |<--Error-----|            |
    |            |  "Job Taken"|            |
    |            |             |            |
```

---

## 4. Real-time Chat Sequence

```
Customer App    WebSocket      Backend API    MongoDB    Technician App
                Server
    |               |               |            |             |
    |--Connect----->|               |            |             |
    |  WebSocket    |               |            |             |
    |               |               |            |             |
    |               |--Verify------>|            |             |
    |               |  Token        |            |             |
    |               |               |            |             |
    |               |<--Valid-------|            |             |
    |               |               |            |             |
    |<--Connected---|               |            |             |
    |               |               |            |             |
    |--Join Room--->|               |            |             |
    |  (request_id) |               |            |             |
    |               |               |            |             |
    |               |<--Verify Access---------->|             |
    |               |  (User in    |            |             |
    |               |   request)   |            |             |
    |               |               |            |             |
    |<--Joined------|               |            |             |
    |               |               |            |             |
    |               |               |            |<--Connect---|
    |               |               |            |  WebSocket  |
    |               |               |            |             |
    |               |               |<--Verify Token-----------|
    |               |               |            |             |
    |               |<--Connected---|            |             |
    |               |               |            |             |
    |               |<--Join Room---|            |             |
    |               |  (request_id) |            |             |
    |               |               |            |             |
    |--Send-------->|               |            |             |
    |  Message      |               |            |             |
    |               |               |            |             |
    |               |--Save-------->|            |             |
    |               |  Message      |            |             |
    |               |               |            |             |
    |               |               |--Insert--->|             |
    |               |               |  Message   |             |
    |               |               |            |             |
    |               |               |<--Saved----|             |
    |               |               |            |             |
    |               |<--Confirm-----|            |             |
    |               |               |            |             |
    |<--Delivered---|               |            |             |
    |               |               |            |             |
    |               |--Broadcast-----------------+------------>|
    |               |  Message to                |             |
    |               |  Room                      |             |
    |               |               |            |             |
    |               |               |            |<--Received--|
    |               |               |            |             |
    |               |               |            |             |
    |               |<--Send Push-------------->|             |
    |               |  Notification|            |             |
    |               |  (if offline)|            |             |
    |               |               |            |             |
```

---

## 5. Payment Processing Sequence

```
Customer App   Backend API   MongoDB   Payment     Bank/Gateway
                                       Service
    |              |           |         |              |
    |--Request---->|           |         |              |
    |  Payment     |           |         |              |
    |              |           |         |              |
    |              |--Get----->|         |              |
    |              |  Request  |         |              |
    |              |  Details  |         |              |
    |              |           |         |              |
    |              |<--Details-|         |              |
    |              |           |         |              |
    |              |--Calculate|         |              |
    |              |  Amount   |         |              |
    |              |  (with fee)|        |              |
    |              |           |         |              |
    |<--Payment----|           |         |              |
    |  Details     |           |         |              |
    |              |           |         |              |
    |--Confirm---->|           |         |              |
    |  Payment     |           |         |              |
    |  Method      |           |         |              |
    |              |           |         |              |
    |              |--Create-->|         |              |
    |              |  Payment  |         |              |
    |              |  Record   |         |              |
    |              |           |         |              |
    |              |<--Created-|         |              |
    |              |           |         |              |
    |              |--Process------------->|            |
    |              |  Payment  |         |              |
    |              |           |         |              |
    |              |           |         |--Charge----->|
    |              |           |         |              |
    |              |           |         |              |
    |              |           |         |<--Result-----|
    |              |           |         |  (Success/   |
    |              |           |         |   Failure)   |
    |              |           |         |              |
    |              |<--Status--------------|            |
    |              |           |         |              |
    |              |--Update-->|         |              |
    |              |  Payment  |         |              |
    |              |  Status   |         |              |
    |              |           |         |              |
    |              |<--Updated-|         |              |
    |              |           |         |              |
    |              |--Update-->|         |              |
    |              |  Request  |         |              |
    |              |  Payment  |         |              |
    |              |  Status   |         |              |
    |              |           |         |              |
    |              |<--Updated-|         |              |
    |              |           |         |              |
    |<--Success----|           |         |              |
    |  + Receipt   |           |         |              |
    |              |           |         |              |
    |              |--Schedule----------->|              |
    |              |  Payout to|         |              |
    |              |  Technician         |              |
    |              |           |         |              |
    |              |<--Scheduled---------|              |
    |              |           |         |              |
```

### Payment Failure Handling

```
Customer App   Backend API   MongoDB   Payment     
                                       Service     
    |              |           |         |         
    |              |--Process------------->|       
    |              |  Payment  |         |         
    |              |           |         |         
    |              |           |         |--Fail-->
    |              |           |         |  (Insuff.
    |              |           |         |   Funds) 
    |              |           |         |         
    |              |<--Failed-------------|         
    |              |  + Reason |         |         
    |              |           |         |         
    |              |--Update-->|         |         
    |              |  Payment: |         |         
    |              |  status=  |         |         
    |              |  failed   |         |         
    |              |           |         |         
    |              |<--Updated-|         |         
    |              |           |         |         
    |              |--Log----->|         |         
    |              |  Failure  |         |         
    |              |           |         |         
    |<--Error------|           |         |         
    |  + Retry     |           |         |         
    |  Option      |           |         |         
    |              |           |         |         
    |--Retry------>|           |         |         
    |  (Different  |           |         |         
    |   Method)    |           |         |         
    |              |           |         |         
```

---

## 6. Review Submission Sequence

```
Customer App    Backend API    MongoDB    WebSocket    Technician App
                                          Server
    |               |             |           |             |
    |--Submit------>|             |           |             |
    |  Review       |             |           |             |
    |               |             |           |             |
    |               |--Validate-->|           |             |
    |               |  Review     |           |             |
    |               |  Data       |           |             |
    |               |             |           |             |
    |               |--Verify---->|           |             |
    |               |  Service    |           |             |
    |               |  Completed  |           |             |
    |               |             |           |             |
    |               |<--Valid-----|           |             |
    |               |             |           |             |
    |               |--Save------>|           |             |
    |               |  Review     |           |             |
    |               |             |           |             |
    |               |<--Saved-----|           |             |
    |               |             |           |             |
    |               |--Calculate->|           |             |
    |               |  New Avg    |           |             |
    |               |  Rating     |           |             |
    |               |             |           |             |
    |               |--Update---->|           |             |
    |               |  Technician |           |             |
    |               |  Profile:   |           |             |
    |               |  -rating    |           |             |
    |               |  -count     |           |             |
    |               |             |           |             |
    |               |<--Updated---|           |             |
    |               |             |           |             |
    |<--Success-----|             |           |             |
    |               |             |           |             |
    |               |--Send Push------------->|             |
    |               |  Notification           |             |
    |               |             |           |             |
    |               |             |           |--Notify---->|
    |               |             |           |  "New       |
    |               |             |           |  Review"    |
    |               |             |           |             |
    |               |<--Delivered-|           |             |
    |               |             |           |             |
```

---

## 7. Admin Dashboard Sequence

### View Dashboard Statistics

```
Admin Web      Backend API    MongoDB       Cache
                                            (Redis)
    |               |             |             |
    |--Request----->|             |             |
    |  Dashboard    |             |             |
    |  Data         |             |             |
    |               |             |             |
    |               |--Check------|------------>|
    |               |  Cache      |             |
    |               |             |             |
    |               |<--Cache--------------------|
    |               |  Miss       |             |
    |               |             |             |
    |               |--Aggregate->|             |
    |               |  Queries:   |             |
    |               |  -Active    |             |
    |               |   Requests  |             |
    |               |  -Total     |             |
    |               |   Users     |             |
    |               |  -Revenue   |             |
    |               |  -Ratings   |             |
    |               |             |             |
    |               |<--Results---|             |
    |               |             |             |
    |               |--Store------|------------>|
    |               |  in Cache   |             |
    |               |  (5 min TTL)|             |
    |               |             |             |
    |<--Dashboard---|             |             |
    |  Data         |             |             |
    |               |             |             |
```

### Manage Service Request

```
Admin Web      Backend API    MongoDB    WebSocket    Customer App
                                         Server
    |               |             |           |             |
    |--Update------>|             |           |             |
    |  Request      |             |           |             |
    |  Status       |             |           |             |
    |               |             |           |             |
    |               |--Validate-->|           |             |
    |               |  Admin      |           |             |
    |               |  Permission |           |             |
    |               |             |           |             |
    |               |<--Valid-----|           |             |
    |               |             |           |             |
    |               |--Update---->|           |             |
    |               |  Request    |           |             |
    |               |             |           |             |
    |               |<--Updated---|           |             |
    |               |             |           |             |
    |               |--Log Action>|           |             |
    |               |  (Audit     |           |             |
    |               |   Trail)    |           |             |
    |               |             |           |             |
    |               |<--Logged----|           |             |
    |               |             |           |             |
    |<--Success-----|             |           |             |
    |               |             |           |             |
    |               |--Broadcast------------->|             |
    |               |  Update     |           |             |
    |               |             |           |             |
    |               |             |           |--Notify---->|
    |               |             |           |  Status     |
    |               |             |           |  Change     |
    |               |             |           |             |
```

### Approve Technician

```
Admin Web      Backend API    MongoDB    Email        Technician App
                                         Service
    |               |             |           |             |
    |--Approve----->|             |           |             |
    |  Technician   |             |           |             |
    |               |             |           |             |
    |               |--Validate-->|           |             |
    |               |  Documents  |           |             |
    |               |             |           |             |
    |               |<--Valid-----|           |             |
    |               |             |           |             |
    |               |--Update---->|           |             |
    |               |  User:      |           |             |
    |               |  status=    |           |             |
    |               |  active     |           |             |
    |               |             |           |             |
    |               |<--Updated---|           |             |
    |               |             |           |             |
    |               |--Update---->|           |             |
    |               |  Technician |           |             |
    |               |  Profile:   |           |             |
    |               |  verified=  |           |             |
    |               |  true       |           |             |
    |               |             |           |             |
    |               |<--Updated---|           |             |
    |               |             |           |             |
    |<--Success-----|             |           |             |
    |               |             |           |             |
    |               |--Send-------|---------->|             |
    |               |  Welcome    |           |             |
    |               |  Email      |           |             |
    |               |             |           |             |
    |               |<--Sent------|           |             |
    |               |             |           |             |
    |               |--Send Push------------->|             |
    |               |  Notification           |             |
    |               |             |           |             |
    |               |             |           |--Notify---->|
    |               |             |           |  "Account   |
    |               |             |           |  Approved"  |
    |               |             |           |             |
```

---

## Error Handling Sequences

### Network Error Recovery

```
Customer App    Backend API    Local DB
                              (SQLite)
    |               |             |
    |--Request----->|             |
    |  (No Network) X             |
    |               |             |
    |--Detect------>|             |
    |  Error        |             |
    |               |             |
    |--Queue--------|------------>|
    |  Request      |             |
    |  Locally      |             |
    |               |             |
    |<--Queued------|-------------|
    |               |             |
    |               |             |
    [Network restored]            |
    |               |             |
    |--Sync-------->|             |
    |  Queued       |             |
    |  Requests     |             |
    |               |             |
    |--Get Queued---|------------>|
    |  Items        |             |
    |               |             |
    |<--Items-------|-------------|
    |               |             |
    |--Send-------->|             |
    |  Requests     |             |
    |               |             |
    |<--Success-----|             |
    |               |             |
    |--Clear--------|------------>|
    |  Queue        |             |
    |               |             |
```

### Concurrent Request Handling

```
Request A      Request B      Backend API    MongoDB
    |               |               |            |
    |--Update------>|               |            |
    |  Status       |--Update------>|            |
    |               |  Status       |            |
    |               |               |            |
    |               |               |--Lock----->|
    |               |               |  Document  |
    |               |               |            |
    |               |               |<--Locked---|
    |               |               |            |
    |               |               |--Update--->|
    |               |               |  (A)       |
    |               |               |            |
    |               |               |<--Done-----|
    |               |               |            |
    |               |               |--Unlock--->|
    |               |               |            |
    |<--Success-----|               |            |
    |               |               |            |
    |               |               |--Lock----->|
    |               |               |  Document  |
    |               |               |            |
    |               |               |<--Locked---|
    |               |               |            |
    |               |               |--Update--->|
    |               |               |  (B)       |
    |               |               |            |
    |               |               |<--Done-----|
    |               |               |            |
    |               |<--Success-----|            |
    |               |               |            |
```

---

## Performance Optimization Sequences

### Caching Strategy

```
Mobile App     Backend API    Cache         MongoDB
                              (Redis)
    |               |             |             |
    |--Request----->|             |             |
    |  Categories   |             |             |
    |               |             |             |
    |               |--Check----->|             |
    |               |  Cache      |             |
    |               |             |             |
    |               |<--Hit-------|             |
    |               |  (Data)     |             |
    |               |             |             |
    |<--Categories--|             |             |
    |  (Fast)       |             |             |
    |               |             |             |
    |               |             |             |
[Cache Expires]    |             |             |
    |               |             |             |
    |--Request----->|             |             |
    |  Categories   |             |             |
    |               |             |             |
    |               |--Check----->|             |
    |               |  Cache      |             |
    |               |             |             |
    |               |<--Miss------|             |
    |               |             |             |
    |               |--Query------|------------>|
    |               |             |             |
    |               |<--Data------|-------------|
    |               |             |             |
    |               |--Store----->|             |
    |               |  in Cache   |             |
    |               |             |             |
    |<--Categories--|             |             |
    |               |             |             |
```

---

## Notes

1. **Authentication**: ใช้ Firebase Authentication สำหรับ authentication ทั้งหมด
2. **Real-time**: ใช้ WebSocket (Socket.io) สำหรับการอัพเดทแบบ real-time
3. **Geospatial**: ใช้ MongoDB geospatial queries สำหรับหาช่างใกล้เคียง
4. **Payment**: รองรับ payment gateway หลายตัว (Stripe, Omise, etc.)
5. **Caching**: ใช้ Redis สำหรับ cache ข้อมูลที่เข้าถึงบ่อย
6. **Queue**: ใช้ message queue (e.g., RabbitMQ) สำหรับ async tasks
7. **Error Handling**: มี retry mechanism และ offline support
8. **Security**: ใช้ JWT tokens และ role-based access control

