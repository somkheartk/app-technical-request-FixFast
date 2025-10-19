# Backend API Server - Node.js/Express

Backend API server สำหรับระบบ FixFast

## โครงสร้างโปรเจค

```
backend/
├── src/
│   ├── index.ts               # Entry point
│   ├── app.ts                 # Express app setup
│   ├── server.ts              # Server startup
│   │
│   ├── config/                # Configuration
│   │   ├── database.ts        # MongoDB connection
│   │   ├── redis.ts           # Redis connection
│   │   ├── firebase.ts        # Firebase admin
│   │   ├── socket.ts          # Socket.io setup
│   │   └── env.ts             # Environment variables
│   │
│   ├── controllers/           # Route controllers
│   │   ├── auth.controller.ts
│   │   ├── user.controller.ts
│   │   ├── request.controller.ts
│   │   ├── payment.controller.ts
│   │   ├── review.controller.ts
│   │   ├── category.controller.ts
│   │   ├── chat.controller.ts
│   │   ├── notification.controller.ts
│   │   └── admin.controller.ts
│   │
│   ├── routes/                # API routes
│   │   ├── index.ts           # Main router
│   │   ├── auth.routes.ts
│   │   ├── user.routes.ts
│   │   ├── request.routes.ts
│   │   ├── payment.routes.ts
│   │   ├── review.routes.ts
│   │   ├── category.routes.ts
│   │   ├── chat.routes.ts
│   │   ├── notification.routes.ts
│   │   └── admin.routes.ts
│   │
│   ├── models/                # MongoDB models
│   │   ├── User.model.ts
│   │   ├── TechnicianProfile.model.ts
│   │   ├── ServiceRequest.model.ts
│   │   ├── Category.model.ts
│   │   ├── Payment.model.ts
│   │   ├── Review.model.ts
│   │   ├── Chat.model.ts
│   │   └── Notification.model.ts
│   │
│   ├── services/              # Business logic
│   │   ├── auth.service.ts
│   │   ├── user.service.ts
│   │   ├── request.service.ts
│   │   ├── payment.service.ts
│   │   ├── review.service.ts
│   │   ├── category.service.ts
│   │   ├── chat.service.ts
│   │   ├── notification.service.ts
│   │   ├── email.service.ts
│   │   ├── sms.service.ts
│   │   ├── geolocation.service.ts
│   │   └── storage.service.ts
│   │
│   ├── middlewares/           # Express middlewares
│   │   ├── auth.middleware.ts
│   │   ├── validation.middleware.ts
│   │   ├── error.middleware.ts
│   │   ├── rate-limit.middleware.ts
│   │   ├── upload.middleware.ts
│   │   └── logging.middleware.ts
│   │
│   ├── validations/           # Request validation schemas
│   │   ├── auth.validation.ts
│   │   ├── user.validation.ts
│   │   ├── request.validation.ts
│   │   ├── payment.validation.ts
│   │   └── review.validation.ts
│   │
│   ├── utils/                 # Utility functions
│   │   ├── logger.ts
│   │   ├── response.ts
│   │   ├── error.ts
│   │   ├── jwt.ts
│   │   ├── hash.ts
│   │   └── helpers.ts
│   │
│   ├── types/                 # TypeScript types
│   │   ├── express.d.ts
│   │   ├── user.types.ts
│   │   ├── request.types.ts
│   │   └── common.types.ts
│   │
│   └── socket/                # WebSocket handlers
│       ├── index.ts
│       ├── chat.socket.ts
│       ├── request.socket.ts
│       └── location.socket.ts
│
├── tests/                     # Tests
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── scripts/                   # Utility scripts
│   ├── seed.ts               # Database seeding
│   └── migrate.ts            # Database migration
│
├── .env.example              # Example environment file
├── .eslintrc.js              # ESLint configuration
├── .prettierrc               # Prettier configuration
├── tsconfig.json             # TypeScript configuration
├── package.json              # Dependencies
└── README.md                 # This file
```

