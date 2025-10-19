# Admin Web Dashboard - Next.js

เว็บแอปพลิเคชันสำหรับ Admin ในการจัดการระบบ FixFast

## โครงสร้างโปรเจค

```
admin-web/
├── public/                     # Static files
│   ├── images/
│   ├── icons/
│   └── favicon.ico
│
├── src/
│   ├── app/                   # Next.js 14 App Router
│   │   ├── layout.tsx         # Root layout
│   │   ├── page.tsx           # Home/Dashboard page
│   │   ├── globals.css        # Global styles
│   │   │
│   │   ├── (auth)/            # Auth group
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   └── layout.tsx
│   │   │
│   │   ├── (dashboard)/       # Dashboard group
│   │   │   ├── layout.tsx     # Dashboard layout with sidebar
│   │   │   │
│   │   │   ├── dashboard/     # Main dashboard
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── users/         # User management
│   │   │   │   ├── page.tsx   # Users list
│   │   │   │   ├── [id]/
│   │   │   │   │   └── page.tsx  # User detail
│   │   │   │   └── customers/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── technicians/   # Technician management
│   │   │   │   ├── page.tsx   # Technicians list
│   │   │   │   ├── [id]/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── pending/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── approved/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── requests/      # Service requests
│   │   │   │   ├── page.tsx
│   │   │   │   ├── [id]/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── pending/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── active/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── completed/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── payments/      # Payment management
│   │   │   │   ├── page.tsx
│   │   │   │   ├── [id]/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── pending/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── payouts/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── categories/    # Category management
│   │   │   │   ├── page.tsx
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── reviews/       # Reviews & ratings
│   │   │   │   ├── page.tsx
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── reports/       # Reports & analytics
│   │   │   │   ├── page.tsx
│   │   │   │   ├── revenue/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── users/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── performance/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   └── settings/      # System settings
│   │   │       ├── page.tsx
│   │   │       ├── general/
│   │   │       │   └── page.tsx
│   │   │       └── notifications/
│   │   │           └── page.tsx
│   │   │
│   │   └── api/               # API routes
│   │       ├── auth/
│   │       │   └── [...nextauth]/
│   │       │       └── route.ts
│   │       └── webhook/
│   │           └── route.ts
│   │
│   ├── components/            # Reusable components
│   │   ├── common/
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── Modal.tsx
│   │   │   ├── Table.tsx
│   │   │   ├── Pagination.tsx
│   │   │   └── LoadingSpinner.tsx
│   │   │
│   │   ├── layout/
│   │   │   ├── Sidebar.tsx
│   │   │   ├── Header.tsx
│   │   │   ├── Footer.tsx
│   │   │   └── Breadcrumb.tsx
│   │   │
│   │   ├── dashboard/
│   │   │   ├── StatsCard.tsx
│   │   │   ├── RevenueChart.tsx
│   │   │   ├── UserGrowthChart.tsx
│   │   │   ├── RecentActivity.tsx
│   │   │   └── TopTechnicians.tsx
│   │   │
│   │   ├── users/
│   │   │   ├── UserTable.tsx
│   │   │   ├── UserCard.tsx
│   │   │   ├── UserForm.tsx
│   │   │   └── UserFilter.tsx
│   │   │
│   │   ├── requests/
│   │   │   ├── RequestTable.tsx
│   │   │   ├── RequestCard.tsx
│   │   │   ├── RequestDetail.tsx
│   │   │   ├── RequestMap.tsx
│   │   │   ├── StatusBadge.tsx
│   │   │   └── RequestTimeline.tsx
│   │   │
│   │   ├── payments/
│   │   │   ├── PaymentTable.tsx
│   │   │   ├── PaymentDetail.tsx
│   │   │   └── RefundForm.tsx
│   │   │
│   │   ├── charts/
│   │   │   ├── LineChart.tsx
│   │   │   ├── BarChart.tsx
│   │   │   ├── PieChart.tsx
│   │   │   └── AreaChart.tsx
│   │   │
│   │   └── forms/
│   │       ├── CategoryForm.tsx
│   │       ├── TechnicianApprovalForm.tsx
│   │       └── SettingsForm.tsx
│   │
│   ├── lib/                   # Utilities and helpers
│   │   ├── api/
│   │   │   ├── client.ts      # API client setup
│   │   │   ├── auth.ts
│   │   │   ├── users.ts
│   │   │   ├── requests.ts
│   │   │   ├── payments.ts
│   │   │   └── categories.ts
│   │   │
│   │   ├── hooks/             # Custom React hooks
│   │   │   ├── useAuth.ts
│   │   │   ├── useUsers.ts
│   │   │   ├── useRequests.ts
│   │   │   ├── usePayments.ts
│   │   │   └── useSocket.ts
│   │   │
│   │   ├── store/             # State management (Zustand)
│   │   │   ├── authStore.ts
│   │   │   ├── userStore.ts
│   │   │   └── notificationStore.ts
│   │   │
│   │   ├── utils/             # Utility functions
│   │   │   ├── formatters.ts
│   │   │   ├── validators.ts
│   │   │   ├── helpers.ts
│   │   │   └── constants.ts
│   │   │
│   │   └── types/             # TypeScript types
│   │       ├── user.ts
│   │       ├── request.ts
│   │       ├── payment.ts
│   │       └── common.ts
│   │
│   └── styles/                # Additional styles
│       └── components.css
│
├── .env.local                 # Environment variables
├── .env.example               # Example environment file
├── next.config.js             # Next.js configuration
├── tailwind.config.js         # Tailwind CSS configuration
├── tsconfig.json              # TypeScript configuration
├── package.json               # Dependencies
└── README.md                  # This file
```

