# Wireframes - FixFast

Wireframes สำหรับระบบ FixFast ทั้งหมด

## สารบัญ

1. [ภาพรวม](#ภาพรวม)
2. [Customer Mobile App Wireframes](./customer-app.md)
3. [Technical Mobile App Wireframes](./technical-app.md)
4. [Admin Web Dashboard Wireframes](./admin-web.md)
5. [Design Guidelines](#design-guidelines)

---

## ภาพรวม

เอกสารนี้แสดง wireframes สำหรับทั้ง 3 แพลตฟอร์มของระบบ FixFast:

- **Customer Mobile App** - แอปพลิเคชันสำหรับลูกค้าที่ต้องการใช้บริการช่าง
- **Technical Mobile App** - แอปพลิเคชันสำหรับช่างเทคนิคที่ให้บริการ
- **Admin Web Dashboard** - เว็บแดชบอร์ดสำหรับผู้ดูแลระบบ

## Design Principles

### 1. Mobile-First Design
- ออกแบบสำหรับมือถือเป็นหลัก
- UI/UX เน้นความง่ายและรวดเร็ว
- สามารถใช้งานด้วยนิ้วหัวแม่มือเพียงข้างเดียว

### 2. Consistent Design System
- ใช้สีและ typography ที่สอดคล้องกันทั้งระบบ
- Component reusability สูง
- Follow Material Design / iOS Human Interface Guidelines

### 3. Accessibility
- ขนาดตัวอักษรอ่านง่าย (minimum 14sp)
- Contrast ratio ตามมาตรฐาน WCAG
- Support สำหรับ screen readers

### 4. Performance
- Loading states ที่ชัดเจน
- Skeleton screens สำหรับ content loading
- Optimized image loading

## Color Palette

### Primary Colors
```
Primary Blue:   #2563EB (RGB: 37, 99, 235)
Primary Dark:   #1E40AF (RGB: 30, 64, 175)
Primary Light:  #60A5FA (RGB: 96, 165, 250)
```

### Secondary Colors
```
Success Green:  #10B981 (RGB: 16, 185, 129)
Warning Yellow: #F59E0B (RGB: 245, 158, 11)
Error Red:      #EF4444 (RGB: 239, 68, 68)
```

### Neutral Colors
```
Background:     #FFFFFF (RGB: 255, 255, 255)
Surface:        #F9FAFB (RGB: 249, 250, 251)
Border:         #E5E7EB (RGB: 229, 231, 235)
Text Primary:   #111827 (RGB: 17, 24, 39)
Text Secondary: #6B7280 (RGB: 107, 114, 128)
```

## Typography

### Mobile Apps (Flutter)
```
Heading 1:    32sp, Bold, Kanit/Prompt
Heading 2:    24sp, SemiBold, Kanit/Prompt
Heading 3:    20sp, SemiBold, Kanit/Prompt
Body Large:   16sp, Regular, Kanit/Prompt
Body:         14sp, Regular, Kanit/Prompt
Caption:      12sp, Regular, Kanit/Prompt
```

### Admin Web (Next.js)
```
Heading 1:    2rem (32px), Bold, Prompt
Heading 2:    1.5rem (24px), SemiBold, Prompt
Heading 3:    1.25rem (20px), SemiBold, Prompt
Body Large:   1rem (16px), Regular, Prompt
Body:         0.875rem (14px), Regular, Prompt
Caption:      0.75rem (12px), Regular, Prompt
```

## Spacing System

ใช้ระบบ 8-point grid:
```
XS:  4px   (0.5 units)
SM:  8px   (1 unit)
MD:  16px  (2 units)
LG:  24px  (3 units)
XL:  32px  (4 units)
2XL: 48px  (6 units)
3XL: 64px  (8 units)
```

## Icons

- **Mobile Apps**: Material Icons / Cupertino Icons
- **Admin Web**: Heroicons / Feather Icons
- Icon size: 24px (standard), 20px (small), 32px (large)

## Components

### Buttons

#### Primary Button
```
Background: Primary Blue
Text: White
Height: 48px (mobile), 40px (web)
Border Radius: 8px
Font: SemiBold 16sp
```

#### Secondary Button
```
Background: White
Text: Primary Blue
Border: 1px Primary Blue
Height: 48px (mobile), 40px (web)
Border Radius: 8px
Font: SemiBold 16sp
```

#### Text Button
```
Background: Transparent
Text: Primary Blue
Height: auto
Font: SemiBold 16sp
```

### Input Fields
```
Height: 48px (mobile), 40px (web)
Border: 1px Border Color
Border Radius: 8px
Padding: 12px
Font: Regular 14sp
Focus State: Primary Blue border
```

### Cards
```
Background: White
Border Radius: 12px
Shadow: 0 2px 8px rgba(0,0,0,0.1)
Padding: 16px
```

## Navigation Patterns

### Mobile Navigation
- **Bottom Tab Bar**: หน้าหลัก 4-5 tabs
- **Top App Bar**: แสดง title และ actions
- **Drawer**: เมนูเพิ่มเติมและการตั้งค่า

### Web Navigation
- **Sidebar Navigation**: เมนูหลักด้านซ้าย
- **Top Navigation Bar**: breadcrumbs และ user menu
- **Content Area**: main content พร้อม filters

## Screen States

### Loading State
- Show skeleton screens
- Progress indicators
- Loading text/animations

### Empty State
- Illustration
- Descriptive message
- Call-to-action button

### Error State
- Error icon/illustration
- Clear error message
- Retry button
- Support contact

### Success State
- Success icon/animation
- Confirmation message
- Next action button

## Wireframe Conventions

### Symbols Used
```
[Button]           - Interactive button
[Input Field]      - Text input
[Dropdown ▼]       - Dropdown menu
[✓] Checkbox       - Checkbox
( ) Radio          - Radio button
[=========]        - Progress bar
[🔍 Search]        - Search input
[👤 Profile]       - User profile
[⚙️ Settings]      - Settings
[← Back]           - Back navigation
[+ Add]            - Add new item
[× Close]          - Close/Cancel
```

### Layout Elements
```
┌──────────────┐   - Screen boundary
│              │
│  Content     │   - Content area
│              │
└──────────────┘

═══════════════   - Section divider

[Tab 1] [Tab 2]   - Tabs
```

---

## Wireframe Details

Wireframes แบบละเอียดสำหรับแต่ละแพลตฟอร์ม:

1. **[Customer App Wireframes](./customer-app.md)**
   - Registration & Login
   - Service Request Flow
   - Tracking & Chat
   - Payment & Review

2. **[Technical App Wireframes](./technical-app.md)**
   - Registration & Verification
   - Job Discovery
   - Job Management
   - Earnings & Profile

3. **[Admin Web Wireframes](./admin-web.md)**
   - Dashboard Overview
   - User Management
   - Request Management
   - Analytics & Reports

---

## Design Tools Recommended

- **Wireframing**: Figma, Adobe XD, Sketch
- **Prototyping**: Figma, InVision, Marvel
- **Design System**: Figma with shared components
- **Icon Libraries**: Material Icons, Heroicons
- **Mockups**: Figma, Photoshop

## Version History

- **v1.0** (2025-10-19): Initial wireframes created

---

## Notes

- Wireframes นี้เป็น low-fidelity wireframes เพื่อแสดงโครงสร้างและ flow
- สำหรับ high-fidelity mockups ควรทำใน design tool เช่น Figma
- ทุกหน้าจอควรมี responsive design สำหรับหลายขนาดหน้าจอ
- ควร user testing กับ wireframes ก่อนเริ่ม development