## Technology Stack

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "typescript": "^5.0.0",
    
    "mongoose": "^8.0.0",
    "ioredis": "^5.3.2",
    
    "socket.io": "^4.6.0",
    
    "firebase-admin": "^12.0.0",
    
    "jsonwebtoken": "^9.0.2",
    "bcryptjs": "^2.4.3",
    
    "express-validator": "^7.0.0",
    "joi": "^17.11.0",
    
    "multer": "^1.4.5-lts.1",
    "sharp": "^0.33.0",
    
    "nodemailer": "^6.9.0",
    "twilio": "^4.20.0",
    
    "stripe": "^14.0.0",
    
    "winston": "^3.11.0",
    "morgan": "^1.10.0",
    
    "express-rate-limit": "^7.1.0",
    "helmet": "^7.1.0",
    "cors": "^2.8.5",
    "compression": "^1.7.4",
    
    "node-cron": "^3.0.3",
    "bull": "^4.11.0",
    
    "dotenv": "^16.3.0"
  },
  "devDependencies": {
    "@types/express": "^4.17.0",
    "@types/node": "^20.0.0",
    "@types/bcryptjs": "^2.4.0",
    "@types/jsonwebtoken": "^9.0.0",
    "@types/multer": "^1.4.0",
    "@types/nodemailer": "^6.4.0",
    "@types/cors": "^2.8.0",
    "ts-node": "^10.9.0",
    "ts-node-dev": "^2.0.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0",
    "jest": "^29.7.0",
    "@types/jest": "^29.5.0",
    "supertest": "^6.3.0"
  }
}
```

## Setup Instructions

### Prerequisites
- Node.js 18+
- MongoDB 6.0+
- Redis 7.0+
- Firebase Account

### Installation

1. Clone repository:
```bash
git clone https://github.com/your-repo/fixfast-backend.git
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create environment file:
```bash
cp .env.example .env
```

4. Configure environment variables:
```env
# Server
NODE_ENV=development
PORT=3000
API_VERSION=v1

# Database
MONGODB_URI=mongodb://localhost:27017/fixfast
REDIS_URL=redis://localhost:6379

# JWT
JWT_SECRET=your-jwt-secret
JWT_EXPIRES_IN=7d
JWT_REFRESH_SECRET=your-refresh-secret
JWT_REFRESH_EXPIRES_IN=30d

# Firebase
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
FIREBASE_CLIENT_EMAIL=your-service-account@project.iam.gserviceaccount.com

# Google Maps
GOOGLE_MAPS_API_KEY=your-google-maps-key

# Payment
STRIPE_SECRET_KEY=your-stripe-secret-key
STRIPE_WEBHOOK_SECRET=your-webhook-secret

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-password

# SMS
TWILIO_ACCOUNT_SID=your-twilio-sid
TWILIO_AUTH_TOKEN=your-twilio-token
TWILIO_PHONE_NUMBER=+1234567890

# Storage
FIREBASE_STORAGE_BUCKET=your-bucket.appspot.com

# CORS
CORS_ORIGIN=http://localhost:3001,http://localhost:3002
```

5. Run the server:
```bash
# Development
npm run dev

# Production
npm run build
npm start
```

## Key Implementations

### 1. Express App Setup

```typescript
// src/app.ts
import express, { Application } from 'express'
import cors from 'cors'
import helmet from 'helmet'
import compression from 'compression'
import morgan from 'morgan'
import routes from './routes'
import { errorMiddleware } from './middlewares/error.middleware'
import { notFoundMiddleware } from './middlewares/notFound.middleware'

export function createApp(): Application {
  const app = express()

  // Security middlewares
  app.use(helmet())
  app.use(cors({
    origin: process.env.CORS_ORIGIN?.split(','),
    credentials: true,
  }))

  // Body parsing
  app.use(express.json())
  app.use(express.urlencoded({ extended: true }))

  // Compression
  app.use(compression())

  // Logging
  app.use(morgan('combined'))

  // Health check
  app.get('/health', (req, res) => {
    res.json({ status: 'OK', timestamp: new Date().toISOString() })
  })

  // API routes
  app.use(`/api/${process.env.API_VERSION}`, routes)

  // Error handling
  app.use(notFoundMiddleware)
  app.use(errorMiddleware)

  return app
}
```

