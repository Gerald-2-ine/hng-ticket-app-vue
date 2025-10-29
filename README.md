# hng-ticket-app-vue

A small Vue 3 + Vite ticketing app template used for HNG tasks.

This project contains a simple SPA with these pages:
- `/` — Landing page
- `/login` — Login page
- `/signup` — Signup page
- `/dashboard` — Dashboard (protected area)
- `/tickets` — Tickets list

## Quick start

Prerequisites:
- Node.js 14+ (recommended: 18+)
- npm (or yarn/pnpm)

Install dependencies:

```powershell
npm install
```

Start dev server (hot reload):

```powershell
npm run dev
```

Open the app in your browser at the URL printed by Vite (usually `http://localhost:5173`).

## Useful scripts

- npm run dev — start development server
- npm run build — production build
- npm run preview — preview production build locally
- npm run test:unit — run Vitest unit tests
- npm run test:e2e:dev — run Cypress e2e against dev server
- npm run lint — run ESLint

## Routes

The app's client-side routes live in `src/router/index.js`. If a page (for example the Dashboard) does not open, check these:
1. That `router` is imported and used in `src/main.js` (app.use(router)).
2. That the route path `/dashboard` exists in `src/router/index.js` and its component import is correct.

## Troubleshooting — Dashboard not opening

If you navigate to `/dashboard` and nothing shows or you get a blank page:

- Check the browser console (F12) for runtime errors. Common causes:
  - Missing or wrong import in `src/router/index.js` (component not found).
  - A runtime error thrown during component mount (check error stack).
- Verify `src/main.js` mounts the router:

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index.js'

const app = createApp(App)
app.use(router)
app.mount('#app')
```

- Open the terminal where you ran `npm run dev` and inspect the Vite output for build-time errors.
- If you recently renamed branches or files, ensure there are no stale caches; try stopping the dev server and restarting it.

If you want, I can help debug the Dashboard specifically — tell me what you see in the browser console or paste any error messages.

## Contributing

Small improvements and fixes welcome. Typical workflow:

```powershell
git checkout -b fix/your-change
# make changes
git add -A
git commit -m "fix: describe change"
git push origin fix/your-change
# create PR on GitHub
```

## License

MIT
