+++
title = "Day 02 - 16/09/2026 (remote)"
weight = 2
+++

## PART 1. REACT FUNDAMENTALS

### 1. What is React?
React is an open-source JavaScript library developed by Meta (Facebook) for building user interfaces, primarily for Single Page Applications (SPAs). React focuses exclusively on the View layer in the MVC pattern and operates on a component-driven architecture.

### 2. What is a Component in React? How many types are there?
A component is an independent, reusable building block of UI that manages its own logic and visual presentation. There are two primary types:
- **Function Components:** Declared as JavaScript functions that accept props and return JSX. This is the modern official standard powered by React Hooks.
- **Class Components:** Declared using ES6 classes extending `React.Component`, managing internal state via `this.state` and lifecycle methods. Rarely used in new codebases today.

### 3. What is JSX?
JSX (*JavaScript XML*) is a syntax extension for JavaScript that allows developers to write HTML-like markup directly within JavaScript code. Compilers like Babel or SWC transform JSX into native `React.createElement()` calls.

### 4. What are Props?
Props (*Properties*) represent the unidirectional data mechanism passed from a parent component down to a child component. Props are **immutable (read-only)**; a child component must never directly modify the props it receives.

### 5. What is State? How does State differ from Props?
State is an internal data structure maintained within a component that can change over time due to user interactions or network responses. Whenever state updates, the component re-renders to reflect changes.

| Criterion | Props | State |
| :--- | :--- | :--- |
| **Origin** | Passed from parent to child | Initialized and managed internally |
| **Mutability** | Immutable (Read-only) | Mutable (via updater functions like `setState`) |
| **Purpose** | Component configuration & reusability | Managing dynamic component behavior and data |

### 6. What is the Virtual DOM? Why does React use it?
The Virtual DOM (VDOM) is an in-memory lightweight JavaScript object tree representing the real browser DOM.
- **Why use it:** Direct DOM manipulation is computationally expensive. When state changes, React constructs a new VDOM, compares it with the previous VDOM using a diffing algorithm (Reconciliation), and selectively updates only the modified nodes on the real DOM (*Batch update*), ensuring optimal rendering performance.

### 7. What are Hooks? List common React Hooks.
Introduced in React 16.8, Hooks are built-in functions that enable Function Components to use state, lifecycle events, and other React features without writing class components.
- **Common Hooks:** `useState`, `useEffect`, `useContext`, `useRef`, `useMemo`, `useCallback`, `useReducer`.

### 8. What is `useState` used for?
`useState` declares and manages local component state within a Function Component. Syntax: `const [state, setState] = useState(initialValue);`.

### 9. What is `useEffect` used for?
`useEffect` handles side effects in Function Components, such as data fetching, event listeners, manual DOM modifications, and timer cleanups upon unmounting.

### 10. What are the phases in a React Component Lifecycle?
A component lifecycle consists of three core phases:
- **Mounting:** Creating and inserting the component into the actual DOM tree.
- **Updating:** Re-rendering when component props change or internal state updates.
- **Unmounting:** Removing the component from the DOM and cleaning up resources.

### 11. What is Client-Side Rendering (CSR)?
CSR is a rendering approach where the browser generates the entire UI on the client side. The server initially serves a bare-bones HTML shell (with a `<div id="root"></div>`) and JavaScript bundles. The browser executes the JavaScript to render content dynamically.

### 12. What is React Router?
React Router is the standard declarative routing library for React, enabling seamless navigation across views in Single Page Applications without initiating a full page reload from the server.

### 13. Does vanilla React support Routing, SEO, and API Servers out-of-the-box?
- **Routing:** No out-of-the-box routing (requires external libraries like `react-router-dom`).
- **SEO:** Poor by default, as initial payloads lack pre-rendered HTML content for search engine crawlers.
- **API Server:** Unsupported; vanilla React operates strictly on the client side and requires an external backend server.

