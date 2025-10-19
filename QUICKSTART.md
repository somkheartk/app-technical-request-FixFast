# Quick Start Guide - FixFast

คู่มือเริ่มต้นใช้งานระบบ FixFast

## ภาพรวม

ระบบ FixFast ประกอบด้วย 4 ส่วนหลัก:

1. **Backend API** - Node.js/Express server
2. **Customer App** - Flutter mobile app สำหรับลูกค้า
3. **Technical App** - Flutter mobile app สำหรับช่าง
4. **Admin Web** - Next.js web dashboard สำหรับ admin

## ข้อกำหนดเบื้องต้น

### สำหรับ Backend
- Node.js 18+ ([ดาวน์โหลด](https://nodejs.org/))
- MongoDB 6.0+ ([ดาวน์โหลด](https://www.mongodb.com/try/download/community))
- Redis 7.0+ ([ดาวน์โหลด](https://redis.io/download))

### สำหรับ Mobile Apps
- Flutter 3.0+ ([ติดตั้ง](https://docs.flutter.dev/get-started/install))
- Android Studio หรือ Xcode
- Android SDK / iOS SDK

### สำหรับ Admin Web
- Node.js 18+
- npm หรือ yarn

### บัญชีที่ต้องการ
- Firebase Account ([สมัคร](https://firebase.google.com/))
- Google Maps API Key ([สมัคร](https://developers.google.com/maps))
- Stripe Account (optional, [สมัคร](https://stripe.com/))

## การติดตั้งแบบ Step-by-Step

### Step 1: Setup Backend

```bash
# 1. Clone repository
git clone https://github.com/somkheartk/app-technical-request-FixFast.git
cd app-technical-request-FixFast/backend

# 2. Install dependencies
npm install

# 3. Create environment file
cp .env.example .env

# 4. Edit .env file และใส่ค่าที่จำเป็น
nano .env
```

**ค่าที่สำคัญใน .env:**
```env
# Database
MONGODB_URI=mongodb://localhost:27017/fixfast
REDIS_URL=redis://localhost:6379

# JWT
JWT_SECRET=your-secret-key-here

# Firebase
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=your-email@project.iam.gserviceaccount.com

# Google Maps
GOOGLE_MAPS_API_KEY=your-api-key
```

```bash
# 5. Start MongoDB และ Redis
# macOS with Homebrew:
brew services start mongodb-community
brew services start redis

# Linux:
sudo systemctl start mongod
sudo systemctl start redis

# 6. Seed database (optional)
npm run seed

# 7. Start server
npm run dev
```

เซิร์ฟเวอร์จะทำงานที่ `http://localhost:3000`

### Step 2: Setup Customer App

```bash
# 1. Navigate to customer app
cd ../customer-app

# 2. Install dependencies
flutter pub get

# 3. Configure Firebase
# ติดตั้ง FlutterFire CLI
dart pub global activate flutterfire_cli

# Configure Firebase สำหรับโปรเจค
flutterfire configure

# 4. Create config file
# สร้างไฟล์ lib/core/config/env_config.dart
```

**env_config.dart:**
```dart
class EnvConfig {
  static const String apiBaseUrl = 'http://localhost:3000/api/v1';
  static const String socketUrl = 'http://localhost:3000';
  static const String googleMapsApiKey = 'YOUR_GOOGLE_MAPS_KEY';
}
```

```bash
# 5. Run the app
# สำหรับ Android
flutter run

# สำหรับ iOS
flutter run -d ios

# สำหรับ Android Emulator เชื่อมต่อ localhost
# ใช้ 10.0.2.2 แทน localhost ใน env_config.dart
```

### Step 3: Setup Technical App

```bash
# 1. Navigate to technical app
cd ../technical-app

# 2. Install dependencies
flutter pub get

# 3. Configure Firebase
flutterfire configure

# 4. Create config file (เหมือน Customer App)
# lib/core/config/env_config.dart

# 5. Run the app
flutter run
```

### Step 4: Setup Admin Web

```bash
# 1. Navigate to admin web
cd ../admin-web

# 2. Install dependencies
npm install

# 3. Create environment file
cp .env.example .env.local

# 4. Edit .env.local
nano .env.local
```

**ค่าที่สำคัญใน .env.local:**
```env
NEXT_PUBLIC_API_URL=http://localhost:3000/api/v1
NEXT_PUBLIC_SOCKET_URL=http://localhost:3000

NEXTAUTH_SECRET=your-nextauth-secret
NEXTAUTH_URL=http://localhost:3001
```

```bash
# 5. Run development server
npm run dev
```

เว็บจะทำงานที่ `http://localhost:3001`

## การทดสอบระบบ

### 1. ทดสอบ Backend API

```bash
# Health check
curl http://localhost:3000/health

# ควรได้ response:
# {"status":"OK","timestamp":"..."}
```

### 2. สร้างผู้ใช้งานทดสอบ

```bash
# สร้าง Admin user
curl -X POST http://localhost:3000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@fixfast.com",
    "password": "admin123",
    "role": "admin",
    "profile": {
      "firstName": "Admin",
      "lastName": "User"
    }
  }'

# สร้าง Customer user
curl -X POST http://localhost:3000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "customer@test.com",
    "password": "test123",
    "role": "customer",
    "profile": {
      "firstName": "Test",
      "lastName": "Customer"
    }
  }'

# สร้าง Technician user
curl -X POST http://localhost:3000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "tech@test.com",
    "password": "test123",
    "role": "technical",
    "profile": {
      "firstName": "Test",
      "lastName": "Technician"
    }
  }'
```

### 3. Login และทดสอบ Apps

**Customer App:**
1. เปิดแอพ
2. Login ด้วย `customer@test.com` / `test123`
3. ลองสร้าง service request

**Technical App:**
1. เปิดแอพ
2. Login ด้วย `tech@test.com` / `test123`
3. กรอกข้อมูลเพิ่มเติม (specialization, documents)
4. รอ admin approve

**Admin Web:**
1. เปิดเว็บ `http://localhost:3001`
2. Login ด้วย `admin@fixfast.com` / `admin123`
3. Approve technician
4. ดู dashboard

## โครงสร้างการทำงาน

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Customer   │     │ Technical   │     │   Admin     │
│     App     │     │     App     │     │    Web      │
└──────┬──────┘     └──────┬──────┘     └──────┬──────┘
       │                   │                    │
       │                   │                    │
       └───────────────────┼────────────────────┘
                           │
                           ▼
                   ┌──────────────┐
                   │  Backend API │
                   │   (Port      │
                   │    3000)     │
                   └──────┬───────┘
                          │
              ┌───────────┼───────────┐
              │                       │
              ▼                       ▼
       ┌─────────────┐         ┌────────────┐
       │   MongoDB   │         │   Redis    │
       │   (27017)   │         │   (6379)   │
       └─────────────┘         └────────────┘
```

## การแก้ไขปัญหาทั่วไป

### Backend ไม่เริ่มทำงาน

**ปัญหา:** Cannot connect to MongoDB
```bash
# ตรวจสอบว่า MongoDB ทำงานหรือไม่
mongosh
# หรือ
mongo

# ถ้าไม่ทำงาน ให้เริ่มใหม่
# macOS:
brew services restart mongodb-community

# Linux:
sudo systemctl restart mongod
```

**ปัญหา:** Port 3000 already in use
```bash
# หาว่าใครใช้ port 3000
# macOS/Linux:
lsof -i :3000

# Kill process
kill -9 <PID>

# หรือเปลี่ยน port ใน .env
PORT=3001
```

### Flutter Apps ไม่สามารถเชื่อมต่อ Backend

**ปัญหา:** Connection refused (Android Emulator)

แก้ไข: ใน `env_config.dart` ใช้:
```dart
// Android Emulator
static const String apiBaseUrl = 'http://10.0.2.2:3000/api/v1';

// iOS Simulator
static const String apiBaseUrl = 'http://localhost:3000/api/v1';

// Real Device (ใช้ IP ของเครื่อง)
static const String apiBaseUrl = 'http://192.168.x.x:3000/api/v1';
```

**หา IP ของเครื่อง:**
```bash
# macOS/Linux
ifconfig | grep inet

# Windows
ipconfig
```

### Firebase Configuration Issues

```bash
# ตรวจสอบว่าติดตั้ง FlutterFire CLI แล้ว
dart pub global list

# ติดตั้งใหม่
dart pub global activate flutterfire_cli

# Configure ใหม่
flutterfire configure
```

### Admin Web ไม่สามารถ Login

**ตรวจสอบ:**
1. Backend ทำงานหรือไม่
2. CORS settings ใน backend
3. NextAuth configuration

**แก้ไข CORS:**
```typescript
// backend/src/app.ts
app.use(cors({
  origin: ['http://localhost:3001', 'http://localhost:3002'],
  credentials: true,
}))
```

## Development Workflow

### 1. เริ่มต้นทำงานแต่ละวัน

```bash
# Terminal 1: Backend
cd backend
npm run dev

# Terminal 2: Customer App
cd customer-app
flutter run

# Terminal 3: Technical App
cd technical-app
flutter run

# Terminal 4: Admin Web
cd admin-web
npm run dev
```

### 2. ตรวจสอบ Logs

**Backend:**
```bash
# ดู logs real-time
tail -f logs/combined.log

# ดู error logs
tail -f logs/error.log
```

**Flutter Apps:**
```bash
# Run with verbose output
flutter run -v

# View logs
flutter logs
```

**Admin Web:**
```bash
# Logs จะแสดงใน terminal ที่ run npm run dev
```

### 3. Testing

```bash
# Backend
cd backend
npm test

# Customer App
cd customer-app
flutter test

# Technical App
cd technical-app
flutter test

# Admin Web
cd admin-web
npm test
```

## การ Deploy

### Backend

```bash
# Build
npm run build

# Start production
NODE_ENV=production npm start

# หรือใช้ PM2
pm2 start dist/index.js --name fixfast-backend
```

### Mobile Apps

```bash
# Android
flutter build apk --release
flutter build appbundle --release

# iOS
flutter build ios --release
```

### Admin Web

```bash
# Build
npm run build

# Start production
npm start

# หรือ deploy to Vercel
vercel --prod
```

## Next Steps

1. **ปรับแต่ง UI/UX**: แก้ไข theme และ styles ตามความต้องการ
2. **เพิ่ม Features**: เพิ่มฟีเจอร์ตามที่ต้องการ
3. **Security**: เพิ่ม security measures เพิ่มเติม
4. **Testing**: เขียน tests ให้ครบถ้วน
5. **Documentation**: เขียนเอกสารเพิ่มเติม
6. **Deploy**: Deploy ไปยัง production environment

## ทรัพยากรเพิ่มเติม

- [Database Schema](./docs/database/README.md)
- [API Documentation](./docs/api/README.md)
- [System Flows](./docs/diagrams/flows.md)
- [Sequence Diagrams](./docs/diagrams/sequences.md)
- [Architecture Guide](./docs/architecture/README.md)

## การขอความช่วยเหลือ

หากมีปัญหาหรือคำถาม:

1. ตรวจสอบเอกสารใน `/docs`
2. ดู logs เพื่อหาสาเหตุของปัญหา
3. ตรวจสอบ GitHub Issues
4. ติดต่อทีมพัฒนา

## License

MIT License - ดูรายละเอียดใน [LICENSE](./LICENSE)

---

**หมายเหตุ:** นี่เป็นคู่มือเบื้องต้นสำหรับการพัฒนา (Development) สำหรับการ deploy ไปยัง production โปรดดูเอกสารเพิ่มเติมในแต่ละโปรเจค
