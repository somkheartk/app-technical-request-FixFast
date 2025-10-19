# API Documentation - FixFast

REST API endpoints สำหรับระบบ FixFast

## Base URL

```
Development: http://localhost:3000/api/v1
Production: https://api.fixfast.com/api/v1
```

## Authentication

ทุก API endpoint (ยกเว้น registration และ login) ต้องมี Authentication Token ใน header:

```http
Authorization: Bearer <JWT_TOKEN>
```

## Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": {}
  }
}
```

## Success Response Format

```json
{
  "success": true,
  "data": {},
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100
  }
}
```

---

## สารบัญ

1. [Authentication APIs](#1-authentication-apis)
2. [User APIs](#2-user-apis)
3. [Service Request APIs](#3-service-request-apis)
4. [Category APIs](#4-category-apis)
5. [Review APIs](#5-review-apis)
6. [Payment APIs](#6-payment-apis)
7. [Chat APIs](#7-chat-apis)
8. [Notification APIs](#8-notification-apis)
9. [Admin APIs](#9-admin-apis)
10. [Technician APIs](#10-technician-apis)

---

## 1. Authentication APIs

### 1.1 Register User

```http
POST /auth/register
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword",
  "phoneNumber": "+66812345678",
  "role": "customer",
  "profile": {
    "firstName": "John",
    "lastName": "Doe"
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "_id": "user_id",
      "email": "user@example.com",
      "role": "customer",
      "profile": {
        "firstName": "John",
        "lastName": "Doe"
      }
    },
    "token": "jwt_token_here"
  }
}
```

### 1.2 Login

```http
POST /auth/login
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "_id": "user_id",
      "email": "user@example.com",
      "role": "customer"
    },
    "token": "jwt_token_here"
  }
}
```

### 1.3 Verify Firebase Token

```http
POST /auth/firebase/verify
```

**Request Body:**
```json
{
  "firebaseToken": "firebase_id_token"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {},
    "token": "jwt_token_here"
  }
}
```

### 1.4 Refresh Token

```http
POST /auth/refresh
```

**Request Body:**
```json
{
  "refreshToken": "refresh_token_here"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "token": "new_jwt_token"
  }
}
```

### 1.5 Logout

```http
POST /auth/logout
```

**Headers:**
```
Authorization: Bearer <JWT_TOKEN>
```

**Response:**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

## 2. User APIs

### 2.1 Get Current User Profile

```http
GET /users/me
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "email": "user@example.com",
    "phoneNumber": "+66812345678",
    "role": "customer",
    "profile": {
      "firstName": "John",
      "lastName": "Doe",
      "avatarUrl": "https://...",
      "dateOfBirth": "1990-01-01"
    },
    "address": {
      "street": "123 Main St",
      "city": "Bangkok",
      "location": {
        "type": "Point",
        "coordinates": [100.5018, 13.7563]
      }
    },
    "status": "active",
    "createdAt": "2024-01-01T00:00:00.000Z"
  }
}
```

### 2.2 Update User Profile

```http
PUT /users/me
```

**Request Body:**
```json
{
  "profile": {
    "firstName": "Jane",
    "lastName": "Doe",
    "avatarUrl": "https://..."
  },
  "address": {
    "street": "456 New St",
    "city": "Bangkok",
    "province": "Bangkok",
    "postalCode": "10100",
    "location": {
      "type": "Point",
      "coordinates": [100.5018, 13.7563]
    }
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "profile": {},
    "address": {}
  }
}
```

### 2.3 Upload Profile Picture

```http
POST /users/me/avatar
Content-Type: multipart/form-data
```

**Request Body:**
```
file: <image_file>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "avatarUrl": "https://storage.../avatar.jpg"
  }
}
```

### 2.4 Get User by ID

```http
GET /users/:userId
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "profile": {
      "firstName": "John",
      "lastName": "Doe",
      "avatarUrl": "https://..."
    },
    "role": "customer"
  }
}
```

---

## 3. Service Request APIs

### 3.1 Create Service Request

```http
POST /requests
```

**Request Body:**
```json
{
  "categoryId": "category_id",
  "details": {
    "title": "Leaking pipe",
    "description": "Kitchen sink pipe is leaking",
    "images": ["https://...", "https://..."],
    "urgency": "high"
  },
  "location": {
    "address": "123 Main St, Bangkok",
    "details": "2nd floor, unit 205",
    "coordinates": {
      "type": "Point",
      "coordinates": [100.5018, 13.7563]
    }
  },
  "schedule": {
    "preferredDate": "2024-01-15",
    "preferredTime": "morning",
    "isFlexible": true
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "request_id",
    "requestNumber": "REQ-2024-00001",
    "customerId": "user_id",
    "categoryId": "category_id",
    "status": "pending",
    "details": {},
    "location": {},
    "schedule": {},
    "createdAt": "2024-01-10T10:00:00.000Z"
  }
}
```

### 3.2 Get Service Request by ID

```http
GET /requests/:requestId
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "request_id",
    "requestNumber": "REQ-2024-00001",
    "customer": {
      "_id": "customer_id",
      "profile": {
        "firstName": "John",
        "lastName": "Doe"
      }
    },
    "technician": {
      "_id": "tech_id",
      "profile": {
        "firstName": "Tech",
        "lastName": "Name"
      }
    },
    "category": {
      "_id": "category_id",
      "name": {
        "en": "Plumbing",
        "th": "ช่างประปา"
      }
    },
    "status": "in_progress",
    "details": {},
    "location": {},
    "pricing": {},
    "timeline": {},
    "statusHistory": []
  }
}
```

### 3.3 List Service Requests

```http
GET /requests?status=pending&page=1&limit=20
```

**Query Parameters:**
- `status`: Filter by status (pending, accepted, in_progress, completed, cancelled)
- `categoryId`: Filter by category
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20, max: 100)
- `sort`: Sort field (createdAt, updatedAt)
- `order`: Sort order (asc, desc)

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "request_id",
      "requestNumber": "REQ-2024-00001",
      "status": "pending",
      "details": {
        "title": "Leaking pipe"
      },
      "createdAt": "2024-01-10T10:00:00.000Z"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

### 3.4 Update Service Request Status

```http
PATCH /requests/:requestId/status
```

**Request Body:**
```json
{
  "status": "in_progress",
  "note": "Started working on the issue"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "request_id",
    "status": "in_progress",
    "statusHistory": [
      {
        "status": "in_progress",
        "timestamp": "2024-01-10T11:00:00.000Z",
        "note": "Started working on the issue",
        "updatedBy": "user_id"
      }
    ]
  }
}
```

### 3.5 Update Service Request Pricing

```http
PATCH /requests/:requestId/pricing
```

**Request Body:**
```json
{
  "quotedCost": 1500,
  "note": "Quote for parts and labor"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "request_id",
    "pricing": {
      "quotedCost": 1500,
      "currency": "THB"
    }
  }
}
```

### 3.6 Cancel Service Request

```http
POST /requests/:requestId/cancel
```

**Request Body:**
```json
{
  "reason": "Found another technician"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "request_id",
    "status": "cancelled",
    "cancellation": {
      "cancelledBy": "customer",
      "reason": "Found another technician",
      "timestamp": "2024-01-10T12:00:00.000Z"
    }
  }
}
```

### 3.7 Upload Request Images

```http
POST /requests/:requestId/images
Content-Type: multipart/form-data
```

**Request Body:**
```
files: <image_files>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "images": [
      "https://storage.../image1.jpg",
      "https://storage.../image2.jpg"
    ]
  }
}
```

---

## 4. Category APIs

### 4.1 List Categories

```http
GET /categories
```

**Query Parameters:**
- `parentId`: Get subcategories of a parent (null for top-level)
- `isActive`: Filter by active status

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "category_id",
      "name": {
        "en": "Plumbing",
        "th": "ช่างประปา"
      },
      "description": {
        "en": "Water and pipe related services",
        "th": "บริการเกี่ยวกับน้ำและท่อ"
      },
      "icon": "plumbing_icon.svg",
      "estimatedPrice": {
        "min": 500,
        "max": 3000,
        "unit": "job"
      },
      "isActive": true
    }
  ]
}
```

