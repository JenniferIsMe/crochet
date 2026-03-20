# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # Start Vite dev server
npm run build     # Type-check with vue-tsc, then bundle with Vite
npm run preview   # Preview production build locally
```

There is no linter or test runner configured.

## Architecture

**Stack**: Vue 3 (Composition API with `<script setup>`) + TypeScript + Vite. No router, no state management library (Pinia is planned per the PRD but not yet added).

**Entry flow**: `index.html` → `src/main.ts` → `src/App.vue` → `src/components/HelloWorld.vue`

The real application logic lives entirely in `src/components/HelloWorld.vue`. `App.vue` is a thin shell that renders it. The component implements a swipe-gesture carousel for tracking progress across multiple knitting/crochet tasks — each task has a current count, target count, and progress bar. State is managed with Vue `ref()` and `computed()`.

**Product vision**: `src/assets/crochet.md` is the full PRD for "织心 (YarnHub)". The current codebase is an MVP. Planned additions include Pinia, Vant UI, PWA support, and a Java Spring Boot backend with MySQL.