## Technology Stack

```json
{
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "typescript": "^5.0.0",
    
    "@tanstack/react-query": "^5.0.0",
    "axios": "^1.6.0",
    "socket.io-client": "^4.6.0",
    
    "next-auth": "^4.24.0",
    
    "zustand": "^4.4.0",
    
    "recharts": "^2.10.0",
    "chart.js": "^4.4.0",
    "react-chartjs-2": "^5.2.0",
    
    "react-hook-form": "^7.48.0",
    "zod": "^3.22.0",
    "@hookform/resolvers": "^3.3.0",
    
    "date-fns": "^2.30.0",
    "dayjs": "^1.11.0",
    
    "@headlessui/react": "^1.7.0",
    "@heroicons/react": "^2.0.0",
    "react-icons": "^4.12.0",
    
    "clsx": "^2.0.0",
    "tailwind-merge": "^2.0.0",
    
    "react-hot-toast": "^2.4.0",
    "sonner": "^1.2.0",
    
    "react-table": "^7.8.0",
    "@tanstack/react-table": "^8.10.0",
    
    "react-leaflet": "^4.2.0",
    "leaflet": "^1.9.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "tailwindcss": "^3.3.0",
    "eslint": "^8.0.0",
    "eslint-config-next": "^14.0.0"
  }
}
```

## Features

### 1. Dashboard
- Overview statistics
- Revenue charts
- User growth metrics
- Active requests count
- Recent activity feed
- Top performing technicians

### 2. User Management
- List all users (customers & technicians)
- Filter and search users
- View user details
- Edit user information
- Suspend/activate users
- Export user data

### 3. Technician Management
- View pending registrations
- Approve/reject technicians
- Verify documents
- View technician profiles
- Manage specializations
- Monitor performance metrics

### 4. Service Request Management
- View all service requests
- Filter by status, category, date
- Assign technicians manually
- Update request status
- View request history
- Track on map
- Cancel requests

### 5. Payment Management
- View all transactions
- Track payment status
- Process refunds
- Manage payouts to technicians
- Generate invoices
- Export financial reports

### 6. Category Management
- Create/edit/delete categories
- Manage subcategories
- Set estimated prices
- Upload category icons
- Toggle category status

### 7. Review Management
- View all reviews
- Moderate reviews
- Handle flagged content
- Response management
- Rating analytics

### 8. Reports & Analytics
- Revenue reports
- User statistics
- Technician performance
- Category popularity
- Geographic distribution
- Time-based analytics
- Export data (CSV, PDF)

### 9. System Settings
- General configuration
- Notification templates
- Email settings
- Payment gateway settings
- App settings

## Setup Instructions

### Prerequisites
- Node.js 18+
- npm or yarn
- Backend API running

### Installation

1. Clone repository:
```bash
git clone https://github.com/your-repo/fixfast-admin-web.git
cd admin-web
```

2. Install dependencies:
```bash
npm install
# or
yarn install
```

3. Create environment file:
```bash
cp .env.example .env.local
```

4. Configure environment variables:
```env
# .env.local
NEXT_PUBLIC_API_URL=http://localhost:3000/api/v1
NEXT_PUBLIC_SOCKET_URL=http://localhost:3000
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your_google_maps_api_key

NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=http://localhost:3001
```

