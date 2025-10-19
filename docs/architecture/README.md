# System Architecture - FixFast

คู่มือสถาปัตยกรรมระบบ FixFast

## สารบัญ

1. [Overview](#overview)
2. [Architecture Layers](#architecture-layers)
3. [Technology Stack](#technology-stack)
4. [Component Design](#component-design)
5. [Data Flow](#data-flow)
6. [Security Architecture](#security-architecture)
7. [Scalability & Performance](#scalability--performance)
8. [Deployment Architecture](#deployment-architecture)

---

## Overview

ระบบ FixFast ใช้สถาปัตยกรรมแบบ **Microservices-ready Monolith** โดยเริ่มต้นด้วย monolithic architecture ที่ออกแบบให้แยกเป็น microservices ได้ในอนาคต

### Key Characteristics

- **Client-Server Architecture**: แยก frontend และ backend อย่างชัดเจน
- **RESTful API**: ใช้ REST API เป็นหลักในการสื่อสาร
- **Real-time Communication**: ใช้ WebSocket สำหรับ real-time updates
- **Event-Driven**: ใช้ event-driven pattern สำหรับ async operations
- **Mobile-First**: ออกแบบให้ทำงานบน mobile เป็นหลัก
- **Cloud-Native**: พร้อมสำหรับ deployment บน cloud

---

## Architecture Layers

### 1. Presentation Layer (Client)

```
┌────────────────────────────────────────────────────┐
│            Presentation Layer                       │
├────────────────┬──────────────┬────────────────────┤
│  Customer App  │ Technical App│   Admin Web        │
│   (Flutter)    │  (Flutter)   │   (Next.js)        │
├────────────────┴──────────────┴────────────────────┤
│  - UI Components                                    │
│  - State Management (Provider/Riverpod/Redux)      │
│  - Local Storage (SQLite/Hive)                     │
│  - API Client                                       │
│  - WebSocket Client                                 │
└────────────────────────────────────────────────────┘
```

#### Customer App (Flutter)
- **Purpose**: ให้ลูกค้าสร้างและติดตามคำขอบริการ
- **Key Features**: 
  - Service request creation
  - Real-time tracking
  - In-app chat
  - Payment integration
  - Review system

#### Technical App (Flutter)
- **Purpose**: ให้ช่างรับงานและจัดการงาน
- **Key Features**:
  - Job notifications
  - Job acceptance/rejection
  - Navigation to customer location
  - Status updates
  - Earnings tracking

#### Admin Web (Next.js)
- **Purpose**: ให้ admin จัดการระบบทั้งหมด
- **Key Features**:
  - Dashboard & analytics
  - User management
  - Request monitoring
  - Payment management
  - Reports generation

### 2. API Gateway Layer

```
┌────────────────────────────────────────────────────┐
│              API Gateway Layer                      │
├────────────────────────────────────────────────────┤
│  - Request Routing                                  │
│  - Authentication/Authorization                     │
│  - Rate Limiting                                    │
│  - Request/Response Transformation                  │
│  - API Versioning                                   │
│  - Load Balancing                                   │
└────────────────────────────────────────────────────┘
```

### 3. Application Layer (Backend)

```
┌────────────────────────────────────────────────────┐
│           Application Layer (Backend)               │
├────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────────────┐  ┌──────────────┐               │
│  │   REST API   │  │  WebSocket   │               │
│  │   Server     │  │   Server     │               │
│  └──────┬───────┘  └──────┬───────┘               │
│         │                  │                       │
│  ┌──────┴──────────────────┴───────┐              │
│  │     Business Logic Layer         │              │
│  │  - Authentication Service        │              │
│  │  - User Service                  │              │
│  │  - Request Service               │              │
│  │  - Payment Service               │              │
│  │  - Notification Service          │              │
│  │  - Chat Service                  │              │
│  │  - Geolocation Service           │              │
│  └──────────────┬───────────────────┘              │
│                 │                                   │
│  ┌──────────────┴───────────────────┐              │
│  │      Data Access Layer           │              │
│  │  - Repositories                  │              │
│  │  - Data Models                   │              │
│  │  - Query Builders                │              │
│  └──────────────────────────────────┘              │
│                                                     │
└────────────────────────────────────────────────────┘
```

### 4. Data Layer

```
┌────────────────────────────────────────────────────┐
│                 Data Layer                          │
├──────────────┬──────────────┬──────────────────────┤
│   MongoDB    │    Redis     │   File Storage       │
│  (Primary)   │   (Cache)    │  (Firebase/S3)       │
├──────────────┴──────────────┴──────────────────────┤
│  - Users                                            │
│  - Service Requests                                 │
│  - Payments                                         │
│  - Reviews                                          │
│  - Chats                                            │
│  - Notifications                                    │
└────────────────────────────────────────────────────┘
```

### 5. External Services Layer

```
┌────────────────────────────────────────────────────┐
│            External Services                        │
├────────────┬──────────────┬──────────────┬─────────┤
│  Firebase  │  Payment     │  Google Maps │  Email  │
│  Auth/FCM  │  Gateway     │     API      │ Service │
└────────────┴──────────────┴──────────────┴─────────┘
```

---

## Technology Stack

### Frontend Technologies

#### Mobile Apps (Flutter)
```yaml
Framework: Flutter 3.x
Language: Dart 3.x

Key Packages:
  - provider/riverpod: State management
  - dio: HTTP client
  - socket_io_client: WebSocket
  - google_maps_flutter: Maps
  - firebase_auth: Authentication
  - firebase_messaging: Push notifications
  - image_picker: Camera/gallery access
  - cached_network_image: Image caching
  - hive/sqflite: Local storage
```

#### Admin Web (Next.js)
```yaml
Framework: Next.js 14.x
Language: TypeScript 5.x

Key Libraries:
  - React 18.x
  - TailwindCSS: Styling
  - Socket.io-client: Real-time
  - Recharts/Chart.js: Data visualization
  - React Query: Data fetching
  - Zustand/Redux: State management
  - NextAuth.js: Authentication
```

### Backend Technologies

```yaml
Runtime: Node.js 18+ LTS
Framework: Express.js 4.x
Language: TypeScript 5.x

Key Libraries:
  - mongoose: MongoDB ODM
  - socket.io: WebSocket
  - jsonwebtoken: JWT authentication
  - bcrypt: Password hashing
  - express-validator: Input validation
  - multer: File uploads
  - node-cron: Scheduled tasks
  - winston: Logging
  - ioredis: Redis client
```

### Database

```yaml
Primary Database: MongoDB 6.0+
  - Document-based NoSQL
  - Geospatial queries support
  - Flexible schema

Cache: Redis 7.0+
  - Session storage
  - Real-time data cache
  - Rate limiting

File Storage:
  - Firebase Storage / AWS S3
  - Image and document storage
```

---

## Component Design

### 1. Authentication Service

```typescript
interface AuthService {
  // User registration
  register(data: RegisterDto): Promise<AuthResponse>
  
  // User login
  login(credentials: LoginDto): Promise<AuthResponse>
  
  // Firebase token verification
  verifyFirebaseToken(token: string): Promise<AuthResponse>
  
  // JWT token generation
  generateToken(user: User): string
  
  // Token verification
  verifyToken(token: string): Promise<User>
  
  // Refresh token
  refreshToken(refreshToken: string): Promise<AuthResponse>
}
```

### 2. Service Request Service

```typescript
interface ServiceRequestService {
  // Create new request
  create(data: CreateRequestDto): Promise<ServiceRequest>
  
  // Get request by ID
  findById(id: string): Promise<ServiceRequest>
  
  // List requests with filters
  list(filters: RequestFilters): Promise<PaginatedResult<ServiceRequest>>
  
  // Update request status
  updateStatus(id: string, status: RequestStatus): Promise<ServiceRequest>
  
  // Assign technician
  assignTechnician(requestId: string, technicianId: string): Promise<ServiceRequest>
  
  // Cancel request
  cancel(id: string, reason: string): Promise<ServiceRequest>
  
  // Find nearby technicians
  findNearbyTechnicians(location: Location, categoryId: string): Promise<User[]>
}
```

### 3. Payment Service

```typescript
interface PaymentService {
  // Create payment intent
  createIntent(requestId: string): Promise<PaymentIntent>
  
  // Process payment
  processPayment(paymentId: string, method: PaymentMethod): Promise<Payment>
  
  // Confirm payment
  confirmPayment(paymentId: string): Promise<Payment>
  
  // Refund payment
  refund(paymentId: string, amount: number): Promise<Payment>
  
  // Schedule payout to technician
  schedulePayout(technicianId: string, amount: number): Promise<Payout>
}
```

### 4. Notification Service

```typescript
interface NotificationService {
  // Send push notification
  sendPush(userId: string, notification: PushNotification): Promise<void>
  
  // Send email notification
  sendEmail(userId: string, email: EmailNotification): Promise<void>
  
  // Create in-app notification
  create(notification: CreateNotificationDto): Promise<Notification>
  
  // Mark as read
  markAsRead(notificationId: string): Promise<void>
  
  // Get user notifications
  getUserNotifications(userId: string, filters: NotificationFilters): Promise<Notification[]>
}
```

### 5. Geolocation Service

```typescript
interface GeolocationService {
  // Calculate distance between two points
  calculateDistance(point1: Location, point2: Location): number
  
  // Find nearby users
  findNearby(center: Location, radius: number, filters?: any): Promise<User[]>
  
  // Get address from coordinates
  reverseGeocode(location: Location): Promise<Address>
  
  // Get coordinates from address
  geocode(address: string): Promise<Location>
}
```

---

## Data Flow

### 1. Service Request Creation Flow

```
Customer App → API Gateway → Backend API
                ↓
        Validate Request
                ↓
        Save to MongoDB
                ↓
     ┌──────────┴──────────┐
     ↓                     ↓
Find Nearby          Create Notification
Technicians               Record
     ↓                     ↓
Send Push            Save to MongoDB
Notifications
     ↓
Return Response
     ↓
Customer App (Updated)
```

### 2. Real-time Update Flow

```
Event Trigger (Status Change)
        ↓
Backend Updates Database
        ↓
Emit WebSocket Event
        ↓
    ┌───┴───┐
    ↓       ↓
Customer  Technician
  App       App
    ↓       ↓
Update UI in Real-time
```

### 3. Payment Processing Flow

```
Customer Initiates Payment
        ↓
Backend Creates Payment Intent
        ↓
Payment Gateway Processes
        ↓
    ┌───┴────┐
    ↓        ↓
Success   Failure
    ↓        ↓
Update DB  Retry/Cancel
    ↓
Send Receipt
    ↓
Schedule Payout to Technician
```

---

## Security Architecture

### 1. Authentication & Authorization

```
┌─────────────────────────────────────────┐
│         Authentication Flow              │
├─────────────────────────────────────────┤
│  1. User Login with Credentials         │
│     ↓                                    │
│  2. Firebase Auth Verification          │
│     ↓                                    │
│  3. Backend Generates JWT Token         │
│     ↓                                    │
│  4. Client Stores Token                 │
│     ↓                                    │
│  5. Include Token in All Requests       │
│     ↓                                    │
│  6. Backend Validates Token             │
│     ↓                                    │
│  7. Check User Permissions (RBAC)       │
│     ↓                                    │
│  8. Process Request                     │
└─────────────────────────────────────────┘
```

### 2. Role-Based Access Control (RBAC)

```typescript
enum Role {
  CUSTOMER = 'customer',
  TECHNICAL = 'technical',
  ADMIN = 'admin'
}

const permissions = {
  customer: [
    'request:create',
    'request:view:own',
    'request:cancel:own',
    'payment:create',
    'review:create'
  ],
  technical: [
    'request:view:available',
    'request:accept',
    'request:update:assigned',
    'earnings:view:own'
  ],
  admin: [
    'user:*',
    'request:*',
    'payment:*',
    'category:*',
    'report:view'
  ]
}
```

### 3. Data Security

- **Encryption at Rest**: MongoDB encryption
- **Encryption in Transit**: TLS/SSL for all connections
- **Password Security**: bcrypt hashing with salt
- **API Security**: Rate limiting, input validation
- **PII Protection**: Mask sensitive data in logs
- **SQL Injection Prevention**: Use parameterized queries
- **XSS Prevention**: Input sanitization

### 4. Security Best Practices

```typescript
// Input Validation
import { body, validationResult } from 'express-validator'

router.post('/requests', [
  body('categoryId').isMongoId(),
  body('details.title').trim().notEmpty().escape(),
  body('details.description').trim().notEmpty().escape(),
  body('location.coordinates').isArray({ min: 2, max: 2 }),
  // ... more validations
], async (req, res) => {
  const errors = validationResult(req)
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() })
  }
  // Process request
})

// Rate Limiting
import rateLimit from 'express-rate-limit'

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
})

app.use('/api/', limiter)

// CORS Configuration
app.use(cors({
  origin: process.env.ALLOWED_ORIGINS.split(','),
  credentials: true
}))
```

---

## Scalability & Performance

### 1. Horizontal Scaling

```
                Load Balancer
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
    API Server 1  API Server 2  API Server 3
        ↓             ↓             ↓
        └─────────────┼─────────────┘
                      ↓
                  Database
```

### 2. Caching Strategy

```typescript
// Cache Layers
1. Client-Side Cache (Local Storage)
   - User profile
   - Categories
   - Recent requests

2. CDN Cache
   - Static assets
   - Images

3. Application Cache (Redis)
   - Session data
   - Frequently accessed data
   - Real-time data

4. Database Query Cache
   - MongoDB query cache
```

### 3. Database Optimization

```javascript
// Indexes for Performance
db.users.createIndex({ email: 1 }, { unique: true })
db.users.createIndex({ "address.location": "2dsphere" })
db.service_requests.createIndex({ status: 1, createdAt: -1 })
db.service_requests.createIndex({ technicianId: 1, status: 1 })
db.service_requests.createIndex({ "location.coordinates": "2dsphere" })

// Query Optimization
// Use projection to limit fields
db.users.find(
  { role: 'technical' },
  { profile: 1, rating: 1 }
)

// Use indexes for sorting
db.service_requests.find()
  .sort({ createdAt: -1 })
  .limit(20)
```

### 4. Async Processing

```typescript
// Use message queues for heavy tasks
import Bull from 'bull'

const emailQueue = new Bull('email', {
  redis: {
    host: process.env.REDIS_HOST,
    port: process.env.REDIS_PORT
  }
})

// Add job to queue
emailQueue.add({
  to: user.email,
  template: 'welcome',
  data: { name: user.profile.firstName }
})

// Process jobs
emailQueue.process(async (job) => {
  await sendEmail(job.data)
})
```

---

## Deployment Architecture

### Production Environment

```
┌─────────────────────────────────────────────────────┐
│                  Cloud Provider (AWS/GCP/Azure)     │
├─────────────────────────────────────────────────────┤
│                                                      │
│  ┌─────────────────────────────────────────────┐   │
│  │            Load Balancer (ALB/NLB)          │   │
│  └────────────────────┬────────────────────────┘   │
│                       │                             │
│         ┌─────────────┴─────────────┐               │
│         ↓                           ↓               │
│  ┌──────────────┐            ┌──────────────┐      │
│  │  App Server  │            │  App Server  │      │
│  │  Container   │            │  Container   │      │
│  │  (Docker)    │            │  (Docker)    │      │
│  └──────────────┘            └──────────────┘      │
│         │                           │               │
│         └─────────────┬─────────────┘               │
│                       ↓                             │
│              ┌─────────────────┐                    │
│              │   MongoDB Atlas │                    │
│              │   (Managed)     │                    │
│              └─────────────────┘                    │
│                       ↓                             │
│              ┌─────────────────┐                    │
│              │  Redis Cloud    │                    │
│              │  (Managed)      │                    │
│              └─────────────────┘                    │
│                                                      │
│  ┌──────────────────────────────────────────────┐  │
│  │        External Services                     │  │
│  │  - Firebase (Auth, FCM, Storage)             │  │
│  │  - Payment Gateway (Stripe/Omise)            │  │
│  │  - Google Maps API                           │  │
│  │  - SendGrid/AWS SES (Email)                  │  │
│  └──────────────────────────────────────────────┘  │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### Container Architecture

```dockerfile
# Backend Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

### Kubernetes Deployment (Optional)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fixfast-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: fixfast-api
  template:
    metadata:
      labels:
        app: fixfast-api
    spec:
      containers:
      - name: api
        image: fixfast/api:latest
        ports:
        - containerPort: 3000
        env:
        - name: MONGODB_URI
          valueFrom:
            secretKeyRef:
              name: fixfast-secrets
              key: mongodb-uri
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

### CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
      - run: npm ci
      - run: npm test
      
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: docker/build-push-action@v2
        with:
          push: true
          tags: fixfast/api:latest
          
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Cloud
        run: |
          # Deploy commands here
```

---

## Monitoring & Logging

### Application Monitoring

```typescript
// Winston Logger
import winston from 'winston'

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
})

// Metrics Collection
import prometheus from 'prom-client'

const httpRequestDuration = new prometheus.Histogram({
  name: 'http_request_duration_ms',
  help: 'Duration of HTTP requests in ms',
  labelNames: ['method', 'route', 'status_code']
})
```

### Health Checks

```typescript
app.get('/health', async (req, res) => {
  const health = {
    uptime: process.uptime(),
    timestamp: Date.now(),
    status: 'OK',
    checks: {
      database: await checkDatabaseConnection(),
      redis: await checkRedisConnection(),
      firebase: await checkFirebaseConnection()
    }
  }
  
  const statusCode = Object.values(health.checks).every(v => v === true) ? 200 : 503
  res.status(statusCode).json(health)
})
```

---

## Disaster Recovery

### Backup Strategy

1. **Database Backups**
   - Automated daily backups
   - Point-in-time recovery
   - Geographic replication

2. **File Storage Backups**
   - Automatic versioning
   - Cross-region replication

3. **Configuration Backups**
   - Version-controlled configs
   - Secret management (Vault/AWS Secrets Manager)

### Recovery Plan

1. **Database Recovery**: Restore from latest backup
2. **Service Recovery**: Deploy from last known good image
3. **Data Validation**: Verify data integrity post-recovery
4. **Gradual Rollout**: Phased recovery to production