### 4.2 Get Category by ID

```http
GET /categories/:categoryId
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "category_id",
    "name": {
      "en": "Plumbing",
      "th": "ช่างประปา"
    },
    "description": {},
    "subcategories": [
      {
        "_id": "sub_category_id",
        "name": {
          "en": "Leak Repair",
          "th": "ซ่อมท่อรั่ว"
        }
      }
    ]
  }
}
```

---

## 5. Review APIs

### 5.1 Create Review

```http
POST /reviews
```

**Request Body:**
```json
{
  "serviceRequestId": "request_id",
  "rating": {
    "overall": 5,
    "quality": 5,
    "punctuality": 4,
    "professionalism": 5,
    "communication": 5
  },
  "comment": "Excellent service!",
  "images": ["https://..."]
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "review_id",
    "serviceRequestId": "request_id",
    "reviewerId": "customer_id",
    "revieweeId": "technician_id",
    "rating": {},
    "comment": "Excellent service!",
    "createdAt": "2024-01-10T14:00:00.000Z"
  }
}
```

### 5.2 Get Reviews for Technician

```http
GET /reviews/technician/:technicianId?page=1&limit=10
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "review_id",
      "reviewer": {
        "_id": "customer_id",
        "profile": {
          "firstName": "John",
          "displayName": "John D."
        }
      },
      "rating": {
        "overall": 5
      },
      "comment": "Great work!",
      "createdAt": "2024-01-10T14:00:00.000Z"
    }
  ],
  "meta": {
    "averageRating": 4.8,
    "totalReviews": 150,
    "page": 1,
    "limit": 10
  }
}
```

