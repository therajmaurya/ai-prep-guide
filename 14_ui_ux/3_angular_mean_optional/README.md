---
title: "Angular & MEAN Stack"
category: good-to-know
levels: ["mid", "senior", "principal"]
skills: [angular, typescript, mongodb, express, nodejs, rxjs, signals]
questions:
  "mid": ["What is Two-Way Data Binding?", "Difference between Component and Directive."]
  "senior": ["Explain Dependency Injection in Angular.", "How does Change Detection work in Angular?", "Explain Angular Signals vs RxJS Observables."]
  "principal": ["Design a scalable MEAN stack architecture.", "How do you handle hydration and server-side rendering in Angular?"]
---

# Angular & MEAN Stack

## 💡 Core Ideas

### Angular Implementation
*   **Signals (v16+)**: A new reactive primitive for fine-grained reactivity.
    *   **Writable Signals**: Create values that can be updated (`signal()`).
    *   **Computed Signals**: Derive values automatically (`computed()`).
    *   **Effects**: Run side-effects when signals change (`effect()`).
    *   *Advantage*: Enables zoneless applications (eventually removing strict dependency on `zone.js`) and simpler state management than complex RxJS streams.
*   **Standalone Components (v14+)**: The modern way to build Angular apps.
    *   No need for `NgModules`.
    *   Components, Directives, and Pipes are self-contained.
    *   Simplifies lazy loading (`loadComponent`) and testing.
*   **Hydration (v16+)**: Non-destructive client-side hydration.
    *   Reuses existing DOM nodes from Server-Side Rendering (SSR) instead of destroying and re-rendering.
    *   Improves Core Web Vitals (LCP, CLS).
*   **RxJS & Observables**:
    *   **Streams**: Handling asynchronous data (HTTP, User Input).
    *   **Operators**: `map`, `switchMap` (cancels previous), `mergeMap` (concurrent), `concatMap` (sequential).
    *   **Subjects**: `BehaviorSubject` (state with initial value) vs `Subject` (event stream).
*   **Dependency Injection (DI)**:
    *   Hierarchical injectors (Root, Platform, Component).
    *   Resolution modifiers: `@Optional()`, `@Self()`, `@SkipSelf()`, `@Host()`.
*   **Change Detection**:
    *   **Default**: Checks entire tree on every event.
    *   **OnPush**: Only checks when `@Input` reference changes or async pipe emits. Critical for performance.

### MEAN Stack Architecture
*   **MongoDB (Database)**: NoSQL document store. Good for flexible schemas and rapid prototyping.
    *   *Modeling*: Embedding vs Referencing documents.
*   **Express.js (Backend Framework)**: Minimalist web framework for Node.js.
    *   *Middleware*: Handling requests, authentication, and error handling pipeline.
*   **Angular (Frontend)**: Client-side logic, routing, and UI.
*   **Node.js (Runtime)**: Event-driven, non-blocking I/O.
    *   Handles high concurrency but CPU-intensive tasks can block the event loop.

## 📚 Resources
*   [Angular Official Documentation](https://angular.dev/) (New Docs)
*   [Angular Signals Guide](https://angular.dev/guide/signals)
*   [RxJS Operator Decision Tree](https://rxjs.dev/operator-decision-tree)
*   [MongoDB Schema Design Best Practices](https://www.mongodb.com/developer/products/mongodb/mongodb-schema-design-best-practices/)

## 🛠️ Practice
*   **Project 1: Task Manager (Mid)**
    *   Build a CRUD app with Angular and a simple Express API.
    *   Use Template-driven forms.
*   **Project 2: Real-time Dashboard (Senior)**
    *   Use **Signals** for local state and **RxJS** for WebSocket data.
    *   Implement **OnPush** change detection.
    *   Secure endpoints with JWT implementation in Node/Express.
    *   Use **Standalone Components**.

## ❓ Questions

### Mid Level
*   **What is Two-Way Data Binding?** (Understanding `[(ngModel)]`)
*   **Difference between Component and Directive?** (Components have templates; Directives modify behavior/DOM).
*   **What are Lifecycle Hooks?** (`ngOnInit`, `ngOnDestroy`, `ngOnChanges`).
*   **Observables vs Promises?** (Lazy/Cancellable/Stream vs Eager/Single-value).

### Senior Level
*   **How does Change Detection work in Angular?** Explain Zone.js and the component tree check.
*   **Explain Dependency Injection.** How does the hierarchical injector work?
*   **Signals vs RxJS?** When to use which? (Signals for synchronous UI state, RxJS for async event streams).
*   **What is the purpose of `trackBy` in `*ngFor` (or `@for` tracking)?** Performance optimization to prevent re-rendering list items.
*   **Explain Lazy Loading.** How do you implement it with the generic Router?

### Principal Level
*   **Design a scalable Micro-frontend architecture with Angular.** (Module Federation, Standalone components).
*   **Optimize a slow Angular application.** (Profile with DevTools, implement OnPush, remove heavy computations from templates, use Web Workers, configure hydration).
*   **Security in MEAN stack.** How do you prevent XSS in Angular and NoSQL Injection in MongoDB? (Sanitization, validation).
