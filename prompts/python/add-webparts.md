---
description: Coding Standards & Rules for Python Web Parts Integration 2025 
globs: **/.ts, **/.tsx, **/.js, **/.jsx, **/.py, **/.css, **/*.scss 
alwaysApply: false
---

# Python Web Parts Integration (2025)

## Data Visualization Integration

* For integrating visualization libraries like D3.js, Chart.js, PixiJS, Three.js, Babylon.js, or Leaflet with Python backends, always standardize data formats served by APIs clearly (JSON with typed schemas).
* When visualizing data from Python APIs, strongly type incoming data using TypeScript interfaces to validate structure immediately upon retrieval from backend.

## UI Frameworks & Python Interaction

* When using Material UI, Chakra UI, Ant Design, or Tailwind CSS with Python backend-generated data, abstract component logic into reusable, strongly-typed frontend components that clearly map backend responses to UI props.
* Avoid passing raw backend JSON directly into components without intermediate validation or normalization.

## State Management & Python APIs

* For state management libraries (Redux, MobX, Zustand, Recoil, Pinia), always ensure clear separation between API calls to Python backend and frontend state management logic. Employ middleware for async calls, making explicit typed interactions.

## Asynchronous Requests & Python APIs

* Utilize Axios or Fetch API for all interactions with Python backends, preferring Axios due to built-in support for interceptors, easier header management, and automatic JSON parsing.
* Clearly structure HTTP calls, ensuring error handling and request cancellation logic are explicit.

## WebAssembly & Python Interactions

* For heavy computation offloaded via WebAssembly, clearly isolate modules interacting with Python backends. WebAssembly modules must receive clearly structured, well-defined JSON payloads from Python backends.

## Testing Libraries & Python Backends

* For frontend testing (Cypress, Playwright, Jest, Vitest), mock Python backend responses explicitly. Tests should strictly verify frontend logic, not backend correctness, which should be independently covered by backend-specific tests.

## Animation & Interactivity Libraries

* When using GSAP, Anime.js, Framer Motion, or Motion One, strictly separate visual animation logic from data fetching. Animations should rely on stable, normalized frontend data structures derived from backend API responses.

## Editors & Interactive Components

* Integrating CodeMirror, Monaco Editor, Tone.js, or interactive components should be explicitly typed and data-driven. Backend APIs must serve structured data tailored specifically for interactive experiences.

## Build Tools & Bundlers (Vite, Webpack, Rollup, Parcel)

* Clearly define build configurations separately from Python backend logic. Bundler configurations should minimize coupling with backend deployments. Static asset handling must remain independent, allowing Python backend changes without triggering frontend recompilation unnecessarily.

## CSS & Styling (Styled-Components, Emotion, Sass, PostCSS, Vanilla Extract, CSS-in-JS)

* Clearly separate styling logic from backend-generated HTML or JSON. Style definitions must be modular and isolated. Styling components should never directly depend on dynamic backend logic without an intermediary layer.

## Real-Time Communication (WebSockets, Server-Sent Events, WebRTC)

* When using real-time protocols, clearly define data payload structure via typed schemas. Ensure Python backend asynchronously serves events, clearly delineating backend event generation from frontend event handling.

## Persistent Storage (Local Storage, IndexedDB, Cookies)

* Explicitly type data stored locally in browsers. Clearly separate backend authentication (cookies) logic from frontend local caching strategies (Local Storage, IndexedDB).

## Progressive Enhancement & Compatibility

* Utilize feature detection for progressive enhancement of frontend capabilities interacting with Python APIs. Ensure graceful fallback when Python backend data isn't available or when browser capabilities vary.