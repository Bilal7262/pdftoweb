# Portfolio Database Structure

## Database Overview
**Database Name**: `portfolio`  
**Engine**: MySQL 8.2.0  
**Character Set**: utf8mb4_unicode_ci  
**Total Tables**: 25

---

## 📊 Complete Table Structure

### 1. **users** - User Management
Primary table for user accounts and profiles.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| first_name | varchar(191) | YES | User's first name |
| last_name | varchar(191) | YES | User's last name |
| user_name | varchar(191) | YES | Username |
| name | varchar(191) | YES | Full name |
| profile_img | text | YES | Profile image URL |
| about_me | text | YES | User bio/description |
| email | varchar(191) | NO | Email (unique) |
| email_verified_at | timestamp | YES | Email verification timestamp |
| password | varchar(191) | NO | Hashed password |
| pass | varchar(191) | YES | Plain password (for reference) |
| phone | varchar(191) | YES | Contact number |
| title | varchar(191) | YES | Professional title |
| dob | date | YES | Date of birth |
| lat | decimal(10,7) | YES | Latitude for location |
| lng | decimal(10,7) | YES | Longitude for location |
| address | varchar(191) | YES | Physical address |
| subdomain | varchar(191) | YES | Custom subdomain |
| total_projects | int | YES | Total completed projects |
| year_of_experience | int | YES | Years of experience |
| world_wide_clients | int | YES | Number of worldwide clients |
| remember_token | varchar(100) | YES | Session token |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Indexes**: 
- PRIMARY KEY: `id`
- UNIQUE: `email`

---

### 2. **portfolios** - Portfolio Projects
Stores user's portfolio projects.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| title | varchar(191) | NO | Project title |
| description | text | NO | Project description |
| role | varchar(191) | NO | User's role in project |
| thumbnail | varchar(191) | YES | Project thumbnail image |
| start_date | date | NO | Project start date |
| end_date | date | YES | Project end date |
| status | enum | NO | Draft/Active/Inactive |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `user_id` → `users(id)` ON DELETE CASCADE

---

### 3. **portfolio_details** - Portfolio Content Details
Detailed content for each portfolio item (headings, images, videos, etc.).

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| portfolio_id | bigint UNSIGNED | NO | Foreign key to portfolios |
| title | varchar(191) | NO | Content title |
| description | text | YES | Content description |
| url | varchar(191) | YES | URL for media/links |
| type | enum | NO | heading1-5/link/video/audio/image |
| order | int | NO | Display order |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `portfolio_id` → `portfolios(id)`

---

### 4. **skills** - Available Skills Catalog
Master list of available skills.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| title | varchar(191) | NO | Skill name |
| icon | varchar(191) | YES | Skill icon |
| status | enum | NO | active/disabled |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

---

### 5. **user_skills** - User's Skills
User's specific skills with proficiency ratings.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| skill_id | bigint UNSIGNED | NO | Foreign key to skills |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| rating | int | YES | Skill proficiency (0-100) |
| test_rating | int | YES | Test/assessment rating |
| description | text | YES | Skill description |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: 
- `skill_id` → `skills(id)`
- `user_id` → `users(id)`

---

### 6. **portfolio_user_skills** - Portfolio Skills Junction
Links skills to specific portfolio projects.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| portfolio_id | bigint UNSIGNED | NO | Foreign key to portfolios |
| user_skill_id | bigint UNSIGNED | NO | Foreign key to user_skills |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: 
- `portfolio_id` → `portfolios(id)`
- `user_skill_id` → `user_skills(id)`

---

### 7. **education** - Educational Background
User's educational qualifications.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| institute | varchar(191) | NO | Institution name |
| degree | varchar(191) | NO | Degree/qualification |
| from | date | NO | Start date |
| to | date | YES | End date (null if ongoing) |
| grade | varchar(191) | YES | Grade/GPA |
| description | text | YES | Additional details |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Indexes**: `from`, `to`  
**Foreign Keys**: `user_id` → `users(id)`

---

### 8. **experiences** - Work Experience
User's professional work experience.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| company | varchar(191) | NO | Company name |
| role | varchar(191) | NO | Job title/role |
| from | date | NO | Start date |
| to | date | YES | End date (null if current) |
| description | text | YES | Job description |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Indexes**: `from`, `to`  
**Foreign Keys**: `user_id` → `users(id)`

---

### 9. **services** - Services Offered
Services provided by the user.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| title | varchar(191) | NO | Service title |
| description | text | NO | Service description |
| icon | varchar(191) | YES | Service icon |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `user_id` → `users(id)`

---