### 2. Database Connection

```typescript
// src/config/database.ts
import mongoose from 'mongoose'
import { logger } from '../utils/logger'

export async function connectDatabase(): Promise<void> {
  try {
    await mongoose.connect(process.env.MONGODB_URI!, {
      maxPoolSize: 10,
      serverSelectionTimeoutMS: 5000,
    })

    logger.info('MongoDB connected successfully')

    // Handle connection events
    mongoose.connection.on('error', (error) => {
      logger.error('MongoDB connection error:', error)
    })

    mongoose.connection.on('disconnected', () => {
      logger.warn('MongoDB disconnected')
    })

  } catch (error) {
    logger.error('MongoDB connection failed:', error)
    process.exit(1)
  }
}
```

### 3. Authentication Middleware

```typescript
// src/middlewares/auth.middleware.ts
import { Request, Response, NextFunction } from 'express'
import jwt from 'jsonwebtoken'
import { User } from '../models/User.model'
import { AppError } from '../utils/error'

export interface AuthRequest extends Request {
  user?: any
}

export const authenticate = async (
  req: AuthRequest,
  res: Response,
  next: NextFunction
) => {
  try {
    // Get token from header
    const token = req.headers.authorization?.replace('Bearer ', '')

    if (!token) {
      throw new AppError('No token provided', 401)
    }

    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET!) as any

    // Get user
    const user = await User.findById(decoded.userId)

    if (!user || user.status !== 'active') {
      throw new AppError('User not found or inactive', 401)
    }

    // Attach user to request
    req.user = user

    next()
  } catch (error) {
    if (error instanceof jwt.JsonWebTokenError) {
      next(new AppError('Invalid token', 401))
    } else {
      next(error)
    }
  }
}

export const authorize = (...roles: string[]) => {
  return (req: AuthRequest, res: Response, next: NextFunction) => {
    if (!req.user) {
      return next(new AppError('Unauthorized', 401))
    }

    if (!roles.includes(req.user.role)) {
      return next(new AppError('Forbidden', 403))
    }

    next()
  }
}
```

### 4. Service Request Controller

```typescript
// src/controllers/request.controller.ts
import { Response, NextFunction } from 'express'
import { AuthRequest } from '../middlewares/auth.middleware'
import { RequestService } from '../services/request.service'
import { NotificationService } from '../services/notification.service'
import { successResponse } from '../utils/response'
import { AppError } from '../utils/error'

export class RequestController {
  private requestService: RequestService
  private notificationService: NotificationService

  constructor() {
    this.requestService = new RequestService()
    this.notificationService = new NotificationService()
  }

  createRequest = async (
    req: AuthRequest,
    res: Response,
    next: NextFunction
  ) => {
    try {
      const customerId = req.user._id
      const requestData = req.body

      // Create request
      const request = await this.requestService.createRequest({
        customerId,
        ...requestData,
      })

      // Find nearby technicians
      const technicians = await this.requestService.findNearbyTechnicians(
        request.location.coordinates,
        request.categoryId
      )

      // Send notifications to technicians
      await Promise.all(
        technicians.map(tech =>
          this.notificationService.sendPushNotification(
            tech._id,
            {
              title: 'New Job Available',
              body: `New ${request.category.name} request nearby`,
              data: { requestId: request._id },
            }
          )
        )
      )

      res.status(201).json(
        successResponse(request, 'Request created successfully')
      )
    } catch (error) {
      next(error)
    }
  }

  getRequest = async (
    req: AuthRequest,
    res: Response,
    next: NextFunction
  ) => {
    try {
      const { id } = req.params
      const userId = req.user._id
      const userRole = req.user.role

      const request = await this.requestService.getRequestById(id)

      if (!request) {
        throw new AppError('Request not found', 404)
      }

      // Check permissions
      if (
        userRole === 'customer' &&
        request.customerId.toString() !== userId.toString()
      ) {
        throw new AppError('Forbidden', 403)
      }

      if (
        userRole === 'technical' &&
        request.technicianId?.toString() !== userId.toString()
      ) {
        throw new AppError('Forbidden', 403)
      }

      res.json(successResponse(request))
    } catch (error) {
      next(error)
    }
  }

  acceptRequest = async (
    req: AuthRequest,
    res: Response,
    next: NextFunction
  ) => {
    try {
      const { id } = req.params
      const technicianId = req.user._id

      const request = await this.requestService.acceptRequest(
        id,
        technicianId
      )

      // Notify customer
      await this.notificationService.sendPushNotification(
        request.customerId,
        {
          title: 'Request Accepted',
          body: 'A technician has accepted your request',
          data: { requestId: request._id },
        }
      )

      res.json(successResponse(request, 'Request accepted successfully'))
    } catch (error) {
      next(error)
    }
  }
}
```

