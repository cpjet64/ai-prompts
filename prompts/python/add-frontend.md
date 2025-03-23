---
description: Coding Standards & Rules for Python Frontend Integration 2025 
globs: **/.tsx, **/.jsx, **/.ts, **/.py, **/.html, **/.jinja2, **/*.mako 
alwaysApply: false
---

# Python Frontend Integration (2025)

## General Frontend Integration Guidelines

* Always prefer TypeScript over JavaScript for frontend interactions with Python backends. Ensure type definitions precisely reflect backend response schemas (use code generation tools like OpenAPI generators or GraphQL Codegen when applicable).
* Use clearly defined API schemas or GraphQL schemas to ensure strong typing and validation of data exchanged with Python backends.
* Limit frontend responsibilities strictly to presentation logic, data fetching, and user interaction handling. Complex business logic or data transformations should reside in the Python backend.

## Framework-Specific Tidbits for Python Integration

### React & Next.js

* For Next.js, use API Routes or server-side rendering (SSR) endpoints to securely interface with Python backends (FastAPI, Django REST Framework). Leverage TypeScript for typed `fetch()` or Axios calls to Python endpoints.
* Prefer TanStack Query or SWR to manage caching, validation, and re-fetching states clearly in React projects interacting with Python APIs.

### Vue.js & Nuxt.js

* Integrate Vue's Composition API or Nuxt 3's server API routes for communicating with Python. Employ typed interfaces generated from backend schemas to ensure seamless integration.
* Use Pinia for state management with clearly defined stores representing Python backend state, making reactive API calls explicit and maintainable.

### Angular

* Implement Angular services to abstract HTTP communication with Python backend APIs, ensuring typed responses using Angular's HTTP Client combined with strongly typed DTOs matching backend structures.

### Svelte & SvelteKit

* Use SvelteKit's server endpoints to proxy or directly call Python APIs. Validate and normalize backend responses into Svelte's reactive store mechanism using TypeScript interfaces.

### Solid.js & SolidStart

* Utilize SolidStart's server routes and create type-safe API interactions with Python backends, employing reactive stores and typed client-side fetch logic.

### Alpine.js & HTMX

* Integrate Alpine.js or HTMX directly within Django, Flask, or Jinja2 templates for minimalist frontend interactivity. Keep interactions lightweight; offload complexity to backend routes or APIs.

### PyScript & Brython

* Clearly isolate PyScript or Brython logic in dedicated components, keeping scripts succinct and focused on immediate interactivity. Complex Python operations should always be routed through a backend API.

## Templating Engines Integration

* For Python backend templating (Django Templates, Jinja2, Mako), minimize embedded logic in templates. Move complex processing or conditionals into Python views or context processors.
* Always sanitize user-generated content using built-in escaping provided by template engines. Avoid manually crafting HTML/JS within templates when built-in helpers exist.
* Utilize partial templates or macros extensively for reusable components, avoiding repetitive code across templates.

## Web Workers & Asynchronous Processing

* When heavy frontend computation is unavoidable, delegate tasks to Web Workers clearly typed with TypeScript interfaces, ensuring data passed from Python backend APIs is structured explicitly.

## Progressive Web Apps (PWAs) & Service Workers

* When building PWAs, clearly separate Service Worker logic from backend-generated dynamic content. Ensure static assets are versioned and fetched separately from dynamic Python-backed routes.

## Data Exchange Formats (JSON, YAML, TOML, XML)

* Always prefer JSON as the standard interchange format with Python backends for simplicity and widespread tooling. YAML/TOML/XML only when required by domain-specific scenarios.