### 5.3 Respond to Review

```http
POST /reviews/:reviewId/response
```

**Request Body:**
```json
{
  "comment": "Thank you for your feedback!"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "review_id",
    "response": {
      "comment": "Thank you for your feedback!",
      "respondedAt": "2024-01-10T15:00:00.000Z"
    }
  }
}
```

---

## 6. Payment APIs

### 6.1 Create Payment Intent

```http
POST /payments/intent
```

**Request Body:**
```json
{
  "serviceRequestId": "request_id",
  "paymentMethod": "credit_card"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "paymentId": "payment_id",
    "clientSecret": "stripe_client_secret",
    "amount": {
      "subtotal": 1500,
      "serviceFee": 150,
      "total": 1650,
      "currency": "THB"
    }
  }
}
```

### 6.2 Confirm Payment

```http
POST /payments/:paymentId/confirm
```

**Request Body:**
```json
{
  "paymentMethodId": "pm_xxx",
  "paymentProvider": "stripe"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "payment_id",
    "status": "completed",
    "receipt": {
      "receiptNumber": "RCP-2024-00001",
      "receiptUrl": "https://..."
    }
  }
}
```

### 6.3 Get Payment Details

```http
GET /payments/:paymentId
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "payment_id",
    "serviceRequestId": "request_id",
    "amount": {},
    "transaction": {
      "status": "completed",
      "paidAt": "2024-01-10T13:00:00.000Z"
    },
    "receipt": {}
  }
}
```

### 6.4 Request Refund

```http
POST /payments/:paymentId/refund
```

**Request Body:**
```json
{
  "reason": "Service not satisfactory",
  "amount": 1650
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "payment_id",
    "transaction": {
      "status": "refunded",
      "refundedAt": "2024-01-10T16:00:00.000Z"
    }
  }
}
```

---

## 7. Chat APIs

### 7.1 Get Chat for Service Request

```http
GET /chats/request/:requestId
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "chat_id",
    "serviceRequestId": "request_id",
    "participants": [
      {
        "userId": "customer_id",
        "role": "customer"
      },
      {
        "userId": "tech_id",
        "role": "technician"
      }
    ],
    "messages": [
      {
        "messageId": "msg_id",
        "senderId": "customer_id",
        "content": "Hello, when can you arrive?",
        "type": "text",
        "timestamp": "2024-01-10T10:30:00.000Z",
        "isRead": true
      }
    ]
  }
}
```