### 5. WebSocket Setup

```typescript
// src/socket/index.ts
import { Server } from 'socket.io'
import { Server as HttpServer } from 'http'
import jwt from 'jsonwebtoken'
import { ChatSocketHandler } from './chat.socket'
import { RequestSocketHandler } from './request.socket'
import { LocationSocketHandler } from './location.socket'

export function setupSocket(httpServer: HttpServer) {
  const io = new Server(httpServer, {
    cors: {
      origin: process.env.CORS_ORIGIN?.split(','),
      credentials: true,
    },
  })

  // Authentication middleware
  io.use((socket, next) => {
    const token = socket.handshake.auth.token

    if (!token) {
      return next(new Error('Authentication error'))
    }

    try {
      const decoded = jwt.verify(token, process.env.JWT_SECRET!) as any
      socket.data.userId = decoded.userId
      next()
    } catch (error) {
      next(new Error('Authentication error'))
    }
  })

  // Connection handler
  io.on('connection', (socket) => {
    console.log(`User connected: ${socket.data.userId}`)

    // Initialize socket handlers
    new ChatSocketHandler(io, socket)
    new RequestSocketHandler(io, socket)
    new LocationSocketHandler(io, socket)

    socket.on('disconnect', () => {
      console.log(`User disconnected: ${socket.data.userId}`)
    })
  })

  return io
}

// src/socket/chat.socket.ts
import { Server, Socket } from 'socket.io'
import { ChatService } from '../services/chat.service'

export class ChatSocketHandler {
  private chatService: ChatService

  constructor(private io: Server, private socket: Socket) {
    this.chatService = new ChatService()
    this.initialize()
  }

  private initialize() {
    this.socket.on('join_room', this.handleJoinRoom)
    this.socket.on('send_message', this.handleSendMessage)
    this.socket.on('typing', this.handleTyping)
  }

  private handleJoinRoom = async (data: { roomId: string }) => {
    const userId = this.socket.data.userId
    
    // Verify user has access to room
    const hasAccess = await this.chatService.verifyRoomAccess(
      data.roomId,
      userId
    )

    if (hasAccess) {
      this.socket.join(data.roomId)
      console.log(`User ${userId} joined room ${data.roomId}`)
    }
  }

  private handleSendMessage = async (data: {
    roomId: string
    content: string
    type: string
  }) => {
    const userId = this.socket.data.userId

    // Save message to database
    const message = await this.chatService.saveMessage({
      roomId: data.roomId,
      senderId: userId,
      content: data.content,
      type: data.type,
    })

    // Broadcast to room
    this.io.to(data.roomId).emit('new_message', message)
  }

  private handleTyping = (data: { roomId: string; isTyping: boolean }) => {
    this.socket.to(data.roomId).emit('user_typing', {
      userId: this.socket.data.userId,
      isTyping: data.isTyping,
    })
  }
}
```

### 6. Payment Service

