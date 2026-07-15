# MSW setup

How to wire up **[MSW (Mock Service Worker)](https://mswjs.io/)** as the mock API layer.

Applies to the **Vite + React** projects whose specs recommend it — **admin** and **kanban** (middle &
senior). The Next.js projects (**ecommerce**, **blog**) use **route handlers** under `app/api/…` instead,
so they don't need MSW.

The idea: the UI makes **real `fetch` requests**; MSW intercepts them in the browser (and in tests) and
answers from your seeded, in-memory + localStorage data. That intercept point **is** your API-layer
boundary — and it's where authorization lives (a `viewer`'s request gets a real `403`).

---

## 1. Install

```bash
npm i -D msw
npx msw init public --save
```

`npx msw init` generates **`public/mockServiceWorker.js`** — the worker the browser runs. **Commit it, don't
edit it.** Re-run `msw init` only when you upgrade MSW.

## 2. Create the mock files

```ts
src/mocks/
  handlers.ts   ← your API layer: the routes + logic (project-specific)
  browser.ts    ← the worker, for the running app
  server.ts     ← the server, for Vitest / Node tests
```

```ts
// src/mocks/handlers.ts  — adapt the routes & shapes to your project
import { http, HttpResponse, delay } from "msw";

// your seeded store lives here (in-memory, mirrored to localStorage)
const store = { products: loadOrSeed() };

export const handlers = [
  // list with query params: /api/products?page=2&sort=price:desc&q=key
  http.get("/api/products", async ({ request }) => {
    await delay(250); // simulate latency
    const url = new URL(request.url);
    const page = Number(url.searchParams.get("page") ?? 1);
    const q = url.searchParams.get("q") ?? "";
    let rows = store.products.filter((p) =>
      p.name.toLowerCase().includes(q.toLowerCase()),
    );
    const total = rows.length;
    rows = rows.slice((page - 1) * 10, page * 10);
    return HttpResponse.json({ rows, total });
  }),

  http.post("/api/products", async ({ request }) => {
    const body = await request.json();
    // ...validate, push, persist...
    return HttpResponse.json(body, { status: 201 });
  }),

  // authorization lives in the handler — a real server-style rejection
  http.delete("/api/products/:id", ({ request, params }) => {
    const role = request.headers.get("x-role");
    if (role === "viewer")
      return HttpResponse.json({ error: "forbidden" }, { status: 403 });
    // ...delete, persist...
    return new HttpResponse(null, { status: 204 });
  }),
];
```

```ts
// src/mocks/browser.ts
import { setupWorker } from "msw/browser";
import { handlers } from "./handlers";

export const worker = setupWorker(...handlers);
```

```ts
// src/mocks/server.ts
import { setupServer } from "msw/node";
import { handlers } from "./handlers";

export const server = setupServer(...handlers);
```

## 3. Start it in the app (dev only)

```ts
// src/main.tsx
async function enableMocking() {
  if (!import.meta.env.DEV) return;
  const { worker } = await import('./mocks/browser');
  await worker.start({ onUnhandledRequest: 'bypass' });
}

enableMocking().then(() => {
  createRoot(document.getElementById('root')!).render(<App />);
});
```

## 4. Wire it into tests (Vitest)

```ts
// src/test/setup.ts
import { server } from "../mocks/server";

beforeAll(() => server.listen({ onUnhandledRequest: "error" }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

```ts
// vite.config.ts
export default defineConfig({
  test: { environment: "jsdom", setupFiles: ["./src/test/setup.ts"] },
});
```
