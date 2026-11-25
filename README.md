# EcoSort – AI-Powered Waste Management & Community Cleaning Platform https://ecosort-v1.vercel.app/

A full-stack Next.js application that helps people manage waste responsibly, learn recycling, organize community cleaning activities, collaborate with nearby people, and earn reward points for environmental contributions.

## 🌿 Features

### AI Waste Scanner
- Upload or capture waste images
- AI classification using Google Gemini Vision
- Identifies waste type, category, recommended bin, recyclability
- Confidence scoring with verification prompts
- Scan history tracking
- Points awarded per scan (daily limit: 10)

### Eco Community
- Create and join cleaning groups
- Public and private groups
- Group discussions and announcements
- Member management with join requests
- Environmental goal tracking

### Cleaning Drives
- Organize local cleanup events
- Event registration and attendance
- Before/after evidence submission
- Organizer verification system
- Points awarded for participation

### Points & Rewards
- Gamified contribution system
- Configurable point values (admin)
- Achievement badges
- Daily/weekly challenges
- Reward redemption
- Individual and group leaderboards

### Nearby Map
- Location-based discovery
- Distance-sorted events and groups
- Manual location selection
- City search and filtering

### Recycling Guides
- Educational content (articles, guides)
- Waste segregation, recycling, composting, e-waste
- Points for completed learning activities
- Daily learning limits

### Admin Dashboard
- User management with role controls
- Evidence review and approval
- Report moderation
- Point configuration management
- Platform statistics

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15, React 19, TypeScript |
| Styling | Tailwind CSS 4 |
| Backend | Next.js API Routes (Server-side) |
| Database | Supabase (PostgreSQL) |
| Auth | Supabase Auth |
| AI | Google Gemini 2.0 Flash (Vision) |
| Icons | Lucide React |
| Notifications | React Hot Toast |
| Deployment | Vercel-ready |

---

## 📦 Installation

### Prerequisites
- Node.js 18+ and npm
- A Supabase project (free tier works)
- Google Gemini API key (for AI Scanner)
- Google Maps API key (optional, for interactive maps)

### 1. Clone & Install

```bash
cd ecosort-app
npm install
```

### 2. Set Up Supabase

1. Create a new project at [supabase.com](https://supabase.com)
2. Go to **SQL Editor** and paste the contents of `supabase/schema.sql`
3. Click **Run** to create all tables, RLS policies, and seed data
4. Go to **Authentication > Providers** and ensure Email provider is enabled
5. Go to **Settings > API** to find your credentials

### 3. Configure Environment Variables

Copy the example env file:

```bash
cp .env.local.example .env.local
```

Edit `.env.local` with your credentials:

```env
# Supabase (required)
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Google Gemini AI (required for AI Scanner)
GEMINI_API_KEY=your-gemini-api-key

# Google Maps (optional - for interactive map view)
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your-maps-api-key

# App URL
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

#### Getting API Keys

**Supabase:**
- URL & Anon Key: Settings > API > Project URL & anon/public key
- Service Role Key: Settings > API > service_role key (keep secret!)

**Google Gemini API:**
- Go to [Google AI Studio](https://aistudio.google.com/apikey)
- Create an API key
- The app uses `gemini-2.0-flash` model

**Google Maps (Optional):**
- Go to [Google Cloud Console](https://console.cloud.google.com)
- Enable Maps JavaScript API
- Create an API key
- Without this, the Nearby Map page shows a list view instead

### 4. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

### 5. Create First Admin User

1. Register a new account via the app
2. In Supabase SQL Editor, run:
```sql
UPDATE public.profiles SET role = 'admin' WHERE email = 'your@email.com';
```

---

## 🗄️ Database Schema

The database includes 20+ tables with full RLS policies:

- **profiles** – User profiles with points, stats, roles
- **groups** – Cleaning groups (public/private)
- **group_members** – Group membership
- **group_join_requests** – Private group join requests
- **group_messages** – Group discussions
- **cleaning_events** – Cleanup drive events
- **event_registrations** – Event signups
- **event_attendance** – Attendance records
- **cleanup_evidence** – Before/after evidence
- **waste_scans** – AI scan results
- **point_transactions** – Points ledger
- **rewards** / **reward_redemptions** – Rewards system
- **achievements** / **user_achievements** – Badges
- **notifications** – In-app notifications
- **learning_content** / **quiz_attempts** – Educational content
- **challenges** / **challenge_completions** – Daily/weekly challenges
- **reports** – Content reporting
- **point_config** – Admin-configurable point values

All tables have Row Level Security (RLS) enabled with proper policies.

---

## 🏗️ Project Structure

```
src/
├── app/
│   ├── api/              # Backend API routes
│   │   ├── admin/        # Admin operations
│   │   ├── events/       # Event registration, attendance, evidence
│   │   ├── groups/       # Group join, leave, messages, requests
│   │   ├── learn/        # Learning completion
│   │   ├── points/       # Points queries
│   │   ├── rewards/      # Reward redemption
│   │   └── scan/         # AI waste scanning
│   ├── auth/             # Auth pages (login, register, etc.)
│   ├── dashboard/        # Protected dashboard pages
│   │   ├── admin/        # Admin panel
│   │   ├── community/    # Groups & group detail
│   │   ├── drives/       # Events & event detail
│   │   ├── learn/        # Learning content
│   │   ├── leaderboard/  # Rankings
│   │   ├── map/          # Nearby activities
│   │   ├── notifications/# Notification center
│   │   ├── profile/      # User profile
│   │   ├── rewards/      # Rewards & challenges
│   │   └── scanner/      # AI waste scanner
│   ├── layout.tsx        # Root layout
│   └── page.tsx          # Landing page
├── components/
│   ├── layout/           # Sidebar, TopBar, MobileNav, DashboardLayout
│   └── ui/               # Reusable components (Avatar, Badge, etc.)
├── hooks/                # Custom hooks (useAuth, useLocation)
├── lib/
│   ├── supabase/         # Supabase client setup
│   ├── types.ts          # TypeScript interfaces
│   └── utils.ts          # Utility functions
└── middleware.ts          # Auth middleware
```

---

## 🔒 Security

- **Row Level Security (RLS)** on all Supabase tables
- **Server-side auth checks** in all API routes
- **Service role client** only used server-side for privileged operations
- **Points awarded server-side** only – cannot be manipulated from client
- **Duplicate prevention** for point awards using reference_id checks
- **Daily limits** on scans and learning activities
- **Admin role verification** on all admin API routes
- **Private group content** restricted to members only

---

## 🚀 Deployment (Vercel)

1. Push to GitHub
2. Import project in [Vercel](https://vercel.com)
3. Add all environment variables from `.env.local`
4. Deploy

---

## 📐 Point System

| Action | Points | Daily Limit |
|--------|--------|-------------|
| Waste Scan | 5 | 10/day |
| Participate in Cleanup | 50 | — |
| Complete Verified Drive | 100 | — |
| Organize Event | 150 | — |
| Upload Verified Evidence | 30 | — |
| Learning Activity | 10 | 5/day |
| Quiz Completion | 15 | 5/day |

All values are configurable by admins in the Admin > Settings panel.

---

## 🎨 Design

- **Primary:** Forest Green (#15803d)
- **Secondary:** Light Green (#22c55e)
- **Background:** Very Light Green (#f0fdf4)
- **Accent:** Lime Green (#a3e635)
- Rounded cards with soft shadows
- Responsive design (mobile-first)
- Bottom navigation on mobile, sidebar on desktop
- Smooth fade-in animations

---

## License

MIT
