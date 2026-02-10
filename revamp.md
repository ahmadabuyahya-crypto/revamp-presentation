---
marp: true
theme: gaia
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

# Nafith Frontend Core Revamp

## Why We Rewrote & What We Gained

---

## Agenda

1. Why the Revamp?
2. Tech Stack Comparison
3. Architecture & Code Organization
4. Type Safety & Code Quality
5. State Management
6. UI Framework & Theming
7. Routing & Navigation
8. Internationalization (i18n)
9. Performance Optimizations
10. Developer Experience
11. Summary

---

## 1. Why the Revamp?

The original codebase served us well, but as the platform grew, several pain points emerged:

- **Mixed paradigms** — Options API, Mixins, and Composition API coexisted with no clear standard
- **JavaScript-first** — Minimal type safety led to runtime bugs and difficult refactoring
- **Monolithic routing** — A single 1,800-line router file became hard to maintain
- **Dual UI frameworks** — PrimeVue + Bootstrap created styling conflicts and a bloated bundle
- **Tightly coupled state** — Vuex boilerplate made state management verbose and rigid
- **No lazy i18n** — All translations loaded upfront regardless of the page visited
- **~27,000 lines of code** — Accumulated technical debt with no clear module boundaries

---

## 2. Tech Stack Comparison

| Area              | Old (frontend-core)                    | New (frontend-core-revamp)                  |
| ----------------- | -------------------------------------- | ------------------------------------------- |
| **Framework**     | Vue 3.2 (mixed JS/TS)                  | Vue 3.5 (full TypeScript, strict mode)      |
| **Language**      | JavaScript (some .ts files)            | TypeScript throughout (`strict: true`)      |
| **State Mgmt**    | Vuex 4.1                               | Pinia 3.0                                   |
| **UI Library**    | PrimeVue 3.34 **+ Bootstrap 5.3**      | Vuetify 3.9 (single library)                |
| **CSS**           | 59 SCSS files + Bootstrap RTL/LTR      | Native Scoped SCSS + Vuetify theming system |
| **Routing**       | Single monolithic file (1,800+ lines)  | Feature-split route modules                 |
| **Code Patterns** | Mixins + Options API + Composition API | Composition API + `<script setup>` only     |
| **Linting**       | ESLint 8 (legacy config)               | ESLint 9 (flat config + strict TS rules)    |
| **i18n**          | All translations loaded at startup     | Lazy per-page/feature loading               |

---

## 3. Architecture & Code Organization

### Old: Flat, Role-Based Structure

```
src/
├── components/          # 106 components in one tree
│   ├── form/
│   ├── layout/
│   ├── portal/          # Deep nesting by feature
│   └── main/
├── views/               # Page views (separate from components)
├── services/            # API services (class-based)
├── store/               # Vuex modules
├── mixins/              # Shared logic via mixins
├── composition/         # Some composables (added later)
├── helpers/             # Utility functions
└── assets/sass/         # 59 SCSS files
```

### New: Feature-Driven Modular Architecture

```
src/
├── features/            # Self-contained feature modules
│   ├── auth/            # views + composables + components
│   ├── sanad_management/
│   ├── financial_details/
│   ├── employee_management/
│   └── ...
├── shared/              # Reusable across features
│   ├── ui/              # Base components (BaseTextField, BaseTable...)
│   ├── composables/     # Shared hooks (useLanguage, useNotification...)
│   ├── directives/      # Custom Vue directives
│   └── utils/           # Pure utility functions
│   ├── constants/       # Type-safe constants
│   └── interfaces/      # TypeScript interfaces
├── services/api/        # Typed API layer
├── stores/              # Pinia stores
├── layouts/             # PublicLayout, PortalLayout
├── config/              # Plugin configuration (i18n, vuetify, vee-validate)
└── locales/             # Feature-split translation files
```

### Key Benefit

Each feature is **self-contained** — its views, components, composables, and utils live together. Adding or removing a feature doesn't ripple through the entire codebase.

---

## 4. Type Safety & Code Quality

### Before (JavaScript + optional TS)

```javascript
// Old: No type safety, runtime errors possible
static request(method, endpoint, data = null, configs = null) {
    return axios[method](endpoint, data, configs)
}

// Mixins with implicit "this" bindings
export default {
    computed: {
        ...mapGetters(["getUser", "isAuthenticated"]),
        isCompanyUser() {
            return this.getUser?.type === "COMPANY"
        }
    }
}
```

### After (Strict TypeScript)

```typescript
// New: Full type safety, errors caught at compile time
export async function companyLogin(credentials: LoginCredentials): Promise<AuthResponse> {
  return apiService.post<AuthResponse>("/auth/company-login/", credentials);
}

// Composables with explicit typing
export function useAuth() {
  const authStore = useAuthStore();
  const isAuthenticated = computed<boolean>(() => authStore.isAuthenticated);
  return { isAuthenticated, companyLogin, logout };
}
```

### TypeScript Strict Mode Flags

- `noUnusedLocals` — No dead variables
- `noUnusedParameters` — No dead parameters
- `exactOptionalPropertyTypes` — Precise optional types
- `noUncheckedIndexedAccess` — Safe array/object access
- `useUnknownInCatchVariables` — Type-safe error handling
- `no-explicit-any` enforced via ESLint — Zero `any` types allowed

---

## 5. State Management: Vuex vs Pinia

### Before (Vuex — verbose boilerplate)

```javascript
// Old: mutations + actions + getters + mappers
// store/auth.js
export default {
    state: { accessToken: null, refreshToken: null, user: null },
    mutations: {
        setAccessToken(state, token) { state.accessToken = token },
        setRefreshToken(state, token) { state.refreshToken = token },
        setUser(state, user) { state.user = user },
        clearAuthData(state) { /* ... */ }
    },
    actions: {
        authenticate({ commit }, payload) { commit("setAccessToken", payload.access) },
        logout({ commit }) { commit("clearAuthData") }
    },
    getters: {
        isAuthenticated: state => !!state.accessToken,
        getUser: state => state.user
    }
}

// In component
computed: { ...mapGetters(["isAuthenticated", "getUser"]) }
this.$store.dispatch("authenticate", tokens)
```

### After (Pinia — clean & type-safe)

```typescript
// New: no mutations, no string-based dispatch, full IntelliSense
// stores/auth.ts
export const useAuthStore = defineStore("auth", () => {
  const accessToken = ref<string | null>(null);
  const isAuthenticated = computed(() => !!accessToken.value);

  function setAuthData(data: AuthResponse) {
    accessToken.value = data.access;
    refreshToken.value = data.refresh;
  }

  function clearAuthData() {
    /* ... */
  }
  return { accessToken, isAuthenticated, setAuthData, clearAuthData };
});

// In component
const { isAuthenticated } = storeToRefs(useAuthStore());
useAuthStore().setAuthData(tokens);
```

### Why Pinia Wins

- No mutations layer (direct state updates)
- Full TypeScript support with auto-complete
- No string-based dispatching — just function calls
- Devtools integration built-in
- Smaller bundle size
- Composition API native

---

## 6. UI Framework: PrimeVue + Bootstrap vs Vuetify

### Old: Two Libraries, Double the Problems

- **PrimeVue 3.34** for interactive components (tables, dialogs, forms)
- **Bootstrap 5.3** for layout grid and utilities
- **Conflicts**: Competing CSS resets, inconsistent spacing/typography
- **Two RTL stylesheets**: `bootstrap.rtl.css` + PrimeVue RTL overrides
- **Bundle bloat**: Two full component libraries shipped to users

### New: One Unified Library

- **Vuetify 3.9** — Material Design 3 system
- Built-in **grid system** (no Bootstrap needed)
- Built-in **RTL support** (automatic with locale)
- Built-in **theming** — 40+ custom color tokens (light + dark themes)
- **Consistent design language** across every component
- **Tree-shakeable** — Only used components are bundled
- **Accessibility** — WCAG compliance built into every component

### Theme Configuration (Single Source of Truth)

```typescript
// config/vuetify.ts
const lightTheme = {
  colors: {
    primary: "#2a7050",
    secondary: "#f5f5f5",
    error: "#B00020",
    success: "#4CAF50",
    // 40+ tokens...
  },
};
```

---

## 7. Routing: Monolith vs Modular

### Before: Single 1,800+ Line File

```javascript
// router/index.js — everything in one massive file
const routes = [
  { path: "/", component: Home },
  { path: "/accounts/login/individual/", ... },
  { path: "/portal/sanad-create/", children: [ /* 6 nested steps */ ] },
  { path: "/portal/sanads/", ... },
  { path: "/portal/company/employees/", ... },
  // ... 80+ more routes
]
```

### After: Feature-Split Route Modules

```typescript
// router/index.ts — clean orchestration
import publicRoutes from "./public";
import sanadManagementRoutes from "./sanadManagement";
import financialDetailsRoutes from "./financialDetails";
import employeeManagementRoutes from "./employeeManagement";
import callbackRoutes from "./callback";
import sanadApprovalRoutes from "./sanadApproval";

// Each module owns its routes
// router/sanadManagement.ts
export default [
  {
    path: "sanads",
    name: "sanadList",
    component: () => import("@/features/sanad_management/views/SanadList.vue"),
  },
  // ...
];
```

### Benefits

- Each team/feature owns its routes
- Adding a feature = adding one import
- Route guards leverage typed metadata (`requiresPermission: string[]`)
- Dynamic i18n loading per route via `meta.i18nPages`

---

## 8. Internationalization: Eager vs Lazy

### Before: Load Everything Upfront

