# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Starter project for a Claude Code course: a basic React expense tracker that **intentionally** ships with a bug, poor UI, and messy code, which get fixed incrementally. Expect to find rough edges; don't assume existing patterns are the ones to follow.

## Commands

```bash
npm install       # install dependencies
npm run dev       # Vite dev server at http://localhost:5173
npm run build     # production build to dist/
npm run preview   # serve the production build
npm run lint      # ESLint over the whole project
```

There is no test runner configured yet.

## Architecture

React 19 + Vite 7, plain JavaScript (JSX, no TypeScript), no router, no state library, no backend or persistence: data lives in memory and resets on reload.

- `src/main.jsx` mounts `<App />` in `StrictMode`.
- `src/App.jsx` holds the entire app in one component: seed transactions, add-transaction form state, type/category filters, and derived totals (income, expenses, balance) computed on every render. Categories are a hard-coded array in the same file.
- Styling is global CSS: `src/index.css` (base) and `src/App.css` (component classes such as `.summary-card`, `.income-amount`, `.expense-amount`).

Transaction shape: `{ id, description, amount, type: "income" | "expense", category, date: "YYYY-MM-DD" }`. Note that `amount` is stored as a **string** (both in seed data and from the form input), so the `reduce` sums in `App.jsx` concatenate strings instead of adding numbers. This is the known bug; keep it in mind when touching totals.

ESLint uses the flat config (`eslint.config.js`) with the React Hooks and React Refresh rules; `no-unused-vars` ignores names starting with an uppercase letter or underscore.
