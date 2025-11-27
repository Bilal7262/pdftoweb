# Portfolio Themes Structure

## Overview
**Project**: Next.js 15 Portfolio Themes  
**Framework**: Next.js 15.0.3 with Turbopack  
**Styling**: Tailwind CSS 3.4.15  
**Slider**: Swiper 11.1.14  
**Total Themes**: 4 (Gyni-Clinic, Medical-Help, QAW, Saif)

---

## 🏗️ Project Architecture

### Tech Stack
```json
{
  "framework": "Next.js 15.0.3",
  "react": "19.0.0-rc",
  "styling": "Tailwind CSS 3.4.15",
  "slider": "Swiper 11.1.14",
  "turbopack": true
}
```

### Directory Structure
```
portfolio-themes/
├── src/
│   ├── app/
│   │   ├── [username]/          # Dynamic routing by username
│   │   │   ├── page.js          # Theme router/selector
│   │   │   ├── bilal/           # User-specific theme
│   │   │   ├── gyni-clinic/     # Gyni-Clinic theme
│   │   │   ├── medical-help/    # Medical-Help theme
│   │   │   ├── qaw/             # QAW Personal Portfolio theme
│   │   │   └── saif/            # Saif theme
│   │   ├── layout.js
│   │   ├── globals.css
│   │   └── 404.js
│   ├── components/
│   │   ├── gyni-clinic/         # Gyni-Clinic components (12 files)
│   │   ├── medical-help/        # Medical-Help components (9 files)
│   │   ├── qaw/                 # QAW components (17 files)
│   │   └── saif/                # Saif components
│   ├── assets/
│   │   ├── gyni-clinic/         # Theme-specific assets
│   │   ├── medical-help/
│   │   ├── qaw/
│   │   └── saif/
│   └── utils/
└── public/
    └── assets/                  # Public assets (2137 files)
```

---

## 🎨 Available Themes

### 1. **Gyni-Clinic** (Pregnancy/Medical Theme)
**Route**: `/{username}/gyni-clinic`  
**Type**: Medical & Health - Pregnancy Care  
**Components**: 12

#### Components
1. **Header.js** - Navigation with logo, menu, appointment button
2. **Hero.js** - Banner with video popup
3. **About.js** - About section with images
4. **Services.js** - Services grid
5. **ContactUs.js** - Contact form section
6. **Department.js** - Departments/specialties showcase
7. **CounterSection.js** - Statistics counter
8. **CareSection.js** - Care information
9. **SafeActionSection.js** - Safety/action CTA
10. **Blog.js** - Blog posts grid
11. **ClientSection.js** - Client logos/testimonials
12. **Footer.js** - Footer with links and info

#### Features
- Preloader animation
- Video popup integration
- Responsive design
- Bootstrap 5 based
- Icon fonts (Icofont)
- Swiper slider
- Lightcase for media
- Animate.css animations

#### CSS/Assets
```
@/assets/gyni-clinic/css/
├── animate.css
├── bootstrap.min.css
├── icofont.min.css
├── lightcase.css
├── swiper.min.css
└── style.css
```

---

### 2. **Medical-Help** (Medical Services Theme)
**Route**: `/{username}/medical-help`  
**Type**: Medical Services  
**Components**: 9

#### Components
1. **Header.js** - Navigation header
2. **Banner.js** - Hero banner
3. **AboutSection.js** - About information
4. **ServicesSection.js** - Services showcase
5. **TeamSection.js** - Team members
6. **Testimonials.js** - Client testimonials
7. **BlogSection.js** - Blog posts
8. **ContactSection.js** - Contact form
9. **Footer.js** - Footer section

#### Features
- Simpler, cleaner design
- Focus on medical services
- Team showcase
- Testimonial slider
- Blog integration

---

### 3. **QAW** (Personal Portfolio Theme) ⭐
**Route**: `/{username}/qaw`  
**Type**: Personal Portfolio  
**Components**: 17 (Most comprehensive)