```javascript
// All translations loaded at app startup
import ar from "@/translations/ar.json"; // Full Arabic file
import en from "@/translations/en.json"; // Full English file

const i18n = createI18n({
  messages: { ar, en }, // Entire payload in memory
});
```

### After: Lazy Per-Feature Loading

```typescript
// Only global translations loaded at startup
// Feature translations loaded on navigation

// Route metadata triggers lazy loading
{ path: "sanads", meta: { i18nPages: ["sanadManagement"] } }

// Navigation guard loads messages dynamically
router.beforeEach(async (to) => {
  if (to.meta.i18nPages) {
    await loadAndSetPageMessages(to.meta.i18nPages);
  }
});
```

### Translation File Structure

```
locales/
├── en.json              # Global strings only
├── ar.json
├── auth/en.json         # Loaded only on auth pages
├── sanadManagement/     # Loaded only on sanad pages
├── financialDetails/    # Loaded only on financial pages
└── ...                  # 15+ feature-specific locale folders
```

### Impact

- **Faster initial load** — only global strings shipped upfront
- **Smaller memory footprint** — unused translations never loaded
- **Easier maintenance** — each feature team manages its own translations
- **Scalable** — adding a new language only requires new JSON files per feature

---

## 9. API Layer & Error Handling

### Before: Class-Based, Loosely Typed

```javascript
// services/BaseService.js
class BaseService {
  static endpoint(path) {
    return `${this.appEndpoint}${path}`;
  }
  static request(method, endpoint, data = null, configs = null) {
    return axios[method](endpoint, data, configs);
  }
}
// No typed responses, no structured error handling
```

### After: Functional, Typed, Structured Errors

```typescript
// services/api/apiService.ts
const apiService = axios.create({
  baseURL: `${import.meta.env.VITE_API_BASE_URL}/api/backend/`,
  timeout: 10_000,
  withCredentials: true,
});

// Custom error class with structured information
class ApiError extends Error {
  status?: number;
  code?: string;
  errors?: Record<string, string[]>;
}

// Typed service functions
export async function getSanads(params: SanadFilters): Promise<PaginatedResponse<Sanad>> {
  return apiService.get("/sanads/", { params });
}
```

### Token Refresh with Request Queuing

```typescript
// Concurrent requests during token refresh are queued
// and replayed automatically after refresh completes
// — prevents race conditions and duplicate refresh calls
```

---

## 10. Performance Optimizations

### Manual Chunk Splitting

```typescript
// vite.config.ts — Optimized bundle strategy
manualChunks: {
  vuetify: ["vuetify"],
  charts: ["chart.js"],
  icons: ["@mdi/font"],
  "vue-ecosystem": ["vue", "vue-router", "pinia"],
  validation: ["vee-validate", "yup", "@vee-validate/rules"],
  utils: ["@vueuse/core"],
  locales: ["vue-i18n"],
}
```

### What This Means

- **Vendor caching** — Library chunks cached independently; app code updates don't invalidate vendor cache
- **Parallel loading** — Browser loads multiple smaller chunks concurrently
- **Bundle analysis** — `npm run build:analyze` generates visual bundle report
- **Lazy route loading** — Every route component loaded on demand
- **Lazy i18n** — Translation files loaded per feature on navigation

### Removed Overhead

- No more **Bootstrap CSS** (~200KB uncompressed)
- No more **core-js polyfills** (modern browsers only)
- No more **@vitejs/plugin-legacy** (dropped IE/ES5 support)
- Single UI library instead of two

---

## 11. Developer Experience

| Aspect               | Old                                    | New                                        |
| -------------------- | -------------------------------------- | ------------------------------------------ |
| **IDE Support**      | Limited (JS, no strict types)          | Full IntelliSense, auto-imports (TS)       |
| **Refactoring**      | Risky (no compiler checks)             | Safe (TypeScript catches breaking changes) |
| **Component API**    | Mixed (Options + Composition + Mixins) | Single standard (`<script setup>`)         |
| **Finding Code**     | Search through flat folders            | Navigate by feature module                 |
| **Adding a Feature** | Touch 5+ directories                   | Create one feature folder                  |
| **State Management** | `this.$store.dispatch("string")`       | `useStore().method()` with types           |
| **Error Discovery**  | Runtime (in browser)                   | Compile time (in editor)                   |

---

## 12. Summary

### What We Achieved

**Reliability** — Full TypeScript with strict mode catches bugs before they reach users

**Maintainability** — Feature-driven architecture makes the codebase navigable and modular

**Performance** — Lazy loading (routes + i18n), chunk splitting, single UI library, no polyfills

**Consistency** — One component API, one UI library, one state management pattern

**Developer Velocity** — IDE auto-complete, type-safe refactoring, clear module boundaries

**Scalability** — Adding features is additive, not invasive; each module is self-contained

**Less Code, More Value** — Massive reduction in lines of code while maintaining all functionality

---

## Thank You

Questions?