### 10. **awards** - Awards & Achievements
User's awards and recognitions.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| company_name | varchar(191) | NO | Awarding organization |
| company_logo | varchar(191) | YES | Organization logo |
| award_title | varchar(191) | NO | Award name |
| award_year | year | NO | Year received |
| description | text | YES | Award description |
| company_country | varchar(191) | NO | Organization country |
| company_city | varchar(191) | NO | Organization city |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `user_id` → `users(id)`

---

### 11. **blogs** - Blog Posts
User's blog content.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| image_url | varchar(191) | YES | Featured image |
| title | varchar(191) | NO | Blog title |
| short_description | varchar(191) | NO | Summary/excerpt |
| description | text | NO | Full blog content |
| published_at | timestamp | YES | Publication date |
| status | enum | NO | draft/published |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `user_id` → `users(id)`

---

### 12. **testimonials** - Client Testimonials
Client feedback and testimonials.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| image_url | varchar(191) | YES | Client photo |
| name | varchar(191) | NO | Client name |
| rating | float | NO | Rating (0-5) |
| description | text | NO | Testimonial text |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `user_id` → `users(id)`

---

### 13. **clients** - Client Logos/References
Client companies/organizations.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| logo | varchar(191) | NO | Client logo |
| name | varchar(191) | YES | Client name |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: `user_id` → `users(id)`

---

### 14. **social_account_types** - Social Platform Types
Available social media platforms.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| name | varchar(191) | NO | Platform name (GitHub, LinkedIn, etc.) |
| icon | varchar(191) | NO | Platform icon |
| status | enum | NO | active/disabled |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

---

### 15. **social_accounts** - User's Social Accounts
User's social media profiles.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| social_account_type_id | bigint UNSIGNED | NO | Foreign key to social_account_types |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| profile_url | varchar(191) | NO | Social profile URL |
| status | enum | NO | active/inactive |
| order | int | YES | Display order |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Foreign Keys**: 
- `social_account_type_id` → `social_account_types(id)`
- `user_id` → `users(id)`

---

### 16. **domains** - Custom Domains
User's custom domain configurations.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| user_id | bigint UNSIGNED | NO | Foreign key to users |
| domain_name | varchar(191) | NO | Domain (unique) |
| status | varchar(191) | NO | pending/verified/rejected |
| verification_token | varchar(191) | YES | Domain verification token |
| redirect_route | varchar(191) | YES | Redirect configuration |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |

**Indexes**: 
- UNIQUE: `domain_name`

**Foreign Keys**: `user_id` → `users(id)` ON DELETE CASCADE

---

