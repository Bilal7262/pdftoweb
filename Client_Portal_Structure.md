# Portfolio Client Portal Structure

## Overview
**Project**: Next.js 15 Client Portal  
**Framework**: Next.js 15.0.1  
**UI Library**: shadcn/ui + Radix UI  
**Styling**: Tailwind CSS 3.4.1  
**Form Handling**: React Hook Form 7.53.1  
**HTTP Client**: Axios 1.7.7  
**Icons**: Heroicons 2.1.5 + Lucide React

---

## 🏗️ Project Architecture

### Tech Stack
```json
{
  "framework": "Next.js 15.0.1",
  "react": "19.0.0-rc",
  "ui": "shadcn/ui",
  "styling": "Tailwind CSS 3.4.1",
  "forms": "React Hook Form 7.53.1",
  "http": "Axios 1.7.7",
  "icons": "Heroicons + Lucide",
  "theme": "next-themes 0.3.0"
}
```

### Directory Structure
```
portfolio-client-portal/
├── src/
│   ├── app/
│   │   ├── (auth)/              # Authentication routes
│   │   │   ├── signin/
│   │   │   └── signup/
│   │   ├── dashboard/           # Dashboard routes
│   │   │   ├── (pages)/         # Dashboard pages
│   │   │   │   ├── home/
│   │   │   │   ├── portfolios/
│   │   │   │   ├── education/
│   │   │   │   ├── experiences/
│   │   │   │   ├── skills/
│   │   │   │   ├── testimonials/
│   │   │   │   ├── clients/
│   │   │   │   └── social-accounts/
│   │   │   └── layout.jsx
│   │   ├── layout.jsx
│   │   ├── page.jsx
│   │   └── globals.css
│   ├── components/
│   │   ├── common/              # Reusable components (9 files)
│   │   ├── dashboard/           # Dashboard components (2 files)
│   │   ├── pagesCompt/          # Page-specific components (8 files)
│   │   ├── ui/                  # shadcn/ui components (8 files)
│   │   └── theme-provider.jsx
│   ├── services/
│   │   └── api/
│   │       └── apiConfig.js     # API service layer
│   ├── lib/
│   │   └── utils.js             # Utility functions
│   └── assets/
│       └── images/
├── public/
├── components.json              # shadcn/ui config
├── tailwind.config.js
└── package.json
```

---

## 🔐 Authentication System

### API Configuration
**Base URL**: `https://tricloud.faremit.com/individual/`

#### API Service (`apiConfig.js`)
```javascript
- Axios instance with base URL
- Request interceptor for Bearer token authentication
- Response handler for success/error states
- RESTful methods: GET, POST, PUT, DELETE, PATCH, OPTIONS
```

#### Token Management
- Stored in `localStorage` as `user` object
- Auto-attached to all API requests via interceptor
- Format: `Bearer {token}`

### Authentication Routes

#### Sign In (`/signin`)
**Features**:
- Email validation (regex pattern)
- Password validation (min 6 characters)
- React Hook Form validation
- Auto-redirect to dashboard on success
- Stores user data + token in localStorage

**API Endpoint**: `POST /login`

#### Sign Up (`/signup`)
**Features**:
- User registration form
- Form validation
- API integration

**API Endpoint**: `POST /register`

---

## 📊 Dashboard System

### Layout Structure
```
Dashboard Layout (2-column)
├── SideNav (left, fixed width 256px)
│   └── NavLinks (navigation menu)
└── Main Content (right, flexible)
    └── Dynamic page content
```

### Dashboard Pages

#### 1. **Home** (`/dashboard`)
**Features**:
- Profile image upload with edit button
- User profile display (first name, about me)
- Read-only form fields:
  - First Name
  - Last Name
  - Profession (title)
  - Email
  - Phone
  - Date of Birth
  - Address
- Profile image preview

**Data Source**: `localStorage.user.user`

---

#### 2. **Portfolios** (`/dashboard/portfolios`)
**Features**:
- Portfolio grid display
- Add new portfolio (modal)
- Multi-step portfolio creation
- Image upload for portfolio thumbnail
- Skills selection (multi-select)
- Portfolio status (Draft/Active/Inactive)