5. Run development server:
```bash
npm run dev
# or
yarn dev
```

6. Open browser:
```
http://localhost:3001
```

## Key Implementations

### 1. Authentication with NextAuth

```typescript
// app/api/auth/[...nextauth]/route.ts
import NextAuth from 'next-auth'
import CredentialsProvider from 'next-auth/providers/credentials'
import { api } from '@/lib/api/client'

const handler = NextAuth({
  providers: [
    CredentialsProvider({
      name: 'Credentials',
      credentials: {
        email: { label: "Email", type: "email" },
        password: { label: "Password", type: "password" }
      },
      async authorize(credentials) {
        try {
          const response = await api.post('/auth/login', {
            email: credentials?.email,
            password: credentials?.password,
          })
          
          if (response.data.success && response.data.data.user.role === 'admin') {
            return {
              id: response.data.data.user._id,
              email: response.data.data.user.email,
              name: response.data.data.user.profile.firstName,
              token: response.data.data.token,
            }
          }
          
          return null
        } catch (error) {
          return null
        }
      }
    })
  ],
  callbacks: {
    async jwt({ token, user }) {
      if (user) {
        token.id = user.id
        token.accessToken = user.token
      }
      return token
    },
    async session({ session, token }) {
      if (token) {
        session.user.id = token.id
        session.accessToken = token.accessToken
      }
      return session
    }
  },
  pages: {
    signIn: '/login',
  },
})

export { handler as GET, handler as POST }
```

### 2. API Client Setup

```typescript
// lib/api/client.ts
import axios from 'axios'
import { getSession } from 'next-auth/react'

export const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
})

// Request interceptor
api.interceptors.request.use(
  async (config) => {
    const session = await getSession()
    if (session?.accessToken) {
      config.headers.Authorization = `Bearer ${session.accessToken}`
    }
    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)

// Response interceptor
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Redirect to login
      window.location.href = '/login'
    }
    return Promise.reject(error)
  }
)
```

### 3. React Query Setup

```typescript
// app/providers.tsx
'use client'

import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { SessionProvider } from 'next-auth/react'
import { useState } from 'react'

export function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(() => new QueryClient({
    defaultOptions: {
      queries: {
        staleTime: 60 * 1000, // 1 minute
        refetchOnWindowFocus: false,
      },
    },
  }))

  return (
    <SessionProvider>
      <QueryClientProvider client={queryClient}>
        {children}
      </QueryClientProvider>
    </SessionProvider>
  )
}
```

### 4. Custom Hook Example

```typescript
// lib/hooks/useRequests.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'
import { api } from '@/lib/api/client'
import type { ServiceRequest, RequestFilters } from '@/lib/types/request'

export function useRequests(filters?: RequestFilters) {
  return useQuery({
    queryKey: ['requests', filters],
    queryFn: async () => {
      const response = await api.get('/requests', { params: filters })
      return response.data.data
    },
  })
}

export function useRequest(id: string) {
  return useQuery({
    queryKey: ['request', id],
    queryFn: async () => {
      const response = await api.get(`/requests/${id}`)
      return response.data.data
    },
    enabled: !!id,
  })
}

export function useUpdateRequestStatus() {
  const queryClient = useQueryClient()
  
  return useMutation({
    mutationFn: async ({ id, status }: { id: string; status: string }) => {
      const response = await api.patch(`/requests/${id}/status`, { status })
      return response.data.data
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['requests'] })
    },
  })
}
```

### 5. Dashboard Component

```typescript
// app/(dashboard)/dashboard/page.tsx
'use client'

import { useQuery } from '@tanstack/react-query'
import { api } from '@/lib/api/client'
import StatsCard from '@/components/dashboard/StatsCard'
import RevenueChart from '@/components/dashboard/RevenueChart'
import RecentActivity from '@/components/dashboard/RecentActivity'

export default function DashboardPage() {
  const { data: stats, isLoading } = useQuery({
    queryKey: ['dashboard-stats'],
    queryFn: async () => {
      const response = await api.get('/admin/dashboard')
      return response.data.data
    },
  })

  if (isLoading) return <div>Loading...</div>

  return (
    <div className="space-y-6">
      <h1 className="text-3xl font-bold">Dashboard</h1>
      
      {/* Stats Cards */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <StatsCard
          title="Total Users"
          value={stats.statistics.totalUsers.total}
          icon="users"
          trend="+12%"
        />
        <StatsCard
          title="Active Requests"
          value={stats.statistics.serviceRequests.inProgress}
          icon="briefcase"
          trend="+5%"
        />
        <StatsCard
          title="Monthly Revenue"
          value={`฿${stats.statistics.revenue.thisMonth.toLocaleString()}`}
          icon="dollar"
          trend="+23%"
        />
        <StatsCard
          title="Avg Rating"
          value={stats.statistics.averageRating}
          icon="star"
          trend="+0.2"
        />
      </div>

      {/* Charts */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <RevenueChart data={stats.revenueData} />
        <RecentActivity activities={stats.recentActivity} />
      </div>
    </div>
  )
}
```