### 14. What is the Context API? When should it be used?
The Context API provides a built-in mechanism to share data across the component tree without manually threading props through intermediate components (*preventing Prop Drilling*).
- **Use Cases:** Global application state such as UI themes (Light/Dark mode), authentication state, or internationalization (i18n).

### 15. What is a Single Page Application (SPA)?
An SPA is a web application architecture that loads an individual HTML document on initial load. Subsequent user interactions dynamically update page segments via client-side JavaScript and JSON APIs without triggering full page reloads.

---

## PART 2. REACT VS. NEXT.JS COMPARISON

### 1. What is Next.js?
Next.js is a full-stack React framework developed by Vercel that delivers high performance and built-in SEO capabilities through diverse rendering strategies (SSR, SSG, ISR) and unified routing.

### 2. What is the core difference between React and Next.js?
- **React:** A specialized **UI library** focusing exclusively on client-side rendering (CSR). Developers must configure routing, build pipelines, SEO handling, and backend services independently.
- **Next.js:** A comprehensive **full-stack framework** built on top of React, offering zero-config file-based routing, asset optimizations, server rendering capabilities, and integrated API routes.

### 3. How does Routing differ between React and Next.js?
- **React:** Code-based routing relying on external packages like `react-router-dom`.
- **Next.js:** **File-based Routing**. Folders and files map directly to URL routes (e.g., `app/about/page.tsx` maps automatically to `/about`).

### 4. How does Rendering differ between React and Next.js?
- **React:** Relies predominantly on Client-Side Rendering (**CSR**).
- **Next.js:** Supports hybrid rendering techniques: **SSR** (per-request rendering), **SSG** (build-time generation), **ISR** (background revalidation), and **CSR** (via Client Components).

### 5. Why does Next.js provide superior SEO compared to vanilla React?
Next.js delivers fully formed HTML containing complete markup and metadata directly from the server prior to client arrival. Search engine crawlers can index content and OpenGraph data instantly without waiting for JavaScript execution.

### 6. How does First Load Performance compare between React and Next.js?
- **React (CSR):** Slower initial load times due to large client JavaScript bundles required before DOM elements can be constructed.
- **Next.js (SSR/SSG):** Exceptionally fast initial page loads, delivering pre-rendered HTML that renders immediately, resulting in superior First Contentful Paint (FCP) scores.

### 7. How does Project Structure differ between React and Next.js?
- **React (Vite/CRA):** Unopinionated structure centered around `index.html` and `src/App.tsx`.
- **Next.js:** Standardized, convention-based layout (App Router) organized within an `app/` directory with reserved files like `layout.tsx`, `page.tsx`, `loading.tsx`, `error.tsx`, and `route.ts`.

### 8. Does Next.js replace React? Why?
No. Next.js operates **on top of React**, not as a replacement. Components in Next.js are still written using React syntax, state management, and JSX. Next.js functions as an architectural framework managing compilation, routing, and deployment.

### 9. When should you choose vanilla React vs. Next.js?
- **Choose Vanilla React:** Internal admin portals, dashboard tools, enterprise software behind authentication where public SEO and social link previews are irrelevant.
- **Choose Next.js:** E-commerce stores, public content platforms, marketing landing pages, and web apps where SEO discoverability and first-load speed are paramount.

---

## PART 3. NEXT.JS DEEP DIVE

### 1. What are App Router and Pages Router in Next.js?
- **Pages Router (Legacy):** Uses the `pages/` directory for file-based routing; handles data via `getStaticProps` and `getServerSideProps`.
- **App Router (Modern - Next.js 13+):** Uses the `app/` directory, introducing nested layouts, default **React Server Components (RSC)**, and built-in UI streaming.

### 2. How do Server Components and Client Components differ?
- **Server Components (Default in App Router):** Render exclusively on the server, send zero JavaScript to the client bundle, cannot use client hooks (`useState`, `useEffect`) or DOM event listeners (`onClick`). Facilitates secure data querying and minimal bundle sizes.
- **Client Components:** Declared with the `'use client'` directive at the top of the file. Hydrate and run on the client, supporting standard React interactive hooks and DOM event handling.