**Components**:
- `PortfolioCard` - Display portfolio item
- `PortfolioModal` - Create/edit portfolio
- `PortfolioDetail` - Portfolio details form

**API Endpoints**:
- `GET /portfolios` - List user portfolios
- `POST /portfolios` - Create portfolio
- `GET /user-skills` - Get user skills for selection

**Fields**:
- Title
- Description
- Role
- Thumbnail image
- Start date
- End date
- Status (Draft/Active/Inactive)
- Associated skills

---

#### 3. **Education** (`/dashboard/education`)
**Features**:
- Education history management
- Add/edit/delete education entries

**Component**: `EducationCard`

**API Endpoint**: `CRUD /education`

**Fields**:
- Institute
- Degree
- From date
- To date
- Grade
- Description

---

#### 4. **Experiences** (`/dashboard/experiences`)
**Features**:
- Work experience management
- Add/edit/delete experience entries

**Component**: `ExperienceCard`

**API Endpoint**: `CRUD /experiences`

**Fields**:
- Company
- Role
- From date
- To date
- Description

---

#### 5. **Skills** (`/dashboard/skills`)
**Features**:
- Skills management
- Skill proficiency ratings

**Component**: `SkillCard`

**API Endpoints**:
- `GET /skills` - Available skills catalog
- `CRUD /user-skills` - User's skills

**Fields**:
- Skill (from catalog)
- Rating (proficiency level)
- Test rating
- Description

---

#### 6. **Testimonials** (`/dashboard/testimonials`)
**Features**:
- Client testimonials management
- Rating system

**Component**: `TestimonialCard`

**API Endpoint**: `CRUD /testimonials`

**Fields**:
- Client image
- Client name
- Rating (float)
- Description/feedback

---

#### 7. **Clients** (`/dashboard/clients`)
**Features**:
- Client logos/references management
- Image upload for client logos

**Component**: `ClientsCard`

**API Endpoint**: `CRUD /clients`

**Fields**:
- Logo (image)
- Client name

---

#### 8. **Social Accounts** (`/dashboard/social-accounts`)
**Features**:
- Social media profiles management
- Platform selection
- Display order
- Active/inactive status

**Component**: `socialCard`

**API Endpoints**:
- `GET /social-account-types` - Available platforms
- `CRUD /social-accounts` - User's social accounts

**Fields**:
- Social account type (GitHub, LinkedIn, etc.)
- Profile URL
- Status (active/inactive)
- Order (display order)

---

## 🧩 Components Library

### Common Components (`/components/common`)

1. **Button.jsx** - Styled button component
2. **TextInput.jsx** - Text input with validation
3. **MultilineInput.jsx** - Textarea component
4. **AppSelect.jsx** - Dropdown select
5. **MultiSelect.jsx** - Multi-select dropdown
6. **Modal.jsx** - Modal dialog
7. **NavBar.jsx** - Navigation bar
8. **PortfolioLogo.jsx** - Logo component
9. **socialCard.jsx** - Social media card

### Dashboard Components (`/components/dashboard`)

1. **SideNav.jsx** - Sidebar navigation
2. **NavLinks.jsx** - Navigation links with icons

### Page Components (`/components/pagesCompt`)

