# jyotirmaya - Eye Health AI Mobile Application

## Overview

jyotirmaya is a healthcare mobile application designed to enable early detection of eye conditions (cataracts, squint, and allergies) for underserved communities. Developed in partnership with the Dhole Foundation and Galaxy Hospital, the app serves dual purposes: providing accessible eye health screening for community health camps and generating qualified leads for hospital bookings.

The application uses AI-powered image analysis to provide preliminary eye health assessments, with clear disclaimers that results are not diagnostic. It emphasizes simplicity, accessibility, and user engagement through gamification features.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- **Framework:** React with TypeScript using Vite as the build tool
- **UI Components:** Shadcn UI component library (New York style variant) built on Radix UI primitives
- **Styling:** Tailwind CSS with custom theme configuration supporting HSL color variables
- **State Management:** TanStack React Query (v5) for server state management
- **Routing:** Wouter for lightweight client-side routing
- **Form Handling:** React Hook Form with Zod validation via @hookform/resolvers

**Design Rationale:**
- Vite provides fast development experience and optimized production builds
- Shadcn UI offers accessible, customizable components suitable for healthcare applications
- Wouter chosen over React Router for smaller bundle size, critical for mobile performance
- React Query handles caching and synchronization of server state efficiently

**Page Structure:**
- **Onboarding:** User registration with consent management (GDPR/HIPAA compliance)
- **Home/Dashboard:** Quick action cards, scan history, gamification progress tracking
- **Capture:** Camera interface with guided image capture or gallery upload
- **Results:** AI analysis display with severity indicators, educational tips, and booking CTAs
- **Booking:** Hospital appointment scheduling with calendar picker
- **Profile:** Settings, consent management, scan history, multi-language support

**Client-Side Data Storage:**
- LocalStorage used for:
  - Onboarding completion status
  - Scan results history
  - Booking records
  - Scan count for gamification
- This approach allows offline functionality and rapid prototyping without backend dependencies

### Backend Architecture

**Technology Stack:**
- **Runtime:** Node.js with TypeScript
- **Framework:** Express.js for REST API
- **Development Server:** Custom Vite integration for SSR-ready development

**Current State:**
- Minimal backend implementation with route registration structure in place
- Storage interface abstraction (IStorage) with in-memory implementation (MemStorage)
- Ready for migration to persistent database storage

**Design Decisions:**
- Express chosen for simplicity and extensive ecosystem
- Storage interface allows switching from MemStorage to database implementation without changing application logic
- Separation of concerns with dedicated routes, storage, and server modules

### Data Storage Solutions

**Database Configuration:**
- **ORM:** Drizzle ORM (v0.39.1)
- **Database:** PostgreSQL via Neon serverless driver (@neondatabase/serverless)
- **Schema Location:** `shared/schema.ts` for type sharing between frontend and backend
- **Migrations:** Drizzle Kit configured with output to `./migrations` directory

**Schema Design:**
- Users table with UUID primary keys, username, and password fields
- Zod schemas generated from Drizzle tables for runtime validation
- Shared types exported for use across client and server

**Rationale:**
- Drizzle provides type-safe queries with minimal overhead
- Neon serverless offers auto-scaling PostgreSQL with edge support
- Shared schema ensures type consistency across the stack

### Authentication and Authorization

**Current Implementation:**
- Basic user schema defined with username/password fields
- Session management dependencies included (connect-pg-simple for PostgreSQL session store)
- Cookie-based sessions configured for credential handling

**Planned Approach:**
- Session-based authentication with PostgreSQL-backed session store
- Password hashing (implementation pending)
- Protected routes using middleware
- OAuth integration for Google Sign-In (dependencies included)

### External Dependencies

**Third-Party Services:**

1. **Neon Database:**
   - Serverless PostgreSQL hosting
   - Configuration via DATABASE_URL environment variable
   - Auto-scaling and branching capabilities

2. **Planned Integrations (Based on Requirements):**
   - **Galaxy Hospital CRM:** For appointment booking and lead generation
   - **AI/ML Service:** For eye condition detection (cataract, squint, allergy classification)
   - **Grad-CAM Visualization:** For heatmap generation on detected conditions
   - **Push Notification Service:** For appointment reminders

**UI/Component Libraries:**
- Radix UI primitives (30+ components for dialogs, dropdowns, tooltips, etc.)
- Embla Carousel for image galleries
- date-fns for date manipulation
- Lucide React for icons
- cmdk for command palette functionality

**Development Tools:**
- Replit-specific plugins for error overlay, cartographer, and dev banner
- TypeScript with strict mode enabled
- ESBuild for server bundling
- PostCSS with Tailwind and Autoprefixer

**Design System:**
- Custom Tailwind theme with neutral base color
- CSS variables for theming flexibility
- Font stack: DM Sans, Architects Daughter, Fira Code, Geist Mono, Inter
- Responsive design with mobile-first approach (768px breakpoint)

**Build & Deployment:**
- Development: tsx for TypeScript execution with hot reload
- Production: Vite builds frontend to `dist/public`, ESBuild bundles server to `dist`
- Static file serving integrated into Express for production
- Environment-aware configuration (NODE_ENV-based conditional logic)