### 3. What is Server-Side Rendering (SSR)?
A rendering strategy where HTML is generated fresh on the web server **on every incoming client request** before being transmitted to the client.

### 4. What is Static Site Generation (SSG)?
A rendering strategy where HTML pages are compiled once **at build time**. These static assets are distributed via Content Delivery Networks (CDNs) for instantaneous retrieval.

### 5. What is Incremental Static Regeneration (ISR)?
A hybrid mechanism allowing developers to update static pages in the background after deployment on a periodic basis (via the `revalidate` property) without rebuilding the entire project.

### 6. How does File-based Routing work in Next.js?
URL paths correspond directly to directory structures on the filesystem:
- A folder `app/projects/page.tsx` maps automatically to `/projects`.
- Each nested folder represents a route segment, with `page.tsx` defining the publicly accessible UI.

### 7. What are Dynamic Routes in Next.js?
Routes created when URL segments depend on variable parameters (like product IDs or blog slugs). In App Router, they are declared using bracketed directory names (e.g., `app/blog/[slug]/page.tsx`).

### 8. What is the role of `layout.tsx` in the App Router?
`layout.tsx` establishes shared wrapper UI across multiple nested route views (e.g., Headers, Footers). Layouts preserve their component state, remain interactive, and do not re-render when child pages change.

### 9. What are API Routes (Route Handlers) in Next.js?
Integrated serverless backend endpoints within a Next.js codebase. Defined inside `route.ts` files, they process standard HTTP methods such as `GET`, `POST`, `PUT`, and `DELETE`.

### 10. What are `getStaticProps` and `getServerSideProps`?
Legacy data-fetching functions from the **Pages Router**:
- `getStaticProps`: Fetches static data at build time (SSG).
- `getServerSideProps`: Fetches dynamic data per request on the server (SSR).

*(Note: In App Router, these functions are replaced by native `async/await fetch()` calls directly inside Server Components).*

### 11. How does `next/image` optimize assets?
The Next.js `<Image/>` component provides:
- Automated image compression and conversion to modern formats (WebP, AVIF).
- Responsive layout sizing based on device viewports.
- Default lazy-loading for off-screen images.
- Prevention of Cumulative Layout Shift (CLS) via explicit dimension enforcement.

### 12. What is Middleware in Next.js?
Middleware (`middleware.ts`) executes code on incoming requests before navigation completes. Commonly used for request authentication, redirects, rewrites, and header/cookie manipulation.

### 13. How do you navigate between pages in Next.js?
- **Declarative Navigation:** Use the `<Link href="...">` component from `next/link` for soft client-side transitions and background prefetching.
- **Programmatic Navigation:** Utilize the `useRouter` hook from `next/navigation` and call `router.push('/target-path')`.

### 14. How are Metadata and SEO handled in Next.js?
The App Router handles SEO configurations via:
- Static Configuration: `export const metadata: Metadata = { title: "...", description: "..." }` in `layout.tsx` or `page.tsx`.
- Dynamic Configuration: `export async function generateMetadata(...)` to compute OpenGraph tags and page titles dynamically from external APIs.

### 15. Does Next.js support TypeScript?
Yes, Next.js provides out-of-the-box TypeScript integration during initialization, providing built-in type definitions for pages, layouts, API handlers, and route segments.

### 16. What platforms can deploy a Next.js application?
- **Vercel:** Native cloud platform engineered specifically for Next.js with automated continuous delivery and edge distribution.
- **Cloud/PaaS Providers:** Netlify, AWS Amplify, Render, and Railway.
- **Self-Hosted Infrastructure:** Containerized via Docker to run on independent VPS instances (Ubuntu/Debian) or Kubernetes clusters.