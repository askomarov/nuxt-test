# Nuxt Test

A [Nuxt 3](https://v3.nuxtjs.org) application with Tailwind CSS, featuring a music player, multi-page navigation, and reusable Vue components.

## Tech Stack

- **[Nuxt 3](https://v3.nuxtjs.org)** — Vue 3 meta-framework (static target, SSR disabled)
- **[Tailwind CSS](https://tailwindcss.com)** via `@nuxtjs/tailwindcss`
- **[VueUse](https://vueuse.org)** (`@vueuse/core`) — composition utility library
- **[Headless UI](https://headlessui.com/vue)** (`@headlessui/vue`) — accessible UI primitives
- **Pug** — HTML template pre-processor
- **SCSS** — CSS pre-processor

## Pages

| Route | Description |
|-------|-------------|
| `/` | Music Player |
| `/about` | About page |
| `/team` | Team page |
| `/contact` | Contact page |
| `/search` | TV Shows search |

## Components

- `VHeader` — Site header with responsive navigation
- `VMusicPlayer` — Music player component
- `VButton` / `VButtonTS` — Button components
- `ButtonPlay` — Play button
- `Counter` — Counter component

## Setup

Install the dependencies:

```bash
# yarn
yarn install

# npm
npm install

# pnpm
pnpm install --shamefully-hoist
```

## Development Server

Start the development server on <http://localhost:3000>:

```bash
npm run dev
```

## Production

Build the application for production:

```bash
npm run build
```

Generate a static site:

```bash
npm run generate
```

Locally preview the production build:

```bash
npm run preview
```

Check out the [deployment documentation](https://v3.nuxtjs.org/guide/deploy/presets) for more information.
