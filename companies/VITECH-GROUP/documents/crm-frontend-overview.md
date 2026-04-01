# CRM Frontend — Project Overview

> **Version**: 3.66.2  
> **Framework**: Next.js 16 · React 19 · TypeScript 5.8  
> **Data Layer**: Apollo Client 3 + GraphQL (codegen)

---

## Mục lục

- [1. Tổng quan](#1-tổng-quan)
- [2. Tech Stack](#2-tech-stack)
- [3. Cấu trúc dự án](#3-cấu-trúc-dự-án)
- [4. Luồng hoạt động chính](#4-luồng-hoạt-động-chính)
  - [4.1 Provider Hierarchy](#41-provider-hierarchy)
  - [4.2 Authentication Flow](#42-authentication-flow)
  - [4.3 Data Flow (GraphQL)](#43-data-flow-graphql)
  - [4.4 Metadata-Driven Architecture](#44-metadata-driven-architecture)
  - [4.5 Internationalization (i18n)](#45-internationalization-i18n)
- [5. Quy tắc code](#5-quy-tắc-code)
  - [5.1 ESLint](#51-eslint)
  - [5.2 TypeScript](#52-typescript)
  - [5.3 Commit Convention](#53-commit-convention)
  - [5.4 Validation](#54-validation)
- [6. Components tái sử dụng](#6-components-tái-sử-dụng)
  - [6.1 Shadcn UI Primitives](#61-shadcn-ui-primitives)
  - [6.2 Custom UI Components](#62-custom-ui-components)
  - [6.3 Core UI Components](#63-core-ui-components)
  - [6.4 Data Grid System](#64-data-grid-system)
  - [6.5 Module-specific Components](#65-module-specific-components)
- [7. Contexts / Providers](#7-contexts--providers)
- [8. Templates / Page Layout](#8-templates--page-layout)
- [9. Services Layer](#9-services-layer)
- [10. Hooks](#10-hooks)
- [11. Key Patterns](#11-key-patterns)
- [12. Scripts](#12-scripts)
- [13. Deployment](#13-deployment)

---

## 1. Tổng quan

Đây là ứng dụng **CRM (Customer Relationship Management)** frontend, quản lý khách hàng, leads, đơn hàng, hoạt động bán hàng, KPI, báo cáo, chat, tasks, và nhiều module nghiệp vụ khác. Dự án được xây dựng trên **Next.js App Router** với kiến trúc **metadata-driven**, cho phép tạo objects/fields/views động từ backend.

### Các module chính

| Module                | Mô tả                                                               |
| --------------------- | ------------------------------------------------------------------- |
| **Customers**         | Quản lý khách hàng (đã mua, chưa mua, chăm sóc)                     |
| **Leads**             | Quản lý leads, pipeline                                             |
| **Objects**           | Dynamic objects (metadata-driven) — contacts, companies, deals, ... |
| **Order Management**  | Quản lý đơn hàng, sản phẩm                                          |
| **Sales Activities**  | Hoạt động bán hàng                                                  |
| **Report Management** | Báo cáo, analytics                                                  |
| **KPIs**              | Dashboard KPI                                                       |
| **Work Management**   | Quản lý công việc, tasks                                            |
| **Chats**             | Messaging, chat                                                     |
| **Facebook**          | Tích hợp Facebook                                                   |
| **Workflow**          | Workflow automation                                                 |
| **Settings**          | Cài đặt hệ thống, workspace, roles, integrations                    |

---

## 2. Tech Stack

### Core

| Nhóm                  | Thư viện                                                                   |
| --------------------- | -------------------------------------------------------------------------- |
| **Framework**         | Next.js 16 (App Router), React 19, TypeScript 5.8                          |
| **Data Fetching**     | Apollo Client 3 + GraphQL, `@graphql-codegen` (auto-gen types & hooks)     |
| **State Management**  | React Context API (~28 providers), không dùng Redux                        |
| **Form & Validation** | Formik + Zod (qua `zod-formik-adapter`)                                    |
| **Styling**           | Tailwind CSS 3, `tailwind-merge`, `class-variance-authority` (CVA), `clsx` |

### UI

| Nhóm                 | Thư viện                                                  |
| -------------------- | --------------------------------------------------------- |
| **UI Primitives**    | Radix UI + Shadcn UI                                      |
| **Icons**            | Lucide React, Tabler Icons, React Icons                   |
| **Table/Grid**       | `react-data-grid` + custom grid system                    |
| **Calendar**         | FullCalendar, `react-day-picker`                          |
| **Charts**           | ApexCharts (`react-apexcharts`), Recharts                 |
| **Drag & Drop**      | `@dnd-kit/core`, `@dnd-kit/sortable`, `@hello-pangea/dnd` |
| **Rich Text Editor** | BlockNote (`@blocknote/core`), Jodit (`jodit-react`)      |
| **Command Palette**  | `cmdk`                                                    |
| **Carousel/Slider**  | Swiper                                                    |
| **Drawer**           | Vaul                                                      |
| **Toast**            | Sonner                                                    |

### Infrastructure

| Nhóm               | Thư viện                                                  |
| ------------------ | --------------------------------------------------------- |
| **i18n**           | `next-intl`                                               |
| **Auth**           | JWT (`jwt-decode`), `js-cookie`                           |
| **Real-time**      | Firebase (notifications, thay thế GraphQL subscriptions)  |
| **Monitoring**     | Sentry (`@sentry/nextjs`), PostHog (`posthog-js`)         |
| **Logging**        | Pino, Pino Pretty                                         |
| **Security**       | Arcjet (`@arcjet/next`)                                   |
| **Env Validation** | `@t3-oss/env-nextjs`                                      |
| **Date/Time**      | `date-fns`, `date-fns-tz`, Luxon                          |
| **Utilities**      | `lodash-es`, `deep-equal`, `pluralize`, `transliteration` |
| **File Export**    | `xlsx`, `xlsx-js-style`, `file-saver`, `json-2-csv`       |
| **Phone**          | `libphonenumber-js`                                       |
| **Virtualization** | `@tanstack/react-virtual`                                 |
| **Markdown**       | `@uiw/react-markdown-preview`                             |

### Dev & Testing

| Nhóm                 | Thư viện                                                         |
| -------------------- | ---------------------------------------------------------------- |
| **Testing**          | Vitest (unit), Playwright (E2E), Testing Library, Percy (visual) |
| **Linting**          | ESLint (`@antfu/eslint-config`), JSX A11y                        |
| **Git Hooks**        | Husky + lint-staged                                              |
| **Commits**          | Commitlint (conventional commits), Commitizen                    |
| **Release**          | semantic-release                                                 |
| **Storybook**        | Storybook 8                                                      |
| **Bundle Analysis**  | `@next/bundle-analyzer`                                          |
| **Monitoring (E2E)** | Checkly                                                          |

---

## 3. Cấu trúc dự án

```
crm-frontend/
├── public/ # Static assets
│ ├── firebase-messaging-sw.js # Firebase service worker
│ └── assets/images/ # Hình ảnh
│
├── i18n/
│ └── request.ts # next-intl request config
│
├── tests/
│ ├── e2e/ # Playwright E2E tests
│ └── integration/ # Integration tests
│
├── src/
│ ├── app/ # ★ Next.js App Router
│ │ ├── [locale]/ # Dynamic locale route
│ │ │ ├── (auth)/ # Auth routes (public)
│ │ │ │ ├── sign-in/
│ │ │ │ ├── forgot-password/
│ │ │ │ ├── reset-password/
│ │ │ │ ├── verify/
│ │ │ │ └── create/ # Onboarding (workspace/profile)
│ │ │ ├── (onboarding)/ # Onboarding flow
│ │ │ ├── (protected)/ # Protected routes (cần auth)
│ │ │ │ ├── (components)/ # Shared protected components
│ │ │ │ ├── customers/
│ │ │ │ ├── leads/
│ │ │ │ ├── objects/ # Dynamic metadata objects
│ │ │ │ ├── chats/
│ │ │ │ ├── order-management/
│ │ │ │ ├── sales-activities/
│ │ │ │ ├── report-management/
│ │ │ │ ├── kpis/
│ │ │ │ ├── work-management/
│ │ │ │ ├── facebook/
│ │ │ │ ├── workflow/
│ │ │ │ ├── settings/
│ │ │ │ ├── settings-news/
│ │ │ │ ├── layout.tsx # Protected layout (auth guard + providers)
│ │ │ │ └── page.tsx # Home page
│ │ │ ├── layout.tsx # Root layout (all providers)
│ │ │ └── ClientLayout.tsx
│ │ ├── graphql/ # GraphQL proxy API route
│ │ ├── rest/ # REST proxy API route
│ │ ├── metadata/ # Metadata API route
│ │ ├── files/ # File handling route
│ │ ├── client-config/ # Client config route
│ │ ├── revalidate/ # Cache revalidation route
│ │ ├── global-error.tsx
│ │ ├── robots.ts
│ │ └── sitemap.ts
│ │
│ ├── components/ # ★ UI Components (tất cả)
│ │ ├── shadcn-ui/ # Layer 1: Shadcn primitives
│ │ ├── ui/ # Layer 2: Custom reusable UI
│ │ ├── core-ui/ # Layer 3: Business-level UI
│ │ ├── react-table-grid-custom/ # Layer 4: Custom data grid
│ │ ├── react-table-grid-split-custom/
│ │ ├── filter/ # Filter system
│ │ ├── modal/ # Modal system
│ │ ├── searchable-select/ # Searchable select
│ │ ├── selects-scroll/ # Infinite scroll selects
│ │ ├── design/ # Design system components
│ │ ├── loadings/ # Loading/skeleton components
│ │ ├── table-actions/ # Table action buttons
│ │ ├── button-actions/ # Action buttons
│ │ ├── button-tab/ # Tab buttons
│ │ ├── tags/ # Tag components
│ │ ├── custom-fields/ # Dynamic field components
│ │ ├── import-data/ # Data import UI
│ │ ├── object-view/ # Object view components
│ │ ├── conversion/ # Conversion components
│ │ ├── icons.tsx # Icon definitions
│ │ ├── CellDisplayValue.tsx # Cell value renderer
│ │ ├── analytics/ # PostHog analytics
│ │ ├── auth/ # Auth components
│ │ ├── calendar/ # Calendar views
│ │ ├── chatbot/ # Chatbot widget
│ │ ├── chats/ # Chat module components
│ │ ├── customers/ # Customer module components
│ │ ├── dashboard/ # Dashboard components
│ │ ├── departments/ # Department components
│ │ ├── employees/ # Employee components
│ │ ├── order-management/ # Order module components
│ │ ├── report-management/ # Report module components
│ │ ├── sales-activities/ # Sales activity components
│ │ ├── settings/ # Settings components
│ │ ├── tasks/ # Task components
│ │ ├── theme/ # Theme provider
│ │ └── whitewave/ # Whitewave components
│ │
│ ├── services/ # ★ API Layer
│ │ ├── graphql/ # GraphQL queries & mutations
│ │ │ ├── auth/
│ │ │ ├── customers/
│ │ │ ├── leads/
│ │ │ ├── orders/
│ │ │ ├── products/
│ │ │ ├── people/
│ │ │ ├── dashboard/
│ │ │ ├── reports/
│ │ │ ├── chats/
│ │ │ ├── tags/
│ │ │ ├── task/
│ │ │ ├── teams/
│ │ │ ├── roles/
│ │ │ ├── department/
│ │ │ ├── employees/
│ │ │ ├── notification/
│ │ │ ├── object-metadata/
│ │ │ ├── views/
│ │ │ ├── workspace/
│ │ │ ├── workspace-member/
│ │ │ ├── settings-news/
│ │ │ ├── ... (35+ domains)
│ │ │ └── timeline-calendar-events/
│ │ ├── client/ # Client-side service functions
│ │ │ ├── auth/
│ │ │ ├── metadata/
│ │ │ ├── object/
│ │ │ ├── order/
│ │ │ ├── workspace/
│ │ │ ├── member/
│ │ │ ├── accounts/
│ │ │ └── default/
│ │ └── server/ # Server-side service functions (SSR)
│ │ ├── get-current-user.ts
│ │ ├── get-metadata-object-items.ts
│ │ ├── get-widget-agent.ts
│ │ ├── apollo-server-query.ts
│ │ └── apollo-server-mutate.ts
│ │
│ ├── contexts/ # ★ React Context Providers (~28)
│ │ ├── auth-provider.tsx
│ │ ├── apollo-provider.tsx
│ │ ├── apollo-core-provider.tsx
│ │ ├── appearance-provider.tsx
│ │ ├── client-config-provider.tsx
│ │ ├── confirm-modal-provider.tsx
│ │ ├── global-modal-provider.tsx
│ │ ├── notification-provider.tsx
│ │ ├── current-view-provider.tsx
│ │ ├── favorite-provider.tsx
│ │ ├── sidebar-toggle-provider.tsx
│ │ ├── ai-suggestion-provider.tsx
│ │ ├── widget-agent-provider.tsx
│ │ ├── form-fields-provider.tsx
│ │ ├── views-provider.tsx
│ │ ├── report-filter-provider.tsx
│ │ ├── apply-record-filter-provider.tsx
│ │ ├── object-items-provider.tsx
│ │ ├── ... (và nhiều providers khác)
│ │ └── next-intl-client-custom-provider.tsx
│ │
│ ├── hooks/ # ★ Custom Hooks (30+ folders)
│ │ ├── graphql/ # GraphQL-related hooks
│ │ ├── client/ # Client-side hooks
│ │ ├── object/ # Object hooks
│ │ ├── object-metadata/ # Metadata hooks
│ │ ├── object-record/ # Record CRUD hooks
│ │ ├── views/ # View hooks
│ │ ├── fields/ # Field hooks
│ │ ├── customers/ # Customer hooks
│ │ ├── leads/ # Lead hooks
│ │ ├── order/ # Order hooks
│ │ ├── products/ # Product hooks
│ │ ├── employees/ # Employee hooks
│ │ ├── departments/ # Department hooks
│ │ ├── tags/ # Tag hooks
│ │ ├── roles/ # Role hooks
│ │ ├── chats/ # Chat hooks
│ │ ├── favorites/ # Favorite hooks
│ │ ├── kpi/ # KPI hooks
│ │ ├── filter/ # Filter hooks
│ │ ├── cache/ # Cache hooks
│ │ ├── advanced/ # Advanced hooks
│ │ ├── integrations/
│ │ ├── reminder/
│ │ ├── social-messages/
│ │ ├── multiple-objects/
│ │ ├── workspace/
│ │ ├── lab/
│ │ └── default-settings/
│ │
│ ├── configs/ # ★ App Configurations
│ │ ├── apollo-client.ts # Apollo Client setup
│ │ ├── apollo-client-metadata.ts # Metadata Apollo Client
│ │ ├── router-path.ts # Route path constants
│ │ ├── firebase.ts # Firebase config
│ │ ├── appearance.ts # UI appearance config
│ │ ├── appearance-modules.ts # Module appearance
│ │ ├── sidebar-admin.ts # Admin sidebar config
│ │ ├── sidebar-icon-mapping.ts # Sidebar icons
│ │ ├── dashboard.ts # Dashboard config
│ │ ├── default-fields.ts # Default field configs
│ │ ├── columns/ # Column definitions
│ │ ├── options/ # Select options
│ │ ├── imports/ # Import configs
│ │ ├── fake-data.ts # Mock data
│ │ ├── provider.ts
│ │ ├── locales.ts
│ │ ├── settings-tabs.tsx
│ │ └── ...
│ │
│ ├── constants/ # ★ App Constants (60+)
│ │ ├── graphql.ts # GraphQL endpoint
│ │ ├── auth.ts # Auth constants
│ │ ├── app.ts # App constants
│ │ ├── firebase.ts # Firebase constants
│ │ ├── object.ts # Object constants
│ │ ├── notification.ts
│ │ ├── colors.ts
│ │ └── ... (field types, filters, settings, ...)
│ │
│ ├── types/ # ★ TypeScript Types
│ │ ├── auth/
│ │ ├── graphql/
│ │ ├── fields/
│ │ ├── filter/
│ │ ├── views/
│ │ ├── cache/
│ │ ├── settings/
│ │ ├── modal/
│ │ ├── sidebar.ts
│ │ ├── default.ts
│ │ └── ... (domain types)
│ │
│ ├── utils/ # ★ Utility Functions
│ │ ├── cn.ts # className merge (clsx + tailwind-merge)
│ │ ├── date-utils.ts # Date utilities
│ │ ├── format-number.ts # Number formatting
│ │ ├── capitalize.ts
│ │ ├── check-permission.ts # Permission checking
│ │ ├── clean-input.ts # Input sanitization
│ │ ├── exportToExcel.ts # Excel export
│ │ ├── cookie/ # Cookie utilities
│ │ ├── date/ # Date utilities
│ │ ├── fields/ # Field utilities
│ │ ├── filter/ # Filter utilities
│ │ ├── graphql/ # GraphQL utilities
│ │ ├── record/ # Record utilities
│ │ ├── view/ # View utilities
│ │ ├── server/ # Server-only utilities
│ │ ├── validation-schemas/ # Validation schemas
│ │ ├── kanban.ts # Kanban utilities
│ │ ├── kpi-data-transformers.ts # KPI data transformers
│ │ └── ... (100+ utility files)
│ │
│ ├── helpers/ # ★ Helper Functions
│ │ ├── apollo.ts # Apollo Client init
│ │ ├── apollo-partial.ts # Apollo link configs
│ │ ├── apollo-server.ts # Server-side Apollo
│ │ ├── middleware.ts # Middleware helpers
│ │ ├── filter/ # Filter helpers
│ │ ├── kpi-helpers/ # KPI helpers
│ │ ├── object-metadata/ # Metadata helpers
│ │ ├── order-helpers/ # Order helpers
│ │ ├── accounts/ # Account helpers
│ │ ├── operation-signatures/
│ │ └── ...
│ │
│ ├── validations/ # ★ Zod Validation Schemas
│ │ ├── SignInValidation.ts
│ │ ├── lead-schema.ts
│ │ ├── field-metadata-item-schema.ts
│ │ ├── organization-unit-form-schema.ts
│ │ ├── currency-code-schema.ts
│ │ ├── metadata-label-schema.ts
│ │ └── ... (25+ validation schemas)
│ │
│ ├── generated/ # ★ Auto-generated (GraphQL Codegen)
│ │ ├── graphql.tsx # Generated types & hooks (main)
│ │ ├── graphql-upgrade.tsx # Upgrade types
│ │ ├── graphql-crm.tsx # CRM-specific types
│ │ └── index.js
│ │
│ ├── generated-metadata/ # ★ Auto-generated Metadata Types
│ │
│ ├── libs/ # ★ Core Libraries
│ │ ├── Env.ts # Environment validation (t3-env)
│ │ ├── i18n.ts # i18n config
│ │ ├── i18nNavigation.ts # i18n navigation helpers
│ │ ├── Logger.ts # Pino logger
│ │ └── Arcjet.ts # Arcjet security
│ │
│ ├── templates/ # ★ Page Layout Templates
│ │ ├── base-template.tsx # Sidebar + content layout
│ │ ├── content-template.tsx # Main content area
│ │ ├── protected-template.tsx # Auth guard wrapper
│ │ ├── header.tsx # Page header
│ │ ├── notification.tsx # Notification system
│ │ ├── button-add-new.tsx # Add new button
│ │ └── sidebar/ # Sidebar navigation
│ │
│ ├── locales/ # ★ Translation Files (JSON)
│ ├── styles/ # ★ Global CSS
│ ├── i18n/ # ★ i18n Config
│ ├── instrumentation.ts # Sentry instrumentation
│ └── proxy.ts # HTTP proxy config
│
├── codegen.ts # GraphQL Codegen config
├── codegen-metadata.ts # Metadata Codegen config
├── next.config.ts # Next.js config
├── tailwind.config.ts # Tailwind CSS config
├── eslint.config.mjs # ESLint config (antfu)
├── tsconfig.json # TypeScript config
├── vitest.config.mts # Vitest config
├── playwright.config.ts # Playwright config
├── commitlint.config.ts # Commitlint config
├── components.json # Shadcn UI config
├── sentry.client.config.ts # Sentry client config
├── ecosystem.config.js # PM2 config
├── docker-compose.prod.yml # Docker Compose production
├── docker-compose.dev.yml # Docker Compose development
├── prod.Dockerfile # Production Dockerfile
└── dev.Dockerfile # Development Dockerfile
```

---

## 4. Luồng hoạt động chính

### 4.1 Provider Hierarchy

Dự án sử dụng mô hình **nested providers**, mỗi level wrap thêm context cần thiết.

#### Root Layout (`src/app/[locale]/layout.tsx`)

```xml
<html>
  <body>
    <ClientConfigProvider>           ← Client config từ server
      <ThemeProvider>                 ← Dark/Light theme (next-themes)
        <ApolloWrapperProvider>       ← Apollo Client cho GraphQL
          <ApolloCoreProvider>        ← Apollo Client cho core metadata
            <NextIntlClientCustomProvider> ← i18n messages
              <PostHogProvider>       ← Analytics tracking
                {children}
              </PostHogProvider>
            </NextIntlClientCustomProvider>
          </ApolloCoreProvider>
        </ApolloWrapperProvider>
      </ThemeProvider>
    </ClientConfigProvider>
  </body>
</html>
```

#### Protected Layout (`src/app/[locale]/(protected)/layout.tsx`)

```xml
<ProtectedTemplate>             ← Auth guard (redirect nếu chưa login)
  <PageVisibilityHandler />      ← Track tab visibility
  <TokenExpiryWatcher />         ← Auto refresh/logout khi token hết hạn
  <AppearanceProvider>           ← Module visibility theo role
    <ConfirmModalProvider>       ← Confirm dialog global
      <GlobalModalProvider>      ← Global modal system
        <NotificationProvider>   ← Push notification (Firebase)
          <ModalProviderMenuHeader> ← Header menu modals
            <BaseTemplate>       ← Sidebar + Content layout
              <AiSuggestionProvider> ← AI suggestion context
                <CurrentViewProvider> ← Current view state
                  <KpiProvider>  ← KPI context
                    {children}   ← Page content
                  </KpiProvider>
                </CurrentViewProvider>
              </AiSuggestionProvider>
              <ChatbotWidget />  ← Chatbot widget overlay
            </BaseTemplate>
          </ModalProviderMenuHeader>
        </NotificationProvider>
      </GlobalModalProvider>
    </ConfirmModalProvider>
  </AppearanceProvider>
</ProtectedTemplate>
```

### 4.2 Authentication Flow

```
┌─────────────────────────────────────────────────────────┐
│ User truy cập /{locale}/sign-in │
│ → Nhập credentials → POST login API │
│ → Nhận JWT token → Lưu vào cookie (js-cookie) │
│ → Redirect → /{locale}/ (protected) │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ Protected Layout (server-side) │
│ → getCurrentUser() gọi GraphQL server-side │
│ → Nếu chưa auth → redirect /sign-in │
│ → Nếu auth → wrap AuthProvider + render page │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ Token Management │
│ → TokenExpiryWatcher: monitor token expiry │
│ → PageVisibilityHandler: refresh khi tab active lại │
│ → Apollo authLink: tự chèn Bearer token vào mọi request│
└─────────────────────────────────────────────────────────┘
```

### 4.3 Data Flow (GraphQL)

```
┌──────────┐ useQuery/ ┌──────────────┐ HTTP ┌──────────┐
│Component │───useMutation────→ │ Apollo Client │───────────→ │ Backend │
│ │ (generated) │ │ (authLink │ GraphQL │
│ │ │ InMemory │ errorLink │ Server │
│ │←──────────────────│ Cache │←──httpLink)──│ │
│ re-render│ cache update │ │ response │ │
└──────────┘              └──────────────┘              └──────────┘
```

**Chi tiết Apollo Link Chain:**

1. `authLink` (ApolloHttpSetContext): Chèn `Authorization: Bearer <token>` vào headers
2. `errorLink` (ApolloOnError): Xử lý lỗi GraphQL & network errors
3. `httpLink` (ApolloHttpLinkInit): HTTP transport tới GraphQL endpoint

**GraphQL Codegen Pipeline:**

```
Backend GraphQL Schema
  ↓ introspection
codegen.ts config
  ↓ @graphql-codegen/cli
src/services/graphql/**/*.ts (queries/mutations)
  ↓ generates
src/generated/graphql.tsx         (types + React hooks)
src/generated/graphql-upgrade.tsx
src/generated/graphql-crm.tsx
```

### 4.4 Metadata-Driven Architecture

Kiến trúc cốt lõi cho phép tạo objects/fields/views động từ backend:

```
│ Backend Metadata │
│ → Object definitions (name, fields, relations) │
│ → Field types (TEXT, NUMBER, DATE, RELATION, SELECT,...) │
│ → View configurations (table, kanban, calendar) │
└────────────────────┬────────────────────────────────────┘
│ GraphQL query
▼
┌─────────────────────────────────────────────────────────┐
│ Frontend Metadata Layer │
│ hooks/object-metadata/ → parse metadata │
│ hooks/object-record/ → CRUD dựa trên metadata │
│ hooks/views/ → view configuration │
│ hooks/fields/ → field rendering │
└────────────────────┬────────────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────┐
│ Dynamic UI Components │
│ components/object-view/ → Render view theo metadata │
│ components/custom-fields/ → Field editor theo type │
│ react-table-grid-custom/ → Table columns dynamic │
│ components/filter/ → Filter theo field type │
└─────────────────────────────────────────────────────────┘
```

**Ví dụ:** khi admin tạo object "Deals" với fields "Amount (NUMBER)", "Stage (SELECT)", "Contact (RELATION)":

- Frontend tự động tạo table view với columns tương ứng
- Filter tự sinh dựa trên field types
- Form tự render đúng input type
- Kanban có thể group theo SELECT field

### 4.5 Internationalization (i18n)

URL: `/{locale}/customers` → `/vi/customers`, `/en/customers`

```
┌──────────────────────────────────────────────────────┐
│ next-intl flow:                                      │
│ 1. URL /{locale}/... → App Router [locale] segment   │
│ 2. request.ts → load messages cho locale             │
│ 3. NextIntlClientCustomProvider wrap client           │
│ 4. Components dùng useTranslations('namespace')      │
│ 5. Translation files: src/locales/{locale}/*.json    │
└──────────────────────────────────────────────────────┘
```

---

## 5. Quy tắc code

### 5.1 ESLint

Config: `eslint.config.mjs` — base trên `@antfu/eslint-config`

| Rule                                    | Giá trị                           | Giải thích                         |
| --------------------------------------- | --------------------------------- | ---------------------------------- |
| `ts/consistent-type-definitions`        | `['error', 'type']`               | Dùng `type` thay vì `interface`    |
| `style/brace-style`                     | `['error', '1tbs']`               | One True Brace Style               |
| `stylistic.semi`                        | `true`                            | Bắt buộc semicolons                |
| `no-console`                            | `allow: ['warn', 'error', 'log']` | Chỉ được dùng 3 methods này        |
| `react/prefer-destructuring-assignment` | `off`                             | Không bắt buộc destructuring props |
| `react/no-array-index-key`              | `off`                             | Cho phép dùng index làm key        |
| `antfu/no-top-level-await`              | `off`                             | Cho phép top-level await           |
| `test/padding-around-all`               | `error`                           | Bắt buộc padding trong test files  |
| `JSX A11y`                              | `recommended`                     | Accessibility rules                |

Ignored paths: `migrations/`, `next-env.d.ts`, `src/generated/*`, `src/generated-metadata/*`

### 5.2 TypeScript

Config: `tsconfig.json` — strict mode đầy đủ

| Option                             | Giải thích                           |
| ---------------------------------- | ------------------------------------ |
| `strict: true`                     | Enable tất cả strict checks          |
| `strictNullChecks: true`           | Null/undefined phải xử lý tường minh |
| `noImplicitAny: true`              | Không cho implicit any               |
| `noUnusedLocals: true`             | Không cho biến local không dùng      |
| `noUnusedParameters: true`         | Không cho parameter không dùng       |
| `noUncheckedIndexedAccess: true`   | Index access trả về `T \| undefined` |
| `noImplicitReturns: true`          | Mọi path phải return                 |
| `noFallthroughCasesInSwitch: true` | Switch phải có break/return          |

Path alias: `@/*` → `src/*` Import shorthand

### 5.3 Commit Convention

- **Commitlint + Commitizen:** Conventional Commits format
- **Format:** `type(scope): description`
- **Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, ...
- **Husky hooks:** pre-commit chạy lint-staged
- **semantic-release:** auto versioning & changelog

### 5.4 Validation

- Schema-first approach với **Zod**
- Kết hợp **Formik** qua `zod-formik-adapter`
- Validation schemas tổ chức trong `src/validations/`
- Ví dụ: `SignInValidation.ts`, `lead-schema.ts`, `field-metadata-item-schema.ts`

---

## 6. Components tái sử dụng

### 6.1 Shadcn UI Primitives

Đường dẫn: `src/components/shadcn-ui/`

Các atomic components xây trên Radix UI, styled bằng Tailwind + CVA:

| Component           | Mô tả                                                            |
| ------------------- | ---------------------------------------------------------------- |
| `button.tsx`        | Button với variants (default, destructive, outline, ghost, link) |
| `dialog.tsx`        | Modal dialog                                                     |
| `alert-dialog.tsx`  | Confirm dialog                                                   |
| `input.tsx`         | Text input                                                       |
| `textarea.tsx`      | Multiline input                                                  |
| `select.tsx`        | Select dropdown                                                  |
| `checkbox.tsx`      | Checkbox                                                         |
| `switch.tsx`        | Toggle switch                                                    |
| `tabs.tsx`          | Tab navigation                                                   |
| `dropdown-menu.tsx` | Context/Dropdown menu                                            |
| `popover.tsx`       | Popover container                                                |
| `command.tsx`       | Combobox (cmdk)                                                  |
| `calendar.tsx`      | Calendar picker                                                  |
| `date-picker.tsx`   | Date picker                                                      |
| `card.tsx`          | Card container                                                   |
| `table.tsx`         | HTML table wrapper                                               |
| `badge.tsx`         | Badge/Tag                                                        |
| `skeleton.tsx`      | Loading skeleton                                                 |
| `tooltip.tsx`       | Tooltip                                                          |
| `progress.tsx`      | Progress bar                                                     |
| `scroll-area.tsx`   | Custom scrollbar                                                 |
| `separator.tsx`     | Horizontal/Vertical divider                                      |
| `drawer.tsx`        | Drawer (vaul)                                                    |
| `sonner.tsx`        | Toast notifications                                              |
| `pagination.tsx`    | Pagination                                                       |
| `accordion.tsx`     | Collapsible sections                                             |
| `breadcrumb.tsx`    | Breadcrumb navigation                                            |
| `avatar.tsx`        | User avatar                                                      |
| `chart.tsx`         | Chart wrapper                                                    |
| `label.tsx`         | Form label                                                       |
| `alert.tsx`         | Alert banner                                                     |

### 6.2 Custom UI Components

Đường dẫn: `src/components/ui/`

Components tái sử dụng across toàn bộ project:

**Display & Layout**

| Component                                                 | Mô tả                    |
| --------------------------------------------------------- | ------------------------ |
| `avatar.tsx`, `avatar-chip.tsx`, `avatar-placeholder.tsx` | Avatar variants          |
| `badge-status.tsx`                                        | Status badge với màu sắc |
| `Chip.tsx`, `ChipGroup.tsx`                               | Chip / tag group         |
| `tag.tsx`                                                 | Tag component            |
| `tabs.tsx`                                                | Custom tabs              |
| `steps.tsx`                                               | Step indicator           |
| `pagination.tsx`                                          | Custom pagination        |
| `tooltip.tsx`                                             | Enhanced tooltip         |
| `paper.tsx`                                               | Paper/Card container     |
| `copy-box.tsx`                                            | Copy-to-clipboard box    |
| `image-custom.tsx`                                        | Optimized image          |
| `image-preview-card.tsx`                                  | Image card preview       |
| `image-preview-modal.tsx`                                 | Fullscreen image preview |
| `logo.tsx`                                                | App logo                 |
| `portal.tsx`                                              | React portal wrapper     |
| `virtualized.tsx`                                         | Virtualized list         |
| `lazy-section.tsx`                                        | Lazy loaded section      |

**Form & Input**

| Component              | Mô tả                  |
| ---------------------- | ---------------------- |
| `field-edit-form.tsx`  | Field inline edit form |
| `record-edit-form.tsx` | Record edit form       |
| `multi-select.tsx`     | Multi-select dropdown  |
| `selector.tsx`         | Generic selector       |
| `radio-group.tsx`      | Radio button group     |
| `filter-dropdown.tsx`  | Filter dropdown        |
| `color-picker.tsx`     | Color picker           |
| `icons-popover.tsx`    | Icon selector popover  |

**Navigation & Actions**

| Component             | Mô tả                       |
| --------------------- | --------------------------- |
| `nav-toggle.tsx`      | Navigation toggle           |
| `change-language.tsx` | Language switcher           |
| `link-language.tsx`   | Localized link              |
| `user-dropdown.tsx`   | User menu dropdown          |
| `user.tsx`            | User display                |
| `social-link.tsx`     | Social media link           |
| `contact-link.tsx`    | Contact link (phone, email) |
| `rounded-link.tsx`    | Rounded action link         |

**Specialized**

| Component                       | Mô tả                        |
| ------------------------------- | ---------------------------- |
| `calendar-view.tsx`             | Calendar display             |
| `heading-form.tsx`              | Section heading              |
| `person-quick-view-modal.tsx`   | Quick-view modal cho người   |
| `task-list-section.tsx`         | Task list display            |
| `note-attachments-display.tsx`  | Note & attachment viewer     |
| `icon-mapping.tsx`              | Dynamic icon by name         |
| `trans-component-table.tsx`     | Translatable table component |
| `highlight-text.tsx` (in utils) | Text highlight               |
| `record-display/`               | Record value display system  |
| `property-box/`                 | Property display box         |
| `setting/`                      | Setting UI components        |

### 6.3 Core UI Components

Đường dẫn: `src/components/core-ui/`

Business-level components:

| Component                        | Mô tả                           |
| -------------------------------- | ------------------------------- |
| `button-custom.tsx`              | Custom CRM button               |
| `button-export.tsx`              | Export data button              |
| `card-department.tsx`            | Department card                 |
| `card-product.tsx`               | Product card                    |
| `card-role.tsx`                  | Role card                       |
| `card-team.tsx`                  | Team card                       |
| `card-ui.tsx`                    | Generic business card           |
| `card-placeholder-empty.tsx`     | Empty state placeholder         |
| `emty-card.tsx`                  | Empty card                      |
| `badge-urgent.tsx`               | Urgent badge                    |
| `hover-card.tsx`                 | Hover preview card              |
| `user-card.tsx`                  | User info card                  |
| `circle-filled.tsx`              | Filled circle indicator         |
| `compare-button.tsx`             | Compare action button           |
| `view-field-for-object-item.tsx` | Field renderer cho object items |

### 6.4 Data Grid System

Đường dẫn: `src/components/react-table-grid-custom/`

Hệ thống data table/grid phức tạp nhất, dùng `react-data-grid` + custom.

Cũng có `react-table-grid-split-custom/` cho split-view table.

### 6.5 Module-specific Components

Mỗi domain có folder riêng trong `src/components/`:

| Folder               | Module                        |
| -------------------- | ----------------------------- |
| `customers/`         | Quản lý khách hàng            |
| `chats/`             | Chat/Messaging                |
| `order-management/`  | Đơn hàng                      |
| `sales-activities/`  | Hoạt động bán hàng            |
| `report-management/` | Báo cáo                       |
| `dashboard/`         | Dashboard                     |
| `calendar/`          | Calendar views                |
| `chatbot/`           | AI Chatbot widget             |
| `employees/`         | Nhân viên                     |
| `departments/`       | Phòng ban                     |
| `settings/`          | Cài đặt                       |
| `tasks/`             | Tasks                         |
| `tags/`              | Tags                          |
| `filter/`            | Filter system                 |
| `modal/`             | Modal system                  |
| `import-data/`       | Import data                   |
| `object-view/`       | Object view (metadata-driven) |
| `custom-fields/`     | Dynamic field editors         |
| `conversion/`        | Lead conversion               |
| `auth/`              | Auth components               |
| `analytics/`         | PostHog analytics             |
| `design/`            | Design system                 |
| `loadings/`          | Loading states                |
| `table-actions/`     | Table row actions             |
| `button-actions/`    | Action buttons                |
| `button-tab/`        | Tab buttons                   |
| `searchable-select/` | Searchable select             |
| `selects-scroll/`    | Infinite scroll select        |
| `whitewave/`         | Whitewave-specific            |

---

## 7. Contexts / Providers

Toàn bộ providers trong `src/contexts/`:

| Provider                                            | Scope     | Mô tả                              |
| --------------------------------------------------- | --------- | ---------------------------------- |
| `auth-provider.tsx`                                 | Protected | User, workspace, workspace members |
| `apollo-provider.tsx`                               | Root      | Apollo Client cho GraphQL chính    |
| `apollo-core-provider.tsx`                          | Root      | Apollo Client cho core/metadata    |
| `client-config-provider.tsx`                        | Root      | Client config từ server            |
| `appearance-provider.tsx`                           | Protected | Module visibility theo role        |
| `notification-provider.tsx`                         | Protected | Firebase push notifications        |
| `confirm-modal-provider.tsx`                        | Protected | Global confirm dialog              |
| `global-modal-provider.tsx`                         | Protected | Global modal system                |
| `modal-provider-menu-header.tsx`                    | Protected | Header menu modals                 |
| `sidebar-toggle-provider.tsx`                       | Protected | Sidebar open/close state           |
| `current-view-provider.tsx`                         | Protected | Current table/kanban view state    |
| `views-provider.tsx`                                | Module    | Views list                         |
| `favorite-provider.tsx`                             | Module    | Favorites                          |
| `ai-suggestion-provider.tsx`                        | Protected | AI suggestion state                |
| `widget-agent-provider.tsx`                         | Module    | Widget agent state                 |
| `form-fields-provider.tsx`                          | Module    | Form fields state                  |
| `report-filter-provider.tsx`                        | Module    | Report filter state                |
| `object-items-provider.tsx`                         | Module    | Object items state                 |
| `apply-record-filter-provider.tsx`                  | Module    | Record filter application          |
| `apply-record-filter-group-provider.tsx`            | Module    | Filter group                       |
| `apply-one-record-filter-provider.tsx`              | Module    | Single record filter               |
| `active-tab-provider.tsx`                           | Module    | Active tab state                   |
| `field-object-setting-provider.tsx`                 | Module    | Field/Object settings              |
| `object-options-dropdown-view-provider.tsx`         | Module    | View options dropdown              |
| `object-options-dropdown-view-content-provider.tsx` | Module    | View dropdown content              |
| `RecordDataContext.tsx`                             | Module    | Record data context                |
| `next-intl-client-custom-provider.tsx`              | Root      | i18n messages                      |
| `social-messages/`                                  | Module    | Social messages context            |

---

## 8. Templates / Page Layout

Đường dẫn: `src/templates/`

---

## 9. Services Layer

### GraphQL Services (`src/services/graphql/`)

Tổ chức theo domain, mỗi folder chứa files cho queries & mutations.

### Client Services (`src/services/client/`)

Client-side function calls (auth, metadata fetching).

### Server Services (`src/services/server/`)

Server-side only (SSR, Server Components):

| File                           | Mô tả                                |
| ------------------------------ | ------------------------------------ |
| `get-current-user.ts`          | Fetch current authenticated user     |
| `get-metadata-object-items.ts` | Fetch metadata objects               |
| `get-widget-agent.ts`          | Fetch widget agent config            |
| `apollo-server-query.ts`       | Execute GraphQL query server-side    |
| `apollo-server-mutate.ts`      | Execute GraphQL mutation server-side |

---

## 10. Hooks

Đường dẫn: `src/hooks/`

Tổ chức theo domain, mỗi folder chứa custom hooks cho module tương ứng:

| Folder              | Mô tả                 | Ví dụ hooks                                             |
| ------------------- | --------------------- | ------------------------------------------------------- |
| `graphql/`          | GraphQL utility hooks | Apollo cache, queries                                   |
| `client/`           | Client-side hooks     | Client state, browser APIs                              |
| `object/`           | Object hooks          | `useObject`, `useObjectList`                            |
| `object-metadata/`  | Metadata hooks        | `useObjectMetadata`                                     |
| `object-record/`    | Record CRUD           | `useCreateRecord`, `useUpdateRecord`, `useDeleteRecord` |
| `views/`            | View hooks            | `useCurrentView`, `useViewFields`                       |
| `fields/`           | Field hooks           | `useFieldMetadata`, `useFieldValue`                     |
| `customers/`        | Customer hooks        | `useCustomers`, `useCustomerDetail`                     |
| `leads/`            | Lead hooks            | `useLeads`, `useLeadPipeline`                           |
| `order/`            | Order hooks           | `useOrders`                                             |
| `products/`         | Product hooks         | `useProducts`                                           |
| `employees/`        | Employee hooks        | `useEmployees`                                          |
| `departments/`      | Department hooks      | `useDepartments`                                        |
| `tags/`             | Tag hooks             | `useTags`                                               |
| `roles/`            | Role hooks            | `useRoles`, `usePermissions`                            |
| `chats/`            | Chat hooks            | `useChats`, `useMessages`                               |
| `favorites/`        | Favorite hooks        | `useFavorites`                                          |
| `kpi/`              | KPI hooks             | `useKpiData`                                            |
| `filter/`           | Filter hooks          | `useRecordFilter`                                       |
| `cache/`            | Cache hooks           | `useCacheStrategy`                                      |
| `advanced/`         | Advanced hooks        | Complex reusable logic                                  |
| `companies/`        | Company hooks         | `useCompanies`                                          |
| `integrations/`     | Integration hooks     | `useIntegrations`                                       |
| `reminder/`         | Reminder hooks        | `useReminders`                                          |
| `social-messages/`  | Social message hooks  | `useSocialMessages`                                     |
| `multiple-objects/` | Multi-object hooks    | Cross-object queries                                    |
| `workspace/`        | Workspace hooks       | `useWorkspace`                                          |
| `lab/`              | Lab hooks             | Experimental features                                   |
| `default-settings/` | Default setting hooks | `useDefaultSettings`                                    |

---

## 11. Key Patterns

- **Metadata-Driven UI** — Objects, fields, views được tạo động từ backend metadata. Frontend render dựa trên metadata thay vì hard-code.
- **GraphQL Codegen** — Chạy `npm run codegen` → tự sinh TypeScript types + React Apollo hooks vào `src/generated/`. Không viết types thủ công cho API responses.
- **Server Components (RSC)** — Layouts và data fetching chính chạy server-side. Client components được đánh dấu `'use client'`.
- **Dynamic Imports** — Heavy components (modals, editors, charts) dùng `next/dynamic` để lazy load, giảm initial bundle size.
- **Domain-based Organization** — Services, hooks, components tổ chức theo domain (customers, leads, orders,...), không theo technical layer.
- **Apollo Cache** — `InMemoryCache` cho caching + optimistic updates để UX mượt khi mutate data.
- **Zod + Formik (Schema-first Validation)** — Validation schemas định nghĩa bằng Zod, kết hợp Formik qua adapter.
- **Proxy API Routes** — `/app/graphql/` và `/app/rest/` proxy requests tới backend, tránh CORS và giữ token an toàn.
- **Firebase Real-time** — Firebase Cloud Messaging cho push notifications, thay thế GraphQL subscriptions.
- **i18n Routing** — Mỗi URL có prefix locale: `/vi/customers`, `/en/customers`. Translation files trong `src/locales/`.
- **`cn()` Utility** — Merge Tailwind classes an toàn.
- **Permission System** — `check-permission.ts`, `get-object-permissions-for-object.ts`. `AppearanceProvider`: module visibility theo role. Route-level protection qua `ProtectedTemplate`.

---

## 12. Scripts

| Script                     | Mô tả                                          |
| -------------------------- | ---------------------------------------------- |
| `npm run dev`              | Chạy dev server (port 5000) + Sentry Spotlight |
| `npm run build`            | Build production                               |
| `npm run start`            | Start production (port 8988)                   |
| `npm run lint`             | Chạy ESLint                                    |
| `npm run lint:fix`         | Auto-fix ESLint                                |
| `npm run check-types`      | TypeScript type checking                       |
| `npm run test`             | Vitest unit tests                              |
| `npm run test:e2e`         | Playwright E2E tests                           |
| `npm run codegen`          | Generate GraphQL types & hooks                 |
| `npm run codegen:metadata` | Generate metadata types                        |
| `npm run storybook`        | Storybook dev (port 6006)                      |
| `npm run deploy:prod`      | Build + PM2 restart                            |
| `npm run commit`           | Interactive commit (Commitizen)                |
| `npm run build-stats`      | Build với bundle analyzer                      |

---

## 13. Deployment

### Docker

- **Development:** `docker-compose.dev.yml` + `dev.Dockerfile`
- **Production:** `docker-compose.prod.yml` + `prod.Dockerfile`
- Có script: `build-server.sh`, `deploy-to-server.sh`, `push-images.ps1` / `push-images.sh`

### PM2

- Config: `ecosystem.config.js`
- Command: `npm run deploy:prod` → build + pm2 restart

### Environment

- Env validation qua `@t3-oss/env-nextjs` trong `Env.ts`
- Node >= 20 required

> **Lưu ý:** File `generated` và `generated-metadata` được auto-generated bởi GraphQL Codegen. Không chỉnh sửa thủ công.