### 7.2 Send Message

```http
POST /chats/:chatId/messages
```

**Request Body:**
```json
{
  "content": "I'll be there in 30 minutes",
  "type": "text"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "messageId": "msg_id",
    "senderId": "tech_id",
    "content": "I'll be there in 30 minutes",
    "type": "text",
    "timestamp": "2024-01-10T10:31:00.000Z"
  }
}
```

### 7.3 Mark Messages as Read

```http
POST /chats/:chatId/read
```

**Request Body:**
```json
{
  "messageIds": ["msg_id1", "msg_id2"]
}
```

**Response:**
```json
{
  "success": true,
  "message": "Messages marked as read"
}
```

---

## 8. Notification APIs

### 8.1 Get User Notifications

```http
GET /notifications?page=1&limit=20&isRead=false
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "notification_id",
      "type": "request_update",
      "title": "Request Accepted",
      "message": "A technician has accepted your request",
      "data": {
        "serviceRequestId": "request_id"
      },
      "isRead": false,
      "createdAt": "2024-01-10T10:00:00.000Z"
    }
  ],
  "meta": {
    "unreadCount": 5,
    "page": 1,
    "limit": 20
  }
}
```

### 8.2 Mark Notification as Read

```http
PATCH /notifications/:notificationId/read
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "notification_id",
    "isRead": true,
    "readAt": "2024-01-10T11:00:00.000Z"
  }
}
```

### 8.3 Mark All Notifications as Read

```http
POST /notifications/read-all
```

**Response:**
```json
{
  "success": true,
  "message": "All notifications marked as read",
  "data": {
    "updatedCount": 10
  }
}
```

### 8.4 Update FCM Token

```http
POST /notifications/fcm-token
```

**Request Body:**
```json
{
  "fcmToken": "firebase_cloud_messaging_token"
}
```

**Response:**
```json
{
  "success": true,
  "message": "FCM token updated"
}
```

---

## 9. Admin APIs

### 9.1 Get Dashboard Statistics

```http
GET /admin/dashboard
```

**Response:**
```json
{
  "success": true,
  "data": {
    "statistics": {
      "totalUsers": {
        "customers": 1000,
        "technicians": 150,
        "total": 1150
      },
      "serviceRequests": {
        "pending": 25,
        "inProgress": 40,
        "completed": 850,
        "total": 915
      },
      "revenue": {
        "today": 15000,
        "thisWeek": 85000,
        "thisMonth": 350000,
        "total": 2500000
      },
      "averageRating": 4.7
    },
    "recentActivity": [],
    "topTechnicians": []
  }
}
```

### 9.2 List All Users

```http
GET /admin/users?role=technician&status=pending&page=1&limit=20
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "user_id",
      "email": "user@example.com",
      "role": "technician",
      "status": "pending",
      "profile": {},
      "createdAt": "2024-01-10T00:00:00.000Z"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 50
  }
}
```

### 9.3 Approve Technician

```http
POST /admin/technicians/:technicianId/approve
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "technician_id",
    "status": "active"
  }
}
```

### 9.4 Suspend User

```http
POST /admin/users/:userId/suspend
```

**Request Body:**
```json
{
  "reason": "Violation of terms of service"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "status": "suspended"
  }
}
```

### 9.5 Manage Categories

```http
POST /admin/categories
```

**Request Body:**
```json
{
  "name": {
    "en": "Electrical",
    "th": "ช่างไฟฟ้า"
  },
  "description": {
    "en": "Electrical services",
    "th": "บริการด้านไฟฟ้า"
  },
  "estimatedPrice": {
    "min": 500,
    "max": 5000,
    "unit": "job"
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "category_id",
    "name": {},
    "isActive": true
  }
}
```

---

## 10. Technician APIs

### 10.1 Get Available Jobs

```http
GET /technicians/jobs/available?maxDistance=10&categoryId=xxx
```