1. **ClientsCard.jsx** - Client display card
2. **EducationCard.jsx** - Education entry card
3. **ExperienceCard.jsx** - Experience entry card
4. **SkillCard.jsx** - Skill display card
5. **TestimonialCard.jsx** - Testimonial card
6. **portfolio/**
   - `PortfolioCard.jsx` - Portfolio item card
   - `PortfolioModal.jsx` - Portfolio creation modal
   - `PortfolioDetail.jsx` - Portfolio details form

### UI Components (`/components/ui`) - shadcn/ui

1. **badge.jsx** - Badge component
2. **button.jsx** - Button variants
3. **command.jsx** - Command palette
4. **dialog.jsx** - Dialog/modal
5. **multi-select.jsx** - Advanced multi-select
6. **popover.jsx** - Popover component
7. **select.jsx** - Select dropdown
8. **separator.jsx** - Visual separator

---

## 🔌 API Integration

### API Service Layer

#### Base Configuration
```javascript
BASE_URL: "https://tricloud.faremit.com/individual/"
```

#### Authentication
- Bearer token from localStorage
- Auto-attached via request interceptor

#### Response Handling
```javascript
{
  success: boolean,
  data: object,
  error: string,
  status: number
}
```

#### Available Methods
- `ApiService.get(url, params)`
- `ApiService.post(url, data)`
- `ApiService.put(url, data)`
- `ApiService.delete(url)`
- `ApiService.patch(url, data)`
- `ApiService.options(url)`

---

## 🎨 Styling & Theming

### Tailwind Configuration
- **Base Color**: Neutral
- **CSS Variables**: Disabled
- **Dark Mode**: Supported via `next-themes`

### Theme Provider
- Light/Dark mode toggle
- System preference detection
- Persistent theme selection

### Design System
- Consistent spacing
- Responsive breakpoints
- Dark mode optimized
- Glassmorphism effects
- Hover states and transitions

---

## 📱 Responsive Design

### Breakpoints
- Mobile-first approach
- Tablet: `md:` (768px)
- Desktop: `lg:` (1024px)

### Layout Adaptations
- Sidebar collapses on mobile
- Grid to column layout
- Touch-friendly buttons
- Optimized forms for mobile

---

## 🔄 Data Flow

### User Authentication Flow
```
1. User enters credentials
2. POST /login
3. Store user data + token in localStorage
4. Redirect to /dashboard
5. All subsequent requests include Bearer token
```

### Dashboard Data Flow
```
1. Component mounts
2. Fetch data from API (GET)
3. Display in UI
4. User creates/edits (POST/PUT)
5. Refresh data
6. Update UI
```

### Local Storage Structure
```javascript
{
  user: {
    token: "Bearer token",
    user: {
      first_name: "string",
      last_name: "string",
      email: "string",
      phone: "string",
      title: "string",
      dob: "date",
      address: "string",
      about_me: "text",
      profile_img: "url"
    }
  }
}
```

---

## 🛠️ Development

### Running the Project
```bash
npm run dev      # Development server
npm run build    # Production build
npm run start    # Production server
npm run lint     # ESLint
```

### Adding shadcn/ui Components
```bash
npx shadcn-ui@latest add [component-name]
```

---

## 📋 Features Summary

### ✅ Implemented Features

1. **Authentication**
   - Sign in
   - Sign up
   - Token-based auth
   - Protected routes

2. **Profile Management**
   - View profile
   - Profile image upload
   - Personal information display

3. **Portfolio Management**
   - Create portfolios
   - List portfolios
   - Portfolio cards
   - Image uploads
   - Skills association
   - Status management

4. **Professional Background**
   - Education management
   - Work experience
   - Skills with ratings

5. **Content Management**
   - Testimonials
   - Client references
   - Social media accounts

6. **UI/UX**
   - Dark mode support
   - Responsive design
   - Modal dialogs
   - Form validation
   - Loading states

---

## 🔒 Security Features

- Bearer token authentication
- Protected dashboard routes
- Form validation (client-side)
- Secure API communication (HTTPS)
- Token stored in localStorage
- Auto-logout on token expiry

---

## 🎯 Key Highlights

1. **Modern Stack**: Next.js 15 + React 19 RC
2. **Type-Safe Forms**: React Hook Form with validation
3. **Component Library**: shadcn/ui for consistent UI
4. **API Layer**: Centralized API service with interceptors
5. **Responsive**: Mobile-first design
6. **Dark Mode**: Full theme support
7. **Modular**: Well-organized component structure
8. **Scalable**: Easy to add new features/pages

---

## 📊 Dashboard Navigation

### Navigation Menu Items
Based on the dashboard structure:
- 🏠 Home
- 💼 Portfolios
- 🎓 Education
- 💻 Experiences
- ⚡ Skills
- 💬 Testimonials
- 🏢 Clients
- 🔗 Social Accounts

---

## 🚀 Future Enhancements

1. **Profile Editing** - Make profile fields editable
2. **Image Optimization** - Next.js Image optimization
3. **Pagination** - For large datasets
4. **Search & Filter** - Portfolio/experience filtering
5. **Drag & Drop** - Reorder items
6. **Rich Text Editor** - For descriptions
7. **Analytics** - Portfolio views tracking
8. **Export** - PDF resume generation
9. **Notifications** - Real-time updates
10. **Multi-language** - i18n support
