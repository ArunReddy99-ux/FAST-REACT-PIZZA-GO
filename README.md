## Fast React Pizza Co

An end-to-end pizza ordering Single Page Application built with React, Redux Toolkit, React Router (data APIs), Tailwind CSS, and Vite. It showcases modern React patterns: route-based data fetching and mutations, global state management, optimistic UI updates, and a clean feature-first folder structure.

### Overview
- **What it does**: Lets users enter a name, browse a dynamic pizza menu, add items to a cart, place an order, look up an existing order by ID, and update an order (e.g., mark as priority).
- **Why it’s interesting**: Demonstrates React Router v6 loaders/actions for data fetching and form submissions, Redux Toolkit slices for domain state (user, cart), Tailwind for utility-first styling, and a clear separation between UI, features, and services.

### Features
- **Menu & details**: Fetches live menu data from a REST API.
- **Cart management**: Add, remove, and change quantities in a global cart.
- **User onboarding**: Lightweight user name capture stored in app state.
- **Order creation**: Submits orders to a backend API; shows errors gracefully.
- **Order tracking**: Search by order ID; view live order details.
- **Order updates**: Patch existing orders (e.g., priority flag).
- **Reverse geocoding**: Lookup address by coordinates to prefill delivery info.

### Tech Stack
- **React 18** for UI composition.
- **React Router DOM 6** for routing, loaders (data fetching), and actions (mutations).
- **Redux Toolkit + React Redux** for predictable state management.
- **Tailwind CSS** for styling via utility classes.
- **Vite** for fast dev server and production builds.
- **ESLint + Prettier** for code quality and formatting.

### How the application works (high-level flow)
1. User lands on Home and enters a username.
2. Navigates to the Menu where pizzas are fetched via a route loader.
3. Adds items to the cart; the cart summary stays visible in the layout footer.
4. Proceeds to Create Order, where the app can reverse-geocode an address and post the order via an action.
5. Receives an order ID and can navigate to the Order page to track it (loaded via a route loader).
6. Can update the order (e.g., set priority) via a route action that PATCHes the backend.

### Major concepts and how they’re used
- **Routing and Layout**: `src/App.jsx` defines a router with a shared `AppLayout` that renders a persistent header, a scrollable main outlet, and a cart overview.
- **Data Loading with Loaders**: Routes like `Menu` and `Order` export `loader` functions to fetch data before rendering, ensuring screens receive data on first paint.
- **Mutations with Actions**: Routes like `CreateOrder` and `UpdateOrder` export `action` functions that submit forms or perform mutations (POST/PATCH) through React Router’s data APIs.
- **Redux Toolkit Slices**: `userSlice` and `cartSlice` manage domain state, exposing actions/selectors for UI components. The store is configured in `src/store.js` and provided in `src/main.jsx`.
- **Side-effects & Services**: Network logic is colocated in `src/services/` (`apiRestaurant.js`, `apiGeocoding.js`) to keep components declarative and testable.
- **Styling with Tailwind**: Utility classes are applied directly in components. Global styles and custom component classes live in `src/index.css`.
- **Error & Loading States**: Route-level `errorElement` (`ui/Error`) and a global `Loader` in `AppLayout` handle feedback during pending and error states.

### Project structure
```text
fast-react-pizza co/
├─ index.html                     # Vite entry HTML
├─ package.json                   # Scripts and dependencies
├─ src/
│  ├─ main.jsx                    # React root, Redux Provider
│  ├─ App.jsx                     # Router + route tree (loaders/actions)
│  ├─ index.css                   # Tailwind layers & component styles
│  ├─ store.js                    # Redux Toolkit store configuration
│  ├─ services/                   # API clients
│  │  ├─ apiRestaurant.js         # Menu, order fetch/create/update
│  │  └─ apiGeocoding.js          # Reverse geocoding by coordinates
│  ├─ ui/                         # Layout and shared UI
│  │  ├─ AppLayout.jsx            # Header + Outlet + CartOverview + Loader
│  │  ├─ Header.jsx               # App header with search & username
│  │  ├─ Home.jsx                 # Landing screen and CTA
│  │  ├─ Error.jsx                # Route error boundary
│  │  ├─ Loader.jsx               # Global loading overlay
│  │  └─ LinkButton.jsx           # Reusable link/back button
│  ├─ features/
│  │  ├─ menu/
│  │  │  ├─ Menu.jsx             # Lists pizzas, uses loader
│  │  │  └─ MenuItem.jsx         # Single pizza item UI
│  │  ├─ cart/
│  │  │  ├─ Cart.jsx             # Cart page
│  │  │  ├─ CartItem.jsx         # Cart row UI
│  │  │  ├─ CartOverview.jsx     # Footer summary
│  │  │  ├─ UpdateItemQuantity.jsx / DeleteItem.jsx
│  │  │  └─ cartSlice.js         # Cart state, actions, selectors
│  │  ├─ order/
│  │  │  ├─ CreateOrder.jsx      # Form + action for POST
│  │  │  ├─ Order.jsx            # Order details + loader
│  │  │  ├─ UpdateOrder.jsx      # Action for PATCH
│  │  │  └─ SearchOrder.jsx      # ID-based search box in header
│  │  └─ user/
│  │     ├─ CreateUser.jsx       # Username capture form
│  │     ├─ Username.jsx         # Header display of current user
│  │     └─ userSlice.js         # User state (e.g., username)
│  └─ utils/
│     └─ helpers.js              # Utility helpers (formatting, etc.)
├─ tailwind.config.js             # Tailwind configuration
├─ postcss.config.js              # Tailwind/PostCSS integration
└─ vite.config.js                 # Vite config
```

### APIs and Data Flow
- **Restaurant API** (`src/services/apiRestaurant.js`):
  - `GET /menu` → menu list (used by `Menu` loader)
  - `GET /order/:id` → order details (used by `Order` loader)
  - `POST /order` → create new order (used by `CreateOrder` action)
  - `PATCH /order/:id` → update order (used by `UpdateOrder` action)
- **Geocoding API** (`src/services/apiGeocoding.js`):
  - Reverse geocoding endpoint to prefill address fields based on `latitude` and `longitude`.

Error handling is explicit: network calls verify `res.ok` and throw with friendly messages; route `errorElement` renders these messages.

### Getting started
- **Prerequisites**: Node.js 18+ and npm.

#### Install
```bash
npm install
```

#### Run in development
```bash
npm run dev
```
Open the printed local URL in your browser.

#### Build for production
```bash
npm run build
```

#### Preview production build
```bash
npm run preview
```

### Key files to explore
- `src/App.jsx`: route tree, loaders/actions integration.
- `src/store.js`: combined reducers for `user` and `cart`.
- `src/features/cart/cartSlice.js`: cart actions/selectors used across UI.
- `src/features/order/CreateOrder.jsx`: form submission with an action.
- `src/features/order/Order.jsx`: data loading with a loader.