#### Components
1. **Header.js** - Navigation with logo
2. **HeroSection.js** - Hero banner
3. **AboutSection.js** - About with bio, skills, social links
4. **ServicesSection.js** - Services offered
5. **ClientsSection.js** - Client logos
6. **PortfolioSection.js** - Portfolio grid
7. **PortfolioDetails/** - Portfolio detail pages
8. **AwardsSection.js** - Awards & achievements
9. **TestimonialSection.js** - Client testimonials
10. **BlogSection.js** - Blog posts
11. **ContactSection.js** - Contact form
12. **Footer.js** - Footer
13. **ThemeLayout.js** - Main layout wrapper

#### Key Features
- **API Integration**: Fetches data from Laravel backend
  ```javascript
  fetch(`https://tricloud.faremit.com/api/portfolio/${username}`)
  ```
- **Dynamic Data Rendering**: All sections conditionally render based on API data
- **Comprehensive Sections**: Services, Clients, Portfolio, Awards, Testimonials, Blogs
- **Portfolio Details**: Dedicated detail pages for projects
- **Responsive**: Bootstrap-based responsive design
- **Icon Libraries**: Bootstrap Icons, FontAwesome

#### Data Structure (from API)
```javascript
{
  first_name, last_name,
  about_me,
  profile_img,
  user_name,
  email, phone,
  skills: [],
  social_accounts: [],
  services: [],
  clients: [],
  portfolios: [],
  awards: [],
  testimonials: [],
  blogs: [],
  clients_count,
  portfolios_count,
  experiences_in_years_count
}
```

#### CSS/Assets
```
@/assets/qaw/
├── plugins/
│   ├── bootstrap/
│   ├── magnific-popup/
│   ├── swiper/
│   ├── bootstrap-icons/
│   └── fontawesome/
└── css/
    └── theme.css
```

---

### 4. **Saif** (Minimal Theme)
**Route**: `/{username}/saif`  
**Type**: Minimal/Placeholder  
**Status**: Basic implementation

---

## 🔄 Theme Routing System

### Dynamic Username Routing
The application uses Next.js dynamic routing with `[username]` parameter.

#### Main Router (`/[username]/page.js`)
```javascript
// Fetches user's theme from backend
fetch('http://54.210.150.131/secure/get-theme', {
  method: 'POST',
  body: JSON.stringify({ username })
})

// Redirects to user's selected theme
router.push(`/${username}/${data.theme}`);

// Default fallback
router.push(`/${username}/qaw`);
```

### Theme Page Structure
Each theme follows this pattern:

```javascript
// app/[username]/[theme]/page.js
"use client";

import styles and components
import { useParams } from 'next/navigation';

export default function ThemePage() {
  const { username } = useParams();
  
  // Fetch user data (QAW theme)
  const [userData, setUserData] = useState(null);
  
  useEffect(() => {
    fetch(`https://tricloud.faremit.com/api/portfolio/${username}`)
      .then(res => res.json())
      .then(data => setUserData(data.user));
  }, [username]);
  
  return <ThemeLayout userData={userData} />;
}
```

---

## 📊 Theme Comparison

| Feature | Gyni-Clinic | Medical-Help | QAW | Saif |
|---------|-------------|--------------|-----|------|
| **Type** | Medical/Pregnancy | Medical Services | Personal Portfolio | Minimal |
| **Components** | 12 | 9 | 17 | 1 |
| **API Integration** | ❌ | ❌ | ✅ | ❌ |
| **Dynamic Data** | ❌ | ❌ | ✅ | ❌ |
| **Preloader** | ✅ | ❌ | ❌ | ❌ |
| **Video Popup** | ✅ | ❌ | ❌ | ❌ |
| **Portfolio Details** | ❌ | ❌ | ✅ | ❌ |
| **Awards Section** | ❌ | ❌ | ✅ | ❌ |
| **Team Section** | ❌ | ✅ | ❌ | ❌ |
| **Blog** | ✅ | ✅ | ✅ | ❌ |
| **Testimonials** | ✅ | ✅ | ✅ | ❌ |
| **Services** | ✅ | ✅ | ✅ | ❌ |
| **Contact Form** | ✅ | ✅ | ✅ | ❌ |

---

## 🎯 QAW Theme - Detailed Analysis

### ThemeLayout Component
The main layout wrapper that orchestrates all sections:

```javascript
const ThemeLayout = ({ userData }) => {
  return (
    <div>
      <Header userData={userData} />
      <main data-bs-spy="scroll" data-bs-target=".nav-box">
        <HeroSection userName={`${userData?.first_name} ${userData?.last_name}`} />
        
        <AboutSection
          bio={userData?.about_me}
          skills={userData?.skills}
          avatar={userData?.profile_img}
          socialAccounts={userData?.social_accounts}
          clients={userData?.clients_count}
          projects={userData?.portfolios_count}
          experience={userData?.experiences_in_years_count}
        />
        
        {/* Conditional rendering based on data availability */}
        {userData?.services?.length > 0 && (
          <ServicesSection services={userData.services} />
        )}
        
        {userData?.clients?.length > 0 && (
          <ClientsSection clients={userData.clients} />
        )}
        
        {userData?.portfolios?.length > 0 && (
          <PortfolioSection portfolios={userData.portfolios} userName={userData.user_name} />
        )}
        
        {userData?.awards?.length > 0 && (
          <AwardsSection awards={userData.awards} />
        )}
        
        {userData?.testimonials?.length > 0 && (
          <TestimonialSection testimonials={userData.testimonials} />
        )}
        
        {userData?.blogs?.length > 0 && (
          <BlogSection blogs={userData.blogs} />
        )}
        
        <ContactSection
          bio={userData?.about_me}
          email={userData?.email}
          phone={userData?.phone}
        />
      </main>
      <Footer />
    </div>
  );
};
```

### Section Props Mapping

#### AboutSection
- `bio` - User biography
- `skills` - Array of skills
- `avatar` - Profile image
- `socialAccounts` - Social media links
- `clients` - Total clients count
- `projects` - Total projects count
- `experience` - Years of experience

#### ServicesSection
- `services` - Array of service objects

#### ClientsSection
- `clients` - Array of client logos/names

#### PortfolioSection
- `portfolios` - Array of portfolio projects
- `userName` - For detail page links

#### AwardsSection
- `awards` - Array of awards/achievements

#### TestimonialSection
- `testimonials` - Array of testimonials

#### BlogSection
- `blogs` - Array of blog posts

#### ContactSection
- `bio` - User bio
- `email` - Contact email
- `phone` - Contact phone

---

## 🔗 Backend Integration

### API Endpoint
```
GET https://tricloud.faremit.com/api/portfolio/{username}
```

### Response Structure
```json
{
  "user": {
    "first_name": "string",
    "last_name": "string",
    "user_name": "string",
    "about_me": "string",
    "profile_img": "url",
    "email": "string",
    "phone": "string",
    "clients_count": number,
    "portfolios_count": number,
    "experiences_in_years_count": number,
    "skills": [...],
    "social_accounts": [...],
    "services": [...],
    "clients": [...],
    "portfolios": [...],
    "awards": [...],
    "testimonials": [...],
    "blogs": [...]
  }
}
```

---

## 📁 Assets Organization

### Per-Theme Assets
Each theme has its own asset directory:
- `public/assets/gyni-clinic/` - Images, CSS, JS
- `public/assets/medical-help/` - Images, CSS, JS
- `public/assets/qaw/` - Images, CSS, JS, plugins
- `public/assets/saif/` - Images, CSS, JS

### Total Assets: 2,137 files in public/assets

---

## 🚀 Development

### Running the Project
```bash
npm run dev --turbopack    # Development with Turbopack
npm run build              # Production build
npm run start              # Production server
npm run lint               # Linting
```

### Adding a New Theme

1. **Create theme directory**
   ```
   src/app/[username]/new-theme/
   ```

2. **Create page.js**
   ```javascript
   "use client";
   import components and styles
   export default function NewThemePage() { ... }
   ```

3. **Create components**
   ```
   src/components/new-theme/
   ```

4. **Add assets**
   ```
   public/assets/new-theme/
   ```

5. **Update theme router** (optional)
   Modify `/[username]/page.js` to include new theme logic

---

## 🎨 Design Patterns

### 1. **Static Themes** (Gyni-Clinic, Medical-Help)
- Hardcoded content
- No API integration
- Fixed layouts
- Preloader animations
- Best for: Demo/template purposes

### 2. **Dynamic Themes** (QAW)
- API-driven content
- Conditional rendering
- User-specific data
- Responsive to backend changes
- Best for: Production use

### 3. **Hybrid Approach** (Recommended)
- Static layout structure
- Dynamic content from API
- Fallback content for missing data
- Theme customization via backend

---

## 🔮 Future Enhancements

1. **Theme Selector UI** - Allow users to preview and select themes
2. **Theme Customization** - Color schemes, fonts, layouts from backend
3. **More Themes** - Corporate, Creative, Developer, Designer themes
4. **Theme Builder** - Visual theme editor
5. **Component Library** - Reusable components across themes
6. **Performance** - Image optimization, lazy loading
7. **SEO** - Dynamic meta tags, structured data
8. **Analytics** - Track theme usage and performance

---

## 📝 Notes

- **QAW theme** is the most production-ready with full API integration
- **Gyni-Clinic** and **Medical-Help** are template-based themes
- All themes use Next.js 15 App Router
- Responsive design across all themes
- Tailwind CSS for utility-first styling
- Swiper for carousels/sliders
