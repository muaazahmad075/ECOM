# ShopSphere – React E-Commerce Demo

A modern, responsive e-commerce website built with **React 19**, **Vite**, **React Router** and **Lucide icons** as a university web engineering project.

> **Demo only.** There is no backend and no payment gateway. Orders, newsletter sign-ups and contact messages are simulated in the browser. Nothing is sent to a server.

## Features

- Home, Shop, Product Details, Cart, Checkout, Order Confirmation, About, Contact and 404 pages
- 20 sample products in 4 categories with search, category filter, price range and 6 sort options
- Filters live in the URL (e.g. `/shop?category=fashion&sort=price-asc`), so results are shareable
- Shopping cart with quantity limits based on stock, saved in LocalStorage (survives refresh)
- Checkout form with validation, demo payment selector and generated demo order number
- Toast notifications, loading skeletons, empty and error states, image fallback
- Responsive layout (mobile, tablet, desktop), keyboard-friendly and screen-reader-friendly

## Tech stack

React 19 · Vite · React Router · Context API + Hooks · Lucide React · plain CSS (Flexbox + Grid) · LocalStorage

## Run locally

Requires **Node.js 20+** (check with `node -v`).

```bash
npm install
npm run dev        # development server at http://localhost:5173
npm run build      # production build into dist/
npm run preview    # serve the production build locally
npm run lint       # check code with oxlint
```

## Project structure

```
src/
├── components/   Reusable UI (Navbar, Footer, ProductCard, ProductGrid, SearchBar, Filters, CartItem, Button, ...)
├── pages/        One file per route (Home, Shop, ProductDetails, Cart, Checkout, ...)
├── context/      CartContext + CartProvider, ToastContext + ToastProvider
├── hooks/        useCart, useToast, useLocalStorage, useAddToCart
├── data/         products.js (catalog, categories, sort options)
├── utils/        format.js (price formatting, shipping, validation helpers)
├── styles/       global.css, components.css, pages.css
├── App.jsx       Routes and page layout
└── main.jsx      App entry point (providers + router)
public/
└── images/products/   Local SVG product images (no external image dependency)
```

## Push to GitHub

1. Create an **empty** repository on GitHub (no README or .gitignore), e.g. `shopsphere`.
2. In the project folder run:

```bash
git init
git add .
git commit -m "Initial commit: ShopSphere React e-commerce app"
git branch -M main
git remote add origin https://github.com/<your-username>/shopsphere.git
git push -u origin main
```

`node_modules`, `dist` and `.env` files are excluded by `.gitignore`.

## Deploy on Vercel (free)

1. Sign in at [vercel.com](https://vercel.com) with your GitHub account.
2. Click **Add New → Project** and import the `shopsphere` repository.
3. Vercel detects **Vite** automatically. Keep the defaults:
   - Build command: `npm run build`
   - Output directory: `dist`
4. Click **Deploy**. You get a live URL like `https://shopsphere-xyz.vercel.app`.

`vercel.json` rewrites every path to `index.html`, so refreshing on routes like `/cart` or `/product/3` works instead of showing a 404. Every `git push` to `main` redeploys automatically.

## Limitations

- No real backend or database: products are a local JavaScript file.
- No user accounts or login.
- No real payments: the payment options are a demo selector and no card details are collected.
- Cart, last order and newsletter email are stored only in the visitor's own browser.