**Query Parameters:**
- `maxDistance`: Maximum distance in kilometers
- `categoryId`: Filter by category
- `urgency`: Filter by urgency level

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "request_id",
      "requestNumber": "REQ-2024-00001",
      "category": {
        "name": {
          "en": "Plumbing"
        }
      },
      "details": {
        "title": "Leaking pipe",
        "urgency": "high"
      },
      "location": {
        "address": "123 Main St",
        "distance": 2.5
      },
      "schedule": {},
      "pricing": {
        "estimatedCost": 1500
      },
      "createdAt": "2024-01-10T10:00:00.000Z"
    }
  ]
}
```

### 10.2 Accept Job

```http
POST /technicians/jobs/:requestId/accept
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "request_id",
    "status": "accepted",
    "technicianId": "tech_id"
  }
}
```

### 10.3 Decline Job

```http
POST /technicians/jobs/:requestId/decline
```

**Request Body:**
```json
{
  "reason": "Too far from current location"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Job declined"
}
```

### 10.4 Update Availability

```http
PATCH /technicians/me/availability
```

**Request Body:**
```json
{
  "isAvailable": true,
  "maxRadius": 15
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "availability": {
      "isAvailable": true,
      "maxRadius": 15
    }
  }
}
```

### 10.5 Get My Jobs

```http
GET /technicians/jobs/my?status=in_progress&page=1&limit=20
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "request_id",
      "requestNumber": "REQ-2024-00001",
      "status": "in_progress",
      "customer": {},
      "details": {},
      "location": {}
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 50
  }
}
```

### 10.6 Get Earnings

```http
GET /technicians/earnings?period=month&year=2024&month=1
```

**Response:**
```json
{
  "success": true,
  "data": {
    "period": "2024-01",
    "totalJobs": 25,
    "completedJobs": 24,
    "earnings": {
      "gross": 45000,
      "platformFee": 4500,
      "net": 40500
    },
    "pendingPayout": 15000,
    "payoutHistory": []
  }
}
```

---

## WebSocket Events

### Connection

```javascript
// Connect to WebSocket
const socket = io('wss://api.fixfast.com', {
  auth: {
    token: 'jwt_token'
  }
});
```

### Events

#### 1. Join Room
```javascript
socket.emit('join_room', {
  roomId: 'request_id'
});
```

#### 2. New Message
```javascript
socket.on('new_message', (data) => {
  console.log('New message:', data);
});
```

#### 3. Status Update
```javascript
socket.on('status_update', (data) => {
  console.log('Status changed:', data);
});
```

#### 4. Location Update (Technician)
```javascript
socket.emit('update_location', {
  coordinates: [100.5018, 13.7563]
});
```

#### 5. New Job Available (Technician)
```javascript
socket.on('new_job_available', (data) => {
  console.log('New job:', data);
});
```

---

## Rate Limiting

- Anonymous requests: 100 requests/hour
- Authenticated requests: 1000 requests/hour
- Admin requests: 5000 requests/hour

## Status Codes

- `200 OK`: Successful request
- `201 Created`: Resource created successfully
- `400 Bad Request`: Invalid request data
- `401 Unauthorized`: Authentication required
- `403 Forbidden`: Insufficient permissions
- `404 Not Found`: Resource not found
- `409 Conflict`: Resource conflict (e.g., duplicate)
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: Server error

## Pagination

All list endpoints support pagination:
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20, max: 100)

Response includes `meta` object:
```json
{
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

## Sorting

List endpoints support sorting:
- `sort`: Field to sort by (e.g., `createdAt`, `updatedAt`)
- `order`: Sort order (`asc` or `desc`)

Example:
```
GET /requests?sort=createdAt&order=desc
```

## Filtering

Use query parameters for filtering:
```
GET /requests?status=pending&categoryId=xxx&urgency=high
```

## File Upload

File upload endpoints accept `multipart/form-data`:
- Max file size: 10MB
- Allowed formats: jpg, jpeg, png, pdf
- Multiple files supported (max 5 per request)

