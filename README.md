# Grocify - Grocery List Manager

Grocify ek modern grocery shopping app hai jo React Native aur Expo par build kiya gaya hai. Yeh app aapko apni grocery list ko smart tarike se manage karne ki facility deta hai - categories, quantities, aur priorities ke saath.

## Project Structure

```
grocify/
├── src/
│   ├── app/                    # Expo Router pages (file-based routing)
│   │   ├── (auth)/            # Authentication routes
│   │   │   ├── sign-in.tsx   # Sign in page
│   │   │   └── _layout.tsx   # Auth layout
│   │   ├── (tabs)/           # Tab navigation
│   │   │   ├── index.tsx     # List screen (grocery items)
│   │   │   ├── planner.tsx   # Add new items
│   │   │   ├── insights.tsx  # Analytics & stats
│   │   │   └── _layout.tsx   # Tabs layout
│   │   ├── api/              # API routes
│   │   │   └── items/        # Items CRUD endpoints
│   │   │       ├── index+api.ts      # GET all, POST new
│   │   │       ├── [id]+api.ts       # GET/PATCH/DELETE single
│   │   │       └── clear-purchased+api.ts
│   │   ├── sso-callback.tsx # Clerk callback
│   │   ├── _layout.tsx      # Root layout (Clerk provider)
│   │   └── _layout.tsx      # Global styles
│   ├── components/          # Reusable UI components
│   │   ├── list/           # List screen components
│   │   ├── planner/       # Planner components
│   │   ├── insights/      # Insights components
│   │   └── TabScreenBackground.tsx
│   ├── lib/
│   │   └── server/
│   │       └── db/
│   │           ├── client.ts    # Neon DB connection
│   │           ├── schema.ts   # DB schema (Drizzle)
│   │           └── db-actions.ts # DB operations
│   ├── store/
│   │   └── grocery-store.ts   # Zustand state management
│   └── hooks/
│       └── useSocialAuth.ts    # Custom hooks
├── app.json                  # Expo config
├── package.json               # Dependencies
├── drizzle.config.ts         # Drizzle config
├── tailwind.config.js         # Tailwind config
└── metro.config.js           # Metro bundler config
```

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | Expo SDK 55, React Native 0.83 |
| Routing | Expo Router (file-based) |
| Database | Neon PostgreSQL |
| ORM | Drizzle ORM |
| Authentication | Clerk |
| State Management | Zustand |
| UI Framework | NativeWind (Tailwind CSS) |
| Animations | React Native Reanimated |

## App Workflow

### 1. Authentication
- App shuru hoti hai Clerk sign-in screen se
- User apni email/Google/Apple se sign in karta hai
- Session token securely store hota hai

### 2. Main Screens

#### List Tab (Home)
- Yeh primary screen hai jahan grocery items dikhai dete hain
- Pending items upar dikhte hain, purchased items niche
- Har item par click kar ke purchased mark kar sakte hain
- Quantity adjust kar sakte hain
- Delete bhi kar sakte hain

#### Planner Tab (Add Items)
- Nayan grocery items add karne ka screen
- Fields: Name, Category, Quantity, Priority
- Categories: Produce, Dairy, Bakery, Pantry, Snacks
- Priorities: Low, Medium, High

#### Insights Tab (Analytics)
- Total items ki statistics
- Category-wise breakdown
- Priority-wise breakdown
- User profile (Clerk se)
- Clear purchased items button

### 3. Data Flow

```
User Action → Zustand Store → API Call → Server Handler → Drizzle ORM → Neon PostgreSQL
                ↓                                 ↓
            State Update ←←←←←←← Response ←←←←←←
```

### 4. Database Schema

```sql
grocery_items:
- id: TEXT PRIMARY KEY
- name: TEXT NOT NULL
- category: TEXT NOT NULL
- quantity: INTEGER DEFAULT 1
- purchased: BOOLEAN DEFAULT false
- priority: TEXT DEFAULT 'medium'
- updated_at: BIGINT
```

## Production Deployment

### Prerequisites
- Node.js 18+
- Neon PostgreSQL account
- Clerk account

### Environment Variables
`.env` file mein yeh variables honge:

```env
DATABASE_URL=postgresql://...
EXPO_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
```

### Build Steps

1. **Database Setup**
   ```bash
   # Schema push karein
   npm run db:push

   # Seed data (optional)
   npm run seed:grocery
   ```

2. **Development**
   ```bash
   npm install
   npx expo start
   ```

3. **Android Build**
   ```bash
   npx expo run:android
   # Ya
   npx expo prebuild && cd android && ./gradlew assembleRelease
   ```

4. **Production APK**
   ```bash
   npx expo export --platform android
   # Yaha dist folder mein bundled JS milega
   ```

### APK Structure
Production build mein:
- App bundled JavaScript apne mein shamil hota hai
- Offline work karta hai
- API calls ke liye internet zaroori hai

## Features

- ✅ User Authentication (Clerk)
- ✅ Add/Edit/Delete Grocery Items
- ✅ Category Management
- ✅ Priority System
- ✅ Mark as Purchased
- ✅ Analytics Dashboard
- ✅ Responsive UI (Dark/Light mode)
- ✅ File-based Routing

## License

MIT License