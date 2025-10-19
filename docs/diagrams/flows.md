# System Flows - FixFast

Flow diagrams สำหรับกระบวนการสำคัญในระบบ FixFast

## สารบัญ

1. [User Registration Flow](#1-user-registration-flow)
2. [Service Request Creation Flow](#2-service-request-creation-flow)
3. [Job Assignment Flow](#3-job-assignment-flow)
4. [Service Execution Flow](#4-service-execution-flow)
5. [Payment Flow](#5-payment-flow)
6. [Review and Rating Flow](#6-review-and-rating-flow)

---

## 1. User Registration Flow

### Customer Registration

```
┌────────────┐
│  Customer  │
│  Opens App │
└─────┬──────┘
      │
      ▼
┌─────────────────┐
│  Registration   │
│     Screen      │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐      Yes    ┌──────────────┐
│  Enter Email/   │────────────→│   Firebase   │
│  Phone Number   │              │     Auth     │
└─────┬───────────┘              └──────┬───────┘
      │                                  │
      │ No                              │
      ▼                                  ▼
┌─────────────────┐              ┌──────────────┐
│  Enter OTP or   │◄─────────────│  Send OTP/   │
│  Password       │              │  Verify      │
└─────┬───────────┘              └──────────────┘
      │
      ▼
┌─────────────────┐
│  Verification   │
│   Successful    │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Enter Profile  │
│   Information   │
│  (Name, Photo)  │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Add Address    │
│  (Optional)     │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Save to        │
│  Database       │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Registration   │
│   Complete!     │
│  → Home Screen  │
└─────────────────┘
```

### Technician Registration

```
┌────────────┐
│ Technician │
│  Opens App │
└─────┬──────┘
      │
      ▼
┌─────────────────┐
│  Registration   │
│     Screen      │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Basic Info     │
│  (Same as       │
│   Customer)     │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Select         │
│  Specialization │
│  (Categories)   │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Enter          │
│  Experience &   │
│  Certifications │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Upload         │
│  Documents      │
│  (ID Card, etc.)│
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Set Working    │
│  Hours &        │
│  Service Area   │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Bank Account   │
│  Information    │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Submit for     │
│  Admin Approval │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐      ┌──────────────┐
│  Pending        │      │    Admin     │
│  Verification   │◄─────│  Reviews &   │
└─────┬───────────┘      │  Approves    │
      │                  └──────────────┘
      ▼
┌─────────────────┐
│   Approved!     │
│  → Home Screen  │
└─────────────────┘
```

---

## 2. Service Request Creation Flow

```
┌────────────┐
│  Customer  │
│  Logged In │
└─────┬──────┘
      │
      ▼
┌─────────────────┐
│  Home Screen    │
│  "Request       │
│   Service"      │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Select         │
│  Category       │
│  (e.g., Plumber)│
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Enter Problem  │
│  Description    │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Upload Photos  │
│  (Optional)     │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Choose/Confirm │
│  Service        │
│  Location       │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Set Preferred  │
│  Date & Time    │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Set Urgency    │
│  Level          │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Review Request │
│  Summary        │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│  Confirm &      │
│  Submit         │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐      ┌──────────────┐
│  Create Request │─────→│  Save to     │
│  in Database    │      │  Database    │
└─────┬───────────┘      └──────────────┘
      │
      ├──────────────────────┐
      │                      │
      ▼                      ▼
┌─────────────────┐    ┌──────────────┐
│  Send           │    │  Notify      │
│  Notification   │    │  Matching    │
│  to Customer    │    │  Technicians │
└─────────────────┘    └──────────────┘
      │
      ▼
┌─────────────────┐
│  Status:        │
│  "PENDING"      │
│  → Track Screen │
└─────────────────┘
```

---

## 3. Job Assignment Flow

```
┌────────────────┐
│  New Request   │
│    Created     │
└────────┬───────┘
         │
         ▼
┌────────────────────┐
│  System Finds      │
│  Nearby Technicians│
│  (Based on:        │
│   - Location       │
│   - Specialization │
│   - Availability)  │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│  Send Push         │
│  Notification to   │
│  Matched           │
│  Technicians       │
└────────┬───────────┘
         │
         ├─────────────────┬─────────────────┐
         │                 │                 │
         ▼                 ▼                 ▼
   ┌──────────┐      ┌──────────┐    ┌──────────┐
   │Technical │      │Technical │    │Technical │
   │    #1    │      │    #2    │    │    #3    │
   └────┬─────┘      └────┬─────┘    └────┬─────┘
        │                 │                │
        │ Views           │ Views          │ Views
        ▼                 ▼                ▼
   ┌──────────┐      ┌──────────┐    ┌──────────┐
   │ Accept?  │      │ Accept?  │    │ Accept?  │
   └────┬─────┘      └────┬─────┘    └────┬─────┘
        │                 │                │
        │ Accept          │ Decline        │ No Response
        ▼                 ▼                │ (Timeout)
   ┌──────────┐      ┌──────────┐         │
   │  First   │      │ Continue │         │
   │ to Accept│      │ Waiting  │         │
   │   WINS   │      └──────────┘         │
   └────┬─────┘                            │
        │                                  │
        ▼                                  │
┌────────────────┐                         │
│  Assign Job to │                         │
│  Technician #1 │                         │
└────────┬───────┘                         │
         │                                 │
         ▼                                 │
┌────────────────┐                         │
│  Update Status │                         │
│  to "ACCEPTED" │                         │
└────────┬───────┘                         │
         │                                 │
         ├───────────────┬─────────────────┘
         │               │
         ▼               ▼
┌────────────────┐  ┌──────────────┐
│  Notify        │  │  Notify Other│
│  Customer      │  │  Technicians │
│  "Tech Found!" │  │  "Job Taken" │
└────────────────┘  └──────────────┘
         │
         ▼
┌────────────────┐
│  Share Contact │
│  Info & Start  │
│  Chat          │
└────────────────┘
```

### Alternative: No Technician Accepts

```
┌────────────────┐
│  Timeout       │
│  (e.g., 30min) │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Expand Search │
│  Radius        │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Notify More   │
│  Technicians   │
└────────┬───────┘
         │
         ▼
┌────────────────┐      No     ┌──────────────┐
│  Any Accept?   │────────────→│  Notify      │
└────────┬───────┘              │  Customer    │
         │ Yes                  │  "No Tech    │
         ▼                      │   Available" │
┌────────────────┐              └──────────────┘
│  Assign Job    │
└────────────────┘
```

---

## 4. Service Execution Flow

```
┌────────────────┐
│  Job Accepted  │
│  by Technician │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Technician    │
│  Navigates to  │
│  Location      │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Arrives at    │
│  Location      │
│  → Update      │
│  Status:       │
│  "ARRIVED"     │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Assess        │
│  Problem       │
└────────┬───────┘
         │
         ▼
┌────────────────┐      ┌──────────────┐
│  Provide Quote │─────→│  Customer    │
│  (If needed)   │      │  Approves?   │
└────────────────┘      └──────┬───────┘
                               │
                      ┌────────┴────────┐
                      │ Yes             │ No
                      ▼                 ▼
              ┌────────────┐    ┌──────────────┐
              │  Start     │    │  Negotiate   │
              │  Work      │    │  or Cancel   │
              └──────┬─────┘    └──────────────┘
                     │
                     ▼
              ┌────────────┐
              │  Update    │
              │  Status:   │
              │"IN_PROGRESS│
              └──────┬─────┘
                     │
                     ▼
              ┌────────────┐
              │  Perform   │
              │  Service   │
              └──────┬─────┘
                     │
                     ▼
              ┌────────────┐
              │  Take      │
              │  Progress  │
              │  Photos    │
              └──────┬─────┘
                     │
                     ▼
              ┌────────────┐
              │  Complete  │
              │  Work      │
              └──────┬─────┘
                     │
                     ▼
              ┌────────────┐
              │  Update    │
              │  Status:   │
              │ "COMPLETED"│
              └──────┬─────┘
                     │
                     ▼
              ┌────────────┐      ┌──────────────┐
              │  Enter     │─────→│  Customer    │
              │  Final     │      │  Confirms    │
              │  Cost      │      │  Completion  │
              └────────────┘      └──────┬───────┘
                                         │
                                         ▼
                                  ┌──────────────┐
                                  │  Proceed to  │
                                  │  Payment     │
                                  └──────────────┘
```

---

## 5. Payment Flow

```
┌────────────────┐
│  Service       │
│  Completed     │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Customer      │
│  Reviews Final │
│  Invoice       │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Select        │
│  Payment       │
│  Method        │
└────────┬───────┘
         │
         ├──────────────────┬──────────────────┐
         │                  │                  │
         ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Cash      │    │Credit Card  │    │Bank Transfer│
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │
       │                  ▼                  │
       │           ┌─────────────┐           │
       │           │  Process    │           │
       │           │  Payment    │           │
       │           │  via Gateway│           │
       │           └──────┬──────┘           │
       │                  │                  │
       │                  ▼                  │
       │           ┌─────────────┐           │
       │           │  Payment    │           │
       │           │  Successful?│           │
       │           └──────┬──────┘           │
       │                  │                  │
       │         ┌────────┴────────┐         │
       │         │ Yes             │ No      │
       │         ▼                 ▼         │
       │   ┌──────────┐    ┌──────────┐     │
       │   │Continue  │    │  Retry   │     │
       │   └────┬─────┘    │  Payment │     │
       │        │          └──────────┘     │
       └────────┴───────────────────────────┘
                │
                ▼
         ┌────────────────┐
         │  Update Payment│
         │  Status to     │
         │  "PAID"        │
         └────────┬───────┘
                  │
                  ▼
         ┌────────────────┐
         │  Generate      │
         │  Receipt       │
         └────────┬───────┘
                  │
                  ├─────────────────┬─────────────────┐
                  │                 │                 │
                  ▼                 ▼                 ▼
         ┌────────────────┐ ┌──────────────┐ ┌──────────────┐
         │  Send Receipt  │ │  Calculate   │ │  Notify      │
         │  to Customer   │ │  Platform    │ │  Technician  │
         │                │ │  Fee &       │ │  "Payment    │
         │                │ │  Technician  │ │  Received"   │
         │                │ │  Payout      │ │              │
         └────────────────┘ └──────┬───────┘ └──────────────┘
                                   │
                                   ▼
                            ┌──────────────┐
                            │  Schedule    │
                            │  Payout to   │
                            │  Technician  │
                            └──────┬───────┘
                                   │
                                   ▼
                            ┌──────────────┐
                            │  Payment     │
                            │  Complete!   │
                            └──────────────┘
```

---

## 6. Review and Rating Flow

```
┌────────────────┐
│  Payment       │
│  Complete      │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Prompt        │
│  Customer to   │
│  Leave Review  │
└────────┬───────┘
         │
         ▼
┌────────────────┐      Skip    ┌──────────────┐
│  Customer      │─────────────→│  End Process │
│  Decides       │              └──────────────┘
└────────┬───────┘
         │ Review
         ▼
┌────────────────┐
│  Rate Overall  │
│  (1-5 stars)   │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Rate Specific │
│  Aspects:      │
│  - Quality     │
│  - Punctuality │
│  - Professional│
│  - Communicate │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Write Review  │
│  Comment       │
│  (Optional)    │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Upload Photos │
│  (Optional)    │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Submit Review │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Save Review   │
│  to Database   │
└────────┬───────┘
         │
         ├─────────────────┬─────────────────┐
         │                 │                 │
         ▼                 ▼                 ▼
┌────────────────┐ ┌──────────────┐ ┌──────────────┐
│  Update        │ │  Notify      │ │  Update      │
│  Technician    │ │  Technician  │ │  Service     │
│  Rating        │ │  of New      │ │  Request     │
│  Average       │ │  Review      │ │  Status      │
└────────────────┘ └──────────────┘ └──────────────┘
         │
         ▼
┌────────────────┐
│  Review        │
│  Published!    │
└────────────────┘
```

### Technician Response to Review

```
┌────────────────┐
│  Technician    │
│  Receives      │
│  Review        │
│  Notification  │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  View Review   │
└────────┬───────┘
         │
         ▼
┌────────────────┐      Skip    ┌──────────────┐
│  Respond?      │─────────────→│  End Process │
└────────┬───────┘              └──────────────┘
         │ Yes
         ▼
┌────────────────┐
│  Write         │
│  Response      │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Submit        │
│  Response      │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Save Response │
│  to Database   │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Notify        │
│  Customer of   │
│  Response      │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Response      │
│  Published!    │
└────────────────┘
```

---

## Admin Monitoring Flow

```
┌────────────────┐
│  Admin Logs    │
│  into Web      │
│  Dashboard     │
└────────┬───────┘
         │
         ▼
┌────────────────────────┐
│  View Dashboard        │
│  - Active Requests     │
│  - Pending Payments    │
│  - User Statistics     │
│  - System Health       │
└────────┬───────────────┘
         │
         ▼
┌────────────────┐
│  Select        │
│  Management    │
│  Section       │
└────────┬───────┘
         │
         ├──────────────┬──────────────┬──────────────┐
         │              │              │              │
         ▼              ▼              ▼              ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│   Users     │ │  Requests   │ │  Payments   │ │  Reports    │
│ Management  │ │ Management  │ │ Management  │ │  & Stats    │
└──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘
       │               │               │               │
       ▼               ▼               ▼               ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│- View Users │ │- View All   │ │- Track      │ │- Generate   │
│- Edit       │ │- Filter     │ │  Payments   │ │  Reports    │
│- Suspend    │ │- Assign     │ │- Refunds    │ │- View       │
│- Approve    │ │- Cancel     │ │- Payouts    │ │  Analytics  │
│  Technicians│ │- Update     │ │             │ │- Export Data│
└─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘
```

---

## Error Handling Flows

### Connection Error

```
┌────────────────┐
│  User Action   │
└────────┬───────┘
         │
         ▼
┌────────────────┐      No      ┌──────────────┐
│  Internet      │─────────────→│  Show Error  │
│  Available?    │              │  Message     │
└────────┬───────┘              └──────┬───────┘
         │ Yes                         │
         ▼                             ▼
┌────────────────┐              ┌──────────────┐
│  Continue      │              │  Retry or    │
│  Process       │              │  Work Offline│
└────────────────┘              └──────────────┘
```

### Payment Failure

```
┌────────────────┐
│  Payment       │
│  Failed        │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Determine     │
│  Failure Reason│
└────────┬───────┘
         │
         ├──────────────────┬──────────────────┐
         │                  │                  │
         ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│Insufficient │    │  Technical  │    │   Gateway   │
│   Funds     │    │   Error     │    │   Timeout   │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │
       └──────────────────┴──────────────────┘
                          │
                          ▼
                   ┌──────────────┐
                   │  Notify User │
                   │  & Offer     │
                   │  Retry       │
                   └──────┬───────┘
                          │
                          ▼
                   ┌──────────────┐
                   │  Keep Service│
                   │  Request     │
                   │  Status as   │
                   │  "PENDING    │
                   │   PAYMENT"   │
                   └──────────────┘
```

---

## Notification Flow

```
┌────────────────┐
│  Event Trigger │
│  (Status       │
│   Change, etc.)│
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Create        │
│  Notification  │
│  Record        │
└────────┬───────┘
         │
         ├──────────────────┬──────────────────┐
         │                  │                  │
         ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  In-App     │    │    Push     │    │   Email     │
│Notification │    │Notification │    │(Optional)   │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │
       └──────────────────┴──────────────────┘
                          │
                          ▼
                   ┌──────────────┐
                   │  User        │
                   │  Receives    │
                   │  Notification│
                   └──────┬───────┘
                          │
                          ▼
                   ┌──────────────┐
                   │  User Taps   │
                   │  → Navigate  │
                   │  to Relevant │
                   │  Screen      │
                   └──────────────┘
```

---

## Status Lifecycle

### Service Request Status Flow

```
┌─────────┐
│ PENDING │ ← Initial state when customer creates request
└────┬────┘
     │
     ▼
┌──────────┐
│ ACCEPTED │ ← Technician accepts the job
└────┬─────┘
     │
     ▼
┌──────────┐
│ ARRIVED  │ ← Technician arrives at location (Optional)
└────┬─────┘
     │
     ▼
┌─────────────┐
│IN_PROGRESS  │ ← Work has started
└────┬────────┘
     │
     ▼
┌──────────┐
│COMPLETED │ ← Work is finished
└────┬─────┘
     │
     ▼
┌──────────┐
│   PAID   │ ← Payment completed (Final state)
└──────────┘

Alternative paths:
┌─────────┐     ┌───────────┐
│ANY STATE│────→│ CANCELLED │ ← Can be cancelled from any state
└─────────┘     └───────────┘
```

---

## Real-time Updates Flow

```
┌────────────────┐
│  Status Change │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Update        │
│  Database      │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│  Emit Socket   │
│  Event         │
└────────┬───────┘
         │
         ├──────────────────┬──────────────────┐
         │                  │                  │
         ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Customer   │    │ Technician  │    │    Admin    │
│    App      │    │     App     │    │     Web     │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │
       ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Update UI  │    │  Update UI  │    │  Update UI  │
│  Real-time  │    │  Real-time  │    │  Real-time  │
└─────────────┘    └─────────────┘    └─────────────┘
```

