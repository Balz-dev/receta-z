## Copilot / Agentes: instrucciones rápidas para este repo

Breve: este repo es una PWA Next.js (`next@16`) con TypeScript y Tailwind. Usa Dexie para almacenamiento local (IndexedDB), `next-pwa` para PWA, `react-pdf` para generación de PDFs y varias utilidades UI (Radix, lucide). El código fuente principal vive bajo `src/` (alias `@/*` en `tsconfig.json`).

- Punto de entrada de desarrollo: `npm run dev` → `next dev`.
- Build de producción: `npm run build` → `next build`.
- Tests unitarios: `npm run test` → `jest`.
- Tests E2E: `npm run test:e2e` (requiere `npm run playwright:install` primero).

Qué necesita saber un agente para ser efectivo

1. Arquitectura y decisiones clave
  - Frontend Next.js (SSR/SSG/Client components según archivo). Mantén la convención de colocar código en `src/`.
  - PWA: `next-pwa` configura la aplicación como PWA. Espera código relacionado a service workers y manifest en la configuración de Next/`public/`.
  - Offline/local: usa `dexie` y `dexie-react-hooks` para persistencia en IndexedDB (buscar archivos que importen `dexie`).
  - PDF: `@react-pdf/renderer` se utiliza para exportar recetas; revisa usos para mantener compatibilidad.

2. Convenciones de proyecto (detectables en el repo)
  - TypeScript estricto (ver `tsconfig.json`): mantén tipos correctos y no emitir JS.
  - Alias de importación: `@/*` → `./src/*` (usa este alias cuando modifiques/añadas módulos).
  - UI: componentes y clases usan Tailwind + `class-variance-authority` y `clsx` para variantes; sigue patrones existentes en `src/components` si hay.
  - Formulario/Validación: `react-hook-form` + `zod` están presentes; cuando añadas formularios respeta esa combinación.

3. Flujo de desarrollo local y debugging
  - Arranque rápido: `npm install` → `npm run dev`.
  - Para problemas de PWA/Service Worker, reproducir en build: `npm run build` && `npm run start` y probar en navegador.
  - Para reproducir errores de IndexedDB/Dexie, abrir DevTools → Application → IndexedDB.

4. Scripts concretos (ejemplos desde `package.json`)
  - Desarrollo: `npm run dev` (Next dev server).
  - Compilar: `npm run build`.
  - Iniciar producción local: `npm run start`.
  - Tests unitarios: `npm run test` (Jest + jsdom env).
  - E2E: `npm run playwright:install` y luego `npm run test:e2e`.

5. Integration points y dependencias externas
  - `next-pwa` (PWA behavior) — revisar `next.config.js` por opciones si se necesita ajustar cache/strategy.
  - `dexie` para storage local; revisar esquemas y migraciones si se cambia el modelo de datos.
  - `@react-pdf/renderer` para generación de PDFs (compatibilidad con React 19 debe respetarse).

6. Archivos y lugares para mirar primero (ejemplos concretos)
  - `package.json` — scripts y dependencias principales.
  - `tsconfig.json` — paths (`@/*`) y reglas de compilación.
  - `next.config.js` — opciones Next (PWA puede emerger aquí).
  - `recetas/` — actualmente contiene `Recetas Médicas - Sistema de Gestión.pdf` (especificación/diseño del dominio). Usar como referencia de requisitos de negocio.

7. Cómo proponer cambios con seguridad
  - Para cambios en el esquema de Dexie o en la generación de PDFs, dejar notas en el PR sobre migraciones de datos y pasos para probar localmente.
  - Ejecutar `npm run test` y, si aplica, `npm run test:e2e` antes de abrir PR.

8. Lo que NO asumir
  - No hay CI/CD visible en el repo (no hay `.github/workflows/`); no des por sentado un pipeline. Pregunta si se debe añadir uno.

Si falta algo específico (rutas concretas en `src/`, ubicaciones de composants, convenciones de commits, o flujos E2E detallados), dime qué parte prefieres que amplíe y lo incorporo.
