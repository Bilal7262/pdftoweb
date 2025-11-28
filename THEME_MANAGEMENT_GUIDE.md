# Admin Guide: Adding New Portfolio Themes

## Overview

This guide explains how to add new themes to the portfolio system. Themes are user-facing websites that display portfolio data dynamically from the backend.

## Current Theme Architecture

### Existing Themes
1. **QAW** - Personal Portfolio (Most comprehensive, API-integrated) ⭐ **RECOMMENDED AS TEMPLATE**
2. **Gyni-Clinic** - Medical/Pregnancy theme (Static content)
3. **Medical-Help** - Medical Services theme (Static content)
4. **Saif** - Minimal theme (Placeholder)

### How Themes Work
1. User logs into **Client Portal** and manages their data (portfolios, services, skills, etc.)
2. Data is stored in the **Backend** (Laravel)
3. User's portfolio is displayed on their **Theme** (Next.js) at `/{username}/{theme-name}`
4. Theme fetches data from API: `GET /api/portfolio/{username}`

---

## Best Practices for Adding Themes

### 1. **Use QAW Theme as Template** ⭐
The QAW theme is the most production-ready and follows best practices:
- ✅ API-integrated (fetches real data)
- ✅ Conditional rendering (shows/hides sections based on data)
- ✅ Responsive design
- ✅ Comprehensive sections (Services, Portfolio, Awards, Testimonials, Blogs, etc.)

### 2. **Theme Naming Convention**
- Use lowercase with hyphens: `corporate-pro`, `creative-studio`, `developer-portfolio`
- Keep names descriptive and professional
- Avoid generic names like "theme1", "template2"

### 3. **File Structure**
```
portfolio-themes/
├── src/
│   ├── app/
│   │   └── [username]/
│   │       └── your-theme-name/
│   │           └── page.js              # Main theme page
│   ├── components/
│   │   └── your-theme-name/
│   │       ├── Header.js
│   │       ├── HeroSection.js
│   │       ├── AboutSection.js
│   │       ├── ServicesSection.js
│   │       ├── PortfolioSection.js
│   │       ├── TestimonialSection.js
│   │       ├── BlogSection.js
│   │       ├── ContactSection.js
│   │       ├── Footer.js
│   │       └── ThemeLayout.js          # Main layout wrapper
│   └── assets/
│       └── your-theme-name/
│           └── css/
│               └── theme.css
└── public/
    └── assets/
        └── your-theme-name/
            ├── images/
            ├── css/
            └── js/
```

### 4. **Component Architecture**

#### ThemeLayout.js (Main Wrapper)
This is the orchestrator that combines all sections:

```javascript
const ThemeLayout = ({ userData }) => {
  return (
    <div>
      <Header userData={userData} />
      <main>
        <HeroSection userName={`${userData?.first_name} ${userData?.last_name}`} />
        <AboutSection bio={userData?.about_me} skills={userData?.skills} />
        
        {/* Conditional Rendering - Only show if data exists */}
        {userData?.services?.length > 0 && (
          <ServicesSection services={userData.services} />
        )}
        
        {userData?.portfolios?.length > 0 && (
          <PortfolioSection portfolios={userData.portfolios} />
        )}
        
        {userData?.testimonials?.length > 0 && (
          <TestimonialSection testimonials={userData.testimonials} />
        )}
        
        <ContactSection email={userData?.email} phone={userData?.phone} />
      </main>
      <Footer />
    </div>
  );
};
```

#### page.js (Theme Entry Point)
```javascript
"use client";
import { useParams } from 'next/navigation';
import { useEffect, useState } from 'react';
import ThemeLayout from '@/components/your-theme-name/ThemeLayout';
import '@/assets/your-theme-name/css/theme.css';

export default function YourThemePage() {
  const { username } = useParams();
  const [userData, setUserData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(`http://127.0.0.1:8000/api/portfolio/${username}`)
      .then(res => res.json())
      .then(data => {
        setUserData(data.user);
        setLoading(false);
      })
      .catch(error => {
        console.error('Error fetching user data:', error);
        setLoading(false);
      });
  }, [username]);

  if (loading) return <div>Loading...</div>;
  if (!userData) return <div>User not found</div>;

  return <ThemeLayout userData={userData} />;
}
```

### 5. **API Data Structure**
Your theme will receive this data from the backend:

```javascript
{
  first_name: "John",
  last_name: "Doe",
  user_name: "johndoe",
  about_me: "Full bio text...",
  profile_img: "https://...",
  email: "john@example.com",
  phone: "+1234567890",
  
  // Counts
  clients_count: 50,
  portfolios_count: 25,
  experiences_in_years_count: 5,
  
  // Arrays
  skills: [
    { id: 1, title: "JavaScript", pivot: { rating: 9 } }
  ],
  social_accounts: [
    { id: 1, url: "https://github.com/...", social_account_type: {...} }
  ],
  services: [
    { id: 1, title: "Web Development", description: "...", icon: "..." }
  ],
  clients: [
    { id: 1, logo: "https://...", name: "Client Name" }
  ],
  portfolios: [
    { id: 1, title: "Project Name", thumbnail: "...", description: "..." }
  ],
  awards: [
    { id: 1, title: "Best Developer 2024", description: "..." }
  ],
  testimonials: [
    { id: 1, client_name: "Jane", rating: 5, description: "Great work!" }
  ],
  blogs: [
    { id: 1, title: "Blog Post", content: "...", image: "..." }
  ]
}
```

### 6. **Conditional Rendering (CRITICAL)**
Always check if data exists before rendering sections:

```javascript
// ✅ CORRECT
{userData?.services?.length > 0 && (
  <ServicesSection services={userData.services} />
)}