### 6. Data Table Component

```typescript
// components/common/Table.tsx
'use client'

import { flexRender, getCoreRowModel, useReactTable } from '@tanstack/react-table'
import type { ColumnDef } from '@tanstack/react-table'

interface TableProps<T> {
  data: T[]
  columns: ColumnDef<T>[]
}

export function Table<T>({ data, columns }: TableProps<T>) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
  })

  return (
    <div className="overflow-x-auto">
      <table className="min-w-full divide-y divide-gray-200">
        <thead className="bg-gray-50">
          {table.getHeaderGroups().map(headerGroup => (
            <tr key={headerGroup.id}>
              {headerGroup.headers.map(header => (
                <th
                  key={header.id}
                  className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  {flexRender(
                    header.column.columnDef.header,
                    header.getContext()
                  )}
                </th>
              ))}
            </tr>
          ))}
        </thead>
        <tbody className="bg-white divide-y divide-gray-200">
          {table.getRowModel().rows.map(row => (
            <tr key={row.id}>
              {row.getVisibleCells().map(cell => (
                <td key={cell.id} className="px-6 py-4 whitespace-nowrap">
                  {flexRender(cell.column.columnDef.cell, cell.getContext())}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  )
}
```

### 7. Real-time Updates with Socket.io

```typescript
// lib/hooks/useSocket.ts
import { useEffect } from 'react'
import { io, Socket } from 'socket.io-client'
import { useSession } from 'next-auth/react'

let socket: Socket | null = null

export function useSocket() {
  const { data: session } = useSession()

  useEffect(() => {
    if (!session?.accessToken) return

    socket = io(process.env.NEXT_PUBLIC_SOCKET_URL!, {
      auth: {
        token: session.accessToken,
      },
    })

    socket.on('connect', () => {
      console.log('Socket connected')
    })

    return () => {
      socket?.disconnect()
    }
  }, [session])

  return socket
}

// Usage
export function useRequestUpdates(onUpdate: (data: any) => void) {
  const socket = useSocket()

  useEffect(() => {
    if (!socket) return

    socket.on('request_update', onUpdate)

    return () => {
      socket.off('request_update', onUpdate)
    }
  }, [socket, onUpdate])
}
```

## Build & Deployment

### Build for Production

```bash
npm run build
npm run start
```

### Docker Deployment

```dockerfile
# Dockerfile
FROM node:18-alpine AS base

# Dependencies
FROM base AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

# Builder
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Runner
FROM base AS runner
WORKDIR /app

ENV NODE_ENV production

COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3001

CMD ["node", "server.js"]
```

### Deploy to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

## Environment Variables

```env
# API
NEXT_PUBLIC_API_URL=https://api.fixfast.com/api/v1
NEXT_PUBLIC_SOCKET_URL=https://api.fixfast.com

# Authentication
NEXTAUTH_SECRET=your-secret-here
NEXTAUTH_URL=https://admin.fixfast.com

# Google Maps
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your-key-here

# Analytics (Optional)
NEXT_PUBLIC_GA_ID=your-ga-id
```

## Testing

```bash
# Run tests
npm test

# Run with coverage
npm test -- --coverage

# E2E tests with Playwright
npm run test:e2e
```

## Best Practices

1. **Type Safety**: Use TypeScript for all components
2. **Server Components**: Use React Server Components where possible
3. **Client Components**: Only use 'use client' when necessary
4. **Data Fetching**: Use React Query for client-side data fetching
5. **Error Handling**: Implement proper error boundaries
6. **Loading States**: Show loading indicators for async operations
7. **Accessibility**: Follow WCAG guidelines
8. **Performance**: Optimize images and implement lazy loading

## Contributing

Please read [CONTRIBUTING.md](../CONTRIBUTING.md) for details on contributing to this project.

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.