### 17. **themes** - Portfolio Themes
Available portfolio themes with color schemes.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| name | varchar(191) | NO | Theme name |
| slug | varchar(191) | NO | URL-friendly identifier (unique) |
| primary_color | varchar(191) | NO | Primary color (#3498db) |
| secondary_color | varchar(191) | NO | Secondary color (#2ecc71) |
| danger_color | varchar(191) | NO | Danger color (#e74c3c) |
| warning_color | varchar(191) | NO | Warning color (#f39c12) |
| success_color | varchar(191) | NO | Success color (#2ecc71) |
| info_color | varchar(191) | NO | Info color (#2ecc71) |
| background_color | varchar(191) | NO | Background (#ffffff) |
| text_color | varchar(191) | NO | Text color (#333333) |
| border_color | varchar(191) | NO | Border color (#cccccc) |
| light_color | varchar(191) | NO | Light color (#000000) |
| dark_color | varchar(191) | NO | Dark color (#ffffff) |
| is_featured | tinyint(1) | NO | Featured flag |
| order | int | NO | Display order |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |
| deleted_at | timestamp | YES | Soft delete timestamp |

**Indexes**: 
- UNIQUE: `slug`

**Current Themes**:
1. **Pregnancy** (medical-and-health-in-pregnancy)
2. **Slim Personal Portfolio** (sliim-personal-portfolio)

---

### 18. **sections** - Theme Sections
Sections within each theme (Hero, About, Services, etc.).

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| theme_id | bigint UNSIGNED | NO | Foreign key to themes |
| parent_id | bigint UNSIGNED | YES | Parent section (for nesting) |
| name | varchar(191) | NO | Section name |
| slug | varchar(191) | NO | URL-friendly identifier |
| is_featured | tinyint(1) | NO | Featured flag |
| order | int | NO | Display order |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |
| deleted_at | timestamp | YES | Soft delete timestamp |

**Indexes**: 
- UNIQUE: `slug` + `theme_id` (composite)

**Foreign Keys**: 
- `theme_id` → `themes(id)`
- `parent_id` → `sections(id)` (self-referencing)

**Example Sections**:
- Hero, About Us, Services, Portfolio, Contact, CTA, Blog, Testimonials, Team, etc.

---

### 19. **fields** - Section Fields
Customizable fields within each section.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| section_id | bigint UNSIGNED | NO | Foreign key to sections |
| label | varchar(191) | NO | Field label |
| name | varchar(191) | NO | Field name/identifier |
| default | text | YES | Default value |
| placeholder | varchar(191) | YES | Placeholder text |
| type | enum | NO | text/textarea/number/image/file/dropdown/timestamp |
| min | int | YES | Minimum value/length |
| max | int | YES | Maximum value/length |
| ratio | varchar(10) | YES | Image aspect ratio |
| required | tinyint(1) | NO | Required flag |
| order | int | NO | Display order |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |
| deleted_at | timestamp | YES | Soft delete timestamp |

**Foreign Keys**: `section_id` → `sections(id)`

**Field Types**:
- `text` - Single-line text input
- `textarea` - Multi-line text
- `number` - Numeric input
- `image` - Image upload
- `file` - File upload
- `dropdown` - Select dropdown
- `timestamp` - Date/time picker

---

### 20. **options** - Field Options
Options for dropdown/select fields.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | bigint UNSIGNED | NO | Primary key |
| field_id | bigint UNSIGNED | NO | Foreign key to fields |
| label | varchar(191) | NO | Option label |
| value | varchar(191) | NO | Option value |
| order | int | NO | Display order |
| created_at | timestamp | YES | Creation timestamp |
| updated_at | timestamp | YES | Last update timestamp |
| deleted_at | timestamp | YES | Soft delete timestamp |

**Foreign Keys**: `field_id` → `fields(id)`

---

### 21-25. **Authorization Tables (Laratrust)**

#### **roles**
| Column | Type | Description |
|--------|------|-------------|
| id | bigint UNSIGNED | Primary key |
| name | varchar(191) | Role name (unique) |
| display_name | varchar(191) | Display name |
| description | varchar(191) | Role description |

#### **permissions**
| Column | Type | Description |
|--------|------|-------------|
| id | bigint UNSIGNED | Primary key |
| name | varchar(191) | Permission name (unique) |
| display_name | varchar(191) | Display name |
| description | varchar(191) | Permission description |

#### **role_user** (Pivot)
Links users to roles.

#### **permission_role** (Pivot)
Links permissions to roles.

#### **permission_user** (Pivot)
Links permissions directly to users.

---

## 🔗 Database Relationships

### User-Centric Relationships
```
users (1) → (∞) portfolios
users (1) → (∞) education
users (1) → (∞) experiences
users (1) → (∞) user_skills
users (1) → (∞) services
users (1) → (∞) awards
users (1) → (∞) blogs
users (1) → (∞) testimonials
users (1) → (∞) clients
users (1) → (∞) social_accounts
users (1) → (∞) domains
```

### Portfolio Relationships
```
portfolios (1) → (∞) portfolio_details
portfolios (∞) ← → (∞) user_skills (via portfolio_user_skills)
```

### Theme System Relationships
```
themes (1) → (∞) sections
sections (1) → (∞) fields
sections (1) → (∞) sections (parent-child)
fields (1) → (∞) options
```

### Social & Skills
```
skills (1) → (∞) user_skills
social_account_types (1) → (∞) social_accounts
```

---

## 📈 Key Features

### 1. **Multi-Tenancy**
- Each user has isolated data via `user_id` foreign keys
- Custom subdomains and domains per user

### 2. **Theme Customization**
- Dynamic theme system with sections and fields
- Customizable color schemes
- Field types support various content (text, images, files, dates)
- Nested sections via `parent_id`

### 3. **Portfolio Management**
- Rich portfolio items with detailed content
- Support for multiple content types (headings, images, videos, links)
- Skills association with projects

### 4. **Professional Profile**
- Education, experience, skills tracking
- Services, awards, testimonials
- Social media integration
- Client references

### 5. **Content Management**
- Blog system with draft/published status
- Image uploads and media management
- Ratings and reviews

### 6. **Domain Management**
- Custom domain support
- Domain verification system
- Subdomain allocation

---

## 🎨 Current Sample Data

### Themes
1. **Pregnancy** (medical-and-health-in-pregnancy) - Medical/health theme
2. **Slim Personal Portfolio** (sliim-personal-portfolio) - Personal portfolio theme

### Sections (26 total)
Including: Hero, Services, About Us, Contact, CTA, Portfolio, Team, Testimonials, Blogs, etc.

### Fields (99 total)
Extensive field definitions for customizing each section with proper validation, placeholders, and defaults.

---

## 🔒 Security Features

- Password hashing (bcrypt)
- Email verification support
- Role-based access control (Laratrust)
- Soft deletes for themes, sections, fields, options
- Foreign key constraints with cascade deletes
- Unique constraints on critical fields (email, domain, slug)
