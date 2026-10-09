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
- `src/App.jsx` owns the `transactions` state (seeded with sample data) and the hard-coded `categories` array, and composes three child components. It passes `addTransaction` and `deleteTransaction` down as the only ways to change the list.
  - `src/Summary.jsx`: receives `transactions` and computes income, expense and balance totals on every render.
  - `src/AddTransaction.jsx`: owns the form field state; on submit it builds a transaction and calls `onAdd`, then resets the form.
  - `src/TransactionList.jsx`: owns the type/category filter state and renders the filtered table, with a delete button per row that asks for confirmation (`window.confirm`) before calling `onDelete(id)`.
- Styling is global CSS: `src/index.css` (base) and `src/App.css` (component classes such as `.summary-card`, `.income-amount`, `.expense-amount`), imported once in `App.jsx`.

Transaction shape: `{ id, description, amount, type: "income" | "expense", category, date: "YYYY-MM-DD" }`. `amount` must be a **number**: the form input yields a string, so `AddTransaction`'s `handleSubmit` converts it with `parseFloat` before storing. The totals are computed with `reduce` and would concatenate strings instead of adding them.

ESLint uses the flat config (`eslint.config.js`) with the React Hooks and React Refresh rules; `no-unused-vars` ignores names starting with an uppercase letter or underscore.
