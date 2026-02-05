# Landing Page<br><sup>MFE User Journey - Public</sup>

<img src="https://github.com/Ngx-Workshop/.github/blob/main/readme-assets/angular-gradient-wordmark.gif?raw=true" height="132" alt="Angular Logo" /> <img src="https://github.com/Ngx-Workshop/.github/blob/main/readme-assets/module-federation-logo.svg?raw=true" style="max-width: 100%;height: 132px;" alt="Module Federation" />

Angular micro-frontend (remote) for the **Public Landing Page** user journey in the NGX Workshop ecosystem.

Angular 21 standalone micro front-end (remote) that renders the NGX Workshop landing experience and exposes its root component through Module Federation. The remote targets port 4201 by default and ships Material-driven styling for quick embedding inside a shell/host.

### Quick Start

- Prereqs: Node 18+ and npm.
- Install: `npm install`
- Dev server: `npm start` (serves at http://localhost:4201 with live reload)
- Unit tests: `npm test`
- Production build: `npm run build` (outputs to `dist/mfe-user-journey-landing-page`)
- Bundle dev loop (watch + static serve): `npm run dev:bundle` (wraps `npm run watch` and serves via `http-server` on port 4201)

### Repo Snapshot

- Angular CLI configured with `ngx-build-plus` for Module Federation: see [angular.json](angular.json) and [webpack.config.js](webpack.config.js).
- Standalone root component and template: [src/app/app.ts](src/app/app.ts) with styles in [src/app/app.scss](src/app/app.scss).
- Bootstrapping via deferred `bootstrap.ts` import from [src/main.ts](src/main.ts) and application providers in [src/app/app.config.ts](src/app/app.config.ts).
- Global styles placeholder at [src/styles.scss](src/styles.scss); static assets served from `public/`.

### Architectural Overview

- **Standalone Angular app**: Uses `bootstrapApplication` to mount the `App` component without NgModules, enabling tree-shakable configuration and zoneless change detection.
- **Module Federation remote**: `withModuleFederationPlugin` exposes `./Component` mapped to the root component for hosts to consume; remote name `ngx-landing-page-mfe`, manifest emitted as `remoteEntry.js` ([webpack.config.js](webpack.config.js)). Shared singletons include Angular core/router/forms, Material/CDK, `rxjs`, and `tslib` to keep host/remote versions aligned.
- **Runtime config**: Providers add browser error listeners, zoneless change detection, and async animations setup ([src/app/app.config.ts](src/app/app.config.ts)).
- **UI composition**: `App` renders the landing hero and three CTA tiles (NestJS, Angular, RxJS) using Angular Material buttons/icons and `RouterLink` navigation stubs ([src/app/app.ts](src/app/app.ts)). Styling leans on Material design tokens and responsive layout rules in [src/app/app.scss](src/app/app.scss).
- **Build & serve**: CLI build targets use `ngx-build-plus:browser` with custom webpack config and production hashing; dev server bound to port 4201 and configured as a Module Federation remote ([angular.json](angular.json)).

### How to Consume as a Remote

1. Start the remote: `npm start` (or serve the production bundle via `npm run dev:bundle`).
2. Point your host shell’s Module Federation config at `http://localhost:4201/remoteEntry.js` with remote key `ngx-landing-page-mfe` and import exposed module `./Component`.
3. Render the exposed component in the host’s route/component as needed.

### Notes

- Default styles are minimal; extend `src/styles.scss` or adjust `app.scss` for host branding.
- Assets should be placed under `public/` to be copied through the Angular build pipeline.