```typescript
// src/services/payment.service.ts
import Stripe from 'stripe'
import { Payment } from '../models/Payment.model'
import { ServiceRequest } from '../models/ServiceRequest.model'
import { AppError } from '../utils/error'

export class PaymentService {
  private stripe: Stripe

  constructor() {
    this.stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
      apiVersion: '2023-10-16',
    })
  }

  async createPaymentIntent(requestId: string) {
    const request = await ServiceRequest.findById(requestId)
      .populate('customerId')
      .populate('technicianId')

    if (!request) {
      throw new AppError('Request not found', 404)
    }

    // Calculate amounts
    const subtotal = request.pricing.finalCost
    const serviceFee = Math.round(subtotal * 0.1) // 10% platform fee
    const total = subtotal + serviceFee

    // Create Stripe payment intent
    const paymentIntent = await this.stripe.paymentIntents.create({
      amount: total * 100, // Convert to cents
      currency: 'thb',
      metadata: {
        requestId: request._id.toString(),
        customerId: request.customerId._id.toString(),
        technicianId: request.technicianId._id.toString(),
      },
    })

    // Create payment record
    const payment = await Payment.create({
      serviceRequestId: requestId,
      customerId: request.customerId._id,
      technicianId: request.technicianId._id,
      amount: {
        subtotal,
        serviceFee,
        total,
        currency: 'THB',
      },
      paymentProvider: 'stripe',
      transaction: {
        externalId: paymentIntent.id,
        status: 'pending',
      },
    })

    return {
      paymentId: payment._id,
      clientSecret: paymentIntent.client_secret,
      amount: payment.amount,
    }
  }

  async confirmPayment(paymentId: string) {
    const payment = await Payment.findById(paymentId)

    if (!payment) {
      throw new AppError('Payment not found', 404)
    }

    // Verify payment with Stripe
    const paymentIntent = await this.stripe.paymentIntents.retrieve(
      payment.transaction.externalId
    )

    if (paymentIntent.status === 'succeeded') {
      // Update payment status
      payment.transaction.status = 'completed'
      payment.transaction.paidAt = new Date()
      await payment.save()

      // Update request payment status
      await ServiceRequest.findByIdAndUpdate(payment.serviceRequestId, {
        'payment.status': 'paid',
        'payment.paidAt': new Date(),
      })

      // Schedule payout to technician
      await this.schedulePayout(payment)

      return payment
    }

    throw new AppError('Payment not successful', 400)
  }

  private async schedulePayout(payment: any) {
    // Calculate technician payout (90% of subtotal)
    const payoutAmount = Math.round(payment.amount.subtotal * 0.9)

    payment.payout = {
      amount: payoutAmount,
      status: 'pending',
      scheduledAt: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000), // 7 days
    }

    await payment.save()

    // In production, this would be handled by a job queue
    // that processes payouts on schedule
  }
}
```

### 7. Error Handling

```typescript
// src/utils/error.ts
export class AppError extends Error {
  constructor(
    public message: string,
    public statusCode: number = 500,
    public isOperational: boolean = true
  ) {
    super(message)
    Object.setPrototypeOf(this, AppError.prototype)
  }
}

// src/middlewares/error.middleware.ts
import { Request, Response, NextFunction } from 'express'
import { AppError } from '../utils/error'
import { logger } from '../utils/logger'

export const errorMiddleware = (
  error: Error | AppError,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  logger.error('Error:', error)

  if (error instanceof AppError) {
    return res.status(error.statusCode).json({
      success: false,
      error: {
        message: error.message,
        statusCode: error.statusCode,
      },
    })
  }

  // Handle Mongoose validation errors
  if (error.name === 'ValidationError') {
    return res.status(400).json({
      success: false,
      error: {
        message: 'Validation error',
        details: error.message,
      },
    })
  }

  // Default error
  res.status(500).json({
    success: false,
    error: {
      message: 'Internal server error',
    },
  })
}
```

## Testing

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run integration tests
npm run test:integration
```

## Build & Deploy

```bash
# Build
npm run build

# Start production
npm start

# Docker
docker build -t fixfast-backend .
docker run -p 3000:3000 fixfast-backend
```

## Best Practices

1. **Type Safety**: Use TypeScript throughout
2. **Error Handling**: Use centralized error handling
3. **Validation**: Validate all inputs
4. **Security**: Implement helmet, rate limiting, CORS
5. **Logging**: Use Winston for structured logging
6. **Testing**: Write unit and integration tests
7. **Documentation**: Document all endpoints
8. **Code Quality**: Use ESLint and Prettier

## Contributing

Please read [CONTRIBUTING.md](../CONTRIBUTING.md) for details.

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.
