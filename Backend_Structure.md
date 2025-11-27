# Portfolio Backend Structure Analysis

## Technology Stack

- **Framework**: Laravel 11.9 (PHP 8.3+)
- **Admin Panel**: Filament 3.2
- **Authentication**: Laravel Sanctum
- **Authorization**: Laratrust (Role & Permission management)
- **Image Processing**: Intervention Image 3.9
- **Database**: MySQL (with SQLite for development)
- **Deployment**: Laravel Vapor support

## Architecture Overview

The backend follows a multi-tenant portfolio system where individual users can manage their own portfolio data through both:
1. **Individual API** - For client portal consumption
2. **Admin Panel** - Filament-based admin interface
3. **Public API** - For public portfolio viewing

---

## Core Models & Features

### 👤 User Management
- **User** - Main user model with authentication
- **Role & Permission** - Laratrust-based authorization
- Custom domain support for each user

### 📱 Social & Contact
- **SocialAccount** - User's social media accounts
- **SocialAccountType** - Types of social platforms (GitHub, LinkedIn, etc.)

### 🎓 Professional Background
- **Education** - Educational qualifications
- **Experience** - Work experience entries
- **Skill** - Available skills catalog
- **UserSkill** - User's specific skills with proficiency levels

### 💼 Portfolio Content
- **Portfolio** - Main portfolio projects
- **PortfolioDetail** - Detailed information for each portfolio item
- **Service** - Services offered by the user
- **Award** - Awards and achievements
- **Blog** - Blog posts
- **Client** - Client testimonials/references
- **Testimonial** - Testimonials from clients

### 🌐 Domain Management
- **Domain** - Custom domain configuration
- Domain verification system
- SSL certificate generation (Certbot integration)
- Redirect route configuration

### 🎨 Theme System
- **Theme** - Available themes
- **Section** - Theme sections
- **Field** - Customizable fields
- **Option** - Field options

---

## API Routes Structure

### 🔓 Public Routes (`/api`)
```
GET  /api/portfolio/{username}              - Get user's portfolio
GET  /api/portfolio/{username}/{portfolio_id} - Get specific portfolio item
```

### 🔐 Individual Routes (`/individual`)

#### Authentication
```
POST /individual/register                    - User registration
POST /individual/login                       - User login
POST /individual/logout                      - User logout
POST /individual/forgot-password/request-otp - Request password reset OTP
POST /individual/reset-password              - Reset password with OTP
PUT  /individual/change-password             - Change password
```

#### Profile Management
```
PUT  /individual/profile                     - Update user profile
```

#### Social Accounts
```
GET    /individual/social-account-types      - List available social platforms
CRUD   /individual/social-accounts           - Manage social accounts
```

#### Professional Background
```
CRUD   /individual/education                 - Manage education
CRUD   /individual/experiences               - Manage work experience
GET    /individual/skills                    - List available skills
CRUD   /individual/user-skills               - Manage user's skills
```

#### Portfolio & Content
```
CRUD   /individual/portfolios                - Manage portfolio items
CRUD   /individual/portfolios/{id}/details   - Manage portfolio details
CRUD   /individual/services                  - Manage services
CRUD   /individual/awards                    - Manage awards
CRUD   /individual/blogs                     - Manage blog posts
CRUD   /individual/clients                   - Manage clients
CRUD   /individual/testimonials              - Manage testimonials
```

#### Domain Management
```
POST   /individual/domains                   - Add custom domain
GET    /individual/domains                   - List user domains
GET    /individual/domains/{id}/verify       - Verify domain ownership
DELETE /individual/domains/{id}              - Delete domain
GET    /individual/domains/status            - Check domain status
POST   /individual/domains/generate-certificate - Generate SSL certificate
```

### 🛡️ Admin Routes (`/admin`)
- Filament-based admin panel
- Accessible at `/admin` route

---

## Key Features

### 🔐 Authentication & Authorization
- **Sanctum** for API token authentication
- **Laratrust** for role-based access control
- Password reset with OTP system
- Secure password change functionality

### 🌐 Multi-Domain Support
- Users can add custom domains
- Domain verification system
- Automatic SSL certificate generation via Certbot
- Domain redirect configuration

### 🎨 Theme Customization
- Dynamic theme system with sections and fields
- Customizable options for each field
- Default values support

### 📦 File Management
- Image upload and processing via Intervention Image
- Public file storage
- Helper functions for image and domain handling

### 🔄 Observers
The system uses Laravel Observers for automated actions (8 observers in total)

---

## Helper Functions

### ImageHelper
Located at `app/Helpers/ImageHelper.php`
- Image upload handling
- Image processing and optimization

### DomainHelper
Located at `app/Helpers/DomainHelper.php`
- Domain validation
- Domain configuration utilities

---

## Database Structure

The database includes **29 migrations** covering:
- Core Laravel tables (users, cache, jobs)
- Social accounts and types
- Professional background (education, experience, skills)
- Portfolio and content (portfolios, blogs, testimonials, clients)
- Authorization (Laratrust tables)
- Domain management
- Theme system (themes, sections, fields, options)

---

## Admin Panel (Filament)

Located in `app/Filament/Resources/Admin/`
- Full CRUD interface for all models
- User-friendly admin dashboard
- Resource management for portfolio content

---

## Controllers Organization

### Individual Controllers (`app/Http/Controllers/Individual/`)
- AwardController
- AuthController
- BlogController
- ClientController
- DomainController
- EducationController
- ExperienceController
- PortfolioController
- PortfolioDetailController
- ServiceController
- SkillController
- SocialAccountController
- SocialAccountTypeController
- TestimonialController
- UserController
- UserSkillController

### API Controllers (`app/Http/Controllers/Api/`)
- PortfolioController (public portfolio viewing)

### Admin Controllers (`app/Http/Controllers/Admin/`)
- Admin-specific controllers

### Shell Controllers (`app/Http/Controllers/Shell/`)
- CertbotController (SSL certificate generation)

---

## Security Features

- CSRF protection
- API authentication via Sanctum tokens
- Role-based access control
- Secure password hashing (Bcrypt)
- OTP-based password reset
- Domain verification before SSL generation

---

## Deployment

- **Docker** support (Dockerfile & docker-compose.yml)
- **Laravel Vapor** ready
- Environment-based configuration
- Database seeding support