// ❌ WRONG - Will show empty section
<ServicesSection services={userData.services} />
```

### 7. **Responsive Design**
- Use Tailwind CSS or Bootstrap for responsive layouts
- Test on mobile, tablet, and desktop
- Ensure images are optimized and responsive

### 8. **Performance**
- Use Next.js Image component for images
- Lazy load sections below the fold
- Minimize CSS/JS bundle size
- Use Swiper for carousels (already in dependencies)

---

## Step-by-Step: Adding a New Theme

### Step 1: Create Theme Directory
```bash
cd portfolio-themes/src/app/[username]
mkdir corporate-pro
cd corporate-pro
touch page.js
```

### Step 2: Create Components Directory
```bash
cd ../../components
mkdir corporate-pro
cd corporate-pro
touch ThemeLayout.js Header.js HeroSection.js AboutSection.js ServicesSection.js PortfolioSection.js ContactSection.js Footer.js
```

### Step 3: Create Assets Directory
```bash
cd ../../../public/assets
mkdir corporate-pro
cd corporate-pro
mkdir images css js
```

### Step 4: Copy QAW Theme as Starting Point
```bash
# Copy page.js structure
cp src/app/[username]/qaw/page.js src/app/[username]/corporate-pro/page.js

# Copy components (then customize)
cp -r src/components/qaw/* src/components/corporate-pro/
```

### Step 5: Customize Theme
1. Update `page.js` to import your components
2. Modify `ThemeLayout.js` to arrange sections as needed
3. Customize each component's design
4. Add your CSS/images to `public/assets/corporate-pro/`

### Step 6: Test Theme
1. Run development server: `npm run dev --turbopack`
2. Visit: `http://localhost:3000/{test-username}/corporate-pro`
3. Verify all sections render correctly
4. Test with different data scenarios (empty arrays, missing fields)

### Step 7: Add Theme to Backend
In the Laravel backend, add the theme to the database:
```sql
INSERT INTO themes (name, slug, description) 
VALUES ('Corporate Pro', 'corporate-pro', 'Professional corporate portfolio theme');
```

### Step 8: Deploy
```bash
npm run build
npm run start
```

---

## Common Sections to Include

### Essential (Must Have)
1. **Header** - Navigation
2. **Hero** - Banner with name/title
3. **About** - Bio, skills, social links
4. **Contact** - Email, phone, contact form
5. **Footer** - Copyright, links

### Recommended
6. **Services** - What you offer
7. **Portfolio** - Projects showcase
8. **Testimonials** - Client feedback
9. **Blog** - Articles/posts

### Optional (Based on Theme Type)
10. **Awards** - Achievements
11. **Clients** - Client logos
12. **Team** - For business themes
13. **Pricing** - For business themes
14. **FAQ** - For business themes

---

## Theme Types & Recommendations

### Personal Portfolio
- Focus: Individual freelancer/developer
- Sections: About, Skills, Portfolio, Services, Testimonials, Blog, Contact
- Example: QAW theme

### Business/Corporate
- Focus: Company/agency
- Sections: About, Services, Team, Pricing, Portfolio, Clients, Testimonials, FAQ, Contact
- Style: Professional, clean, trust-building

### Creative/Designer
- Focus: Visual artists, designers
- Sections: Portfolio (prominent), About, Services, Awards, Contact
- Style: Bold, visual-heavy, unique layouts

### Developer
- Focus: Software developers
- Sections: About, Skills, Projects (GitHub integration), Blog, Contact
- Style: Minimal, code-focused, dark mode

---

## Testing Checklist

Before marking a theme as complete:

- [ ] Theme loads without errors
- [ ] All sections render with sample data
- [ ] Empty data scenarios handled gracefully (no broken sections)
- [ ] Responsive on mobile, tablet, desktop
- [ ] Images load correctly
- [ ] Links work (social media, contact)
- [ ] Forms submit (if applicable)
- [ ] Performance is acceptable (< 3s load time)
- [ ] No console errors
- [ ] Theme matches design mockup (if provided)

---

## Troubleshooting

### Theme Not Loading
- Check file path: `src/app/[username]/theme-name/page.js`
- Ensure `"use client"` directive at top of page.js
- Verify imports are correct

### Data Not Displaying
- Check API endpoint: `http://127.0.0.1:8000/api/portfolio/{username}`
- Verify userData is being set in state
- Check conditional rendering logic

### Styling Issues
- Ensure CSS is imported in page.js
- Check Tailwind config if using Tailwind
- Verify asset paths in public/assets/

### Images Not Loading
- Use absolute paths: `/assets/theme-name/images/...`
- Or use Next.js Image component with proper src

---

## Resources

- **QAW Theme Code**: `src/app/[username]/qaw/` (Best reference)
- **API Documentation**: Backend_Structure.md
- **Next.js Docs**: https://nextjs.org/docs
- **Tailwind CSS**: https://tailwindcss.com/docs
- **Swiper**: https://swiperjs.com/

---

## Support

For questions or issues:
1. Review existing themes (especially QAW)
2. Check this guide
3. Test with sample data
4. Contact development team if stuck
