# Deliver-E Admin App

A modern administrative dashboard for managing the Deliver-E platform. Built with React 19, Vite, and Tailwind CSS.

## 🚀 Overview

Deliver-E Admin provides a comprehensive interface for managing sellers, drivers, and users, while offering real-time analytics and platform monitoring.

### Key Features
- **Dashboard Overview**: Real-time stats for sellers, drivers, and pending applications.
- **User Management**: Detailed views for managing platform users and sellers.
- **Analytics**: Visualization of new users, customer segmentation, and retention using Recharts.
- **Modern UI**: Built with Material UI components and Tailwind CSS for a responsive, clean experience.

## 🛠️ Tech Stack

- **Framework**: [React 19](https://react.dev/)
- **Build Tool**: [Vite](https://vitejs.dev/) (using [Rolldown](https://rolldown.rs/) via `rolldown-vite`)
- **Styling**: [Tailwind CSS 4](https://tailwindcss.com/), [Material UI (MUI)](https://mui.com/)
- **Routing**: [React Router 7](https://reactrouter.com/)
- **Charts**: [Recharts](https://recharts.org/)
- **Icons**: [Lucide React](https://lucide.dev/)
- **Linting**: [ESLint](https://eslint.org/)

## 📋 Requirements

- [Node.js](https://nodejs.org/) (Latest LTS recommended)
- [npm](https://www.npmjs.com/) (or yarn/pnpm)

## ⚙️ Setup & Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Admin-app
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Environment Variables**
   Create a `.env` file in the root directory. Copy from the template:
   ```bash
   cp .env.example .env
   ```
   
   Edit `.env` with your configuration:
   ```env
   # Firebase credentials (required)
   VITE_FIREBASE_API_KEY=your_key_here
   VITE_FIREBASE_AUTH_DOMAIN=your_domain_here
   VITE_FIREBASE_PROJECT_ID=your_project_here
   # ... (see .env.example for all Firebase vars)

   # API Backend URL (required for authentication)
   # Must match the backend's FRONTEND_URL setting exactly!
   VITE_API_BASE_URL=http://localhost:3000/api/v1
   ```

   **⚠️ Critical**: The `VITE_API_BASE_URL` domain must exactly match what the backend expects (see Backend Setup section below).

## 🚀 Running the App

### Prerequisites: Start the Backend API

Before running the frontend, you must start the backend Admin-API server:

```bash
cd ../Admin-api/Admin-api
npm install
cp .env.example .env
# Edit .env and ensure: FRONTEND_URL=http://localhost:5173
npm run dev
```

The backend should be running at `http://localhost:3000` before you start the frontend.

### Frontend Commands

- **Development Mode**
  Starts the Vite dev server (frontend will be at `http://localhost:5173`).
  ```bash
  npm run dev
  ```

- **Build for Production**
  Generates optimized static files in the `dist` folder.
  ```bash
  npm run build
  ```

- **Preview Production Build**
  Locally preview the production build.
  ```bash
  npm run preview
  ```

- **Linting**
  Run ESLint to check for code quality.
  ```bash
  npm run lint
  ```

## 🔐 Authentication & API Setup

This admin app authenticates users against the Admin-API:

1. **Firebase Auth** (client-side): Users sign in with email/password to Firebase
2. **Session Cookie** (server-side): Backend exchanges Firebase token for a secure session cookie
3. **Authenticated Requests**: All API calls automatically include the session cookie

### Understanding CORS & Credentials

The frontend makes authenticated API requests using `withCredentials: true`. This requires:

- ✅ Backend must send specific origin in CORS headers (not wildcard `*`)
- ✅ Frontend origin (`http://localhost:5173`) must match backend's `FRONTEND_URL` setting
- ✅ All requests automatically include session cookies

See [Backend CORS Configuration](../Admin-api/Admin-api/README.md#cors-configuration-important-for-client-apps) for detailed setup.

### Troubleshooting Authentication Errors

**Error**: `Access-Control-Allow-Origin header must not be the wildcard '*'`

**Cause**: Backend CORS origin mismatch

**Solutions**:
1. Check backend `.env` file: `FRONTEND_URL=http://localhost:5173` (exact match)
2. Check frontend `.env` file: `VITE_API_BASE_URL=http://localhost:3000/api/v1`
3. Restart backend after changing `.env`
4. Check browser DevTools → Network tab → Response headers for CORS headers

**Error**: `401 Unauthorized` or `403 Forbidden`

**Cause**: User not authenticated or session expired

**Solution**: 
1. Ensure backend has the logged-in user as an admin (see Backend README)
2. Try logging out and back in (session cookie may be expired)

## 🚀 Running the App

## 📂 Project Structure

```text
Admin-app/
├── public/          # Static assets
├── src/
│   ├── assets/      # Images, icons, and SVG files
│   ├── components/  # Reusable UI components (Sidebar, Topbar, Layout, etc.)
│   ├── pages/       # Page-level components (Dashboard, Users, SellerApplications, etc.)
│   ├── styles/      # Global and component-specific CSS
│   ├── constants.jsx # Centralized constants and configuration
│   ├── App.jsx      # Main application routing and structure
│   └── main.jsx     # Application entry point
├── index.html       # HTML template
├── vite.config.js   # Vite configuration
└── package.json     # Project dependencies and scripts
```

## 📊 Development Progress

### Dashboard
- [x] Key metrics
    - [x] Total users (sellers / drivers) - *via API integration*
    - [x] Pending applications count - *via API integration*
    - [x] Active issues count - *via API integration*
    - [ ] Active orders (today / this week)
    - [ ] Revenue & commissions
    - [ ] Failed / cancelled orders
- [x] Charts & Analytics
    - [x] User growth trends (with date range selector)
    - [x] Customer segmentation pie chart
    - [x] Performance line chart
    - [x] Retention bar chart
- [ ] Quick actions
    - [ ] Approve sellers / drivers
    - [ ] Resolve flagged orders
    - [ ] View system alerts

### User Management
#### Customers
- [x] Basic customer page with charts
    - [x] User growth visualization
    - [x] Customer count by region
    - [x] Verification status breakdown
- [ ] View detailed user profiles
- [ ] Order history per customer
- [ ] Account status management (active / suspended / banned)
- [ ] Refund history
- [ ] Fraud flags

#### Sellers
- [x] Seller list view (table & card grid toggle)
- [x] Search and filter by status
- [x] View seller mini-stats (total, active, suspended, pending)
- [x] Seller detail panel (basic info)
- [ ] Full seller profile & verification system
- [ ] Business documents verification (IDs, licenses)
- [ ] Store status management (active / paused / suspended)
- [ ] Product count per seller
- [ ] Sales & ratings tracking
- [ ] Commission level management
- [ ] Warnings & strikes system

#### Drivers / Couriers
- [x] Driver list with DataTable component
- [x] Driver analytics (growth, signups, verification status)
- [x] Courier list with DataTable component
- [ ] Personal & vehicle details management
- [ ] License & insurance uploads verification
- [ ] Availability status tracking
- [ ] Delivery performance metrics
- [ ] Earnings & payouts system
- [ ] Suspensions / deactivation workflow

### Seller & Courier Onboarding
- [x] Application list view (filterable by status)
- [x] Application review page with document viewer
- [x] Document selection and preview UI
- [x] Personal & business information display
- [x] Approval / rejection UI with feedback textarea
- [ ] Functional approval / rejection (API integration needed)
- [ ] Document verification workflow
- [ ] Re-apply flow for rejected applications
- [ ] Training completion tracking

### Order Management
- [ ] Full visibility of every order
- [ ] Order timeline (placed → accepted → picked → delivered)
- [ ] Assign / reassign drivers
- [ ] Force-cancel orders
- [ ] Handle stuck deliveries
- [ ] Partial or full refunds
- [ ] Dispute resolution

### Reports & Disputes
- [x] Issues tracking page with DataTable
- [x] Issue status indicators (pending / in review / resolved)
- [x] Filter issues by status
- [x] View issue details (basic)
- [ ] User reports (fraud, abuse, fake products)
- [ ] Order disputes management
- [ ] Delivery complaints system
- [ ] Automated abuse detection

### Product & Store Moderation
- [ ] View all products
- [ ] Approve / reject products
- [ ] Flag prohibited items
- [ ] Edit or remove listings
- [ ] Review product images & descriptions
- [ ] Category management

### Additional Features (Implemented)
- [x] Authentication system (Firebase + JWT)
- [x] Protected routes
- [x] Staff management page
    - [x] View all admins
    - [x] Add new admin
    - [x] Revoke admin access
- [x] Server status monitoring page
    - [x] Ping multiple servers
    - [x] Display uptime and response time
- [x] Analytics page with charts
- [x] Settings page
- [x] Dark/Light theme toggle
- [x] Responsive sidebar navigation
- [x] API service layer with error handling

## 🧪 Testing
- `TODO`: Implement unit and integration tests (e.g., Vitest or Jest). Currently, no automated tests are configured.

## 📄 License
- `TODO`: Define project license (e.g., MIT).
#   E a t U p  
 