# ShopSphere – Viva Preparation Guide

## Part 1: Concepts in simple words

**What is React and why use it?**
React is a JavaScript library for building user interfaces from small reusable pieces called components. When data changes, React updates only the parts of the page that need it, so the site feels fast and we avoid writing manual DOM code.

**What is JSX?**
JSX lets us write HTML-like markup inside JavaScript, e.g. `<h1>{product.name}</h1>`. The build tool (Vite) converts it into normal JavaScript. Curly braces `{}` insert JavaScript values.

**Components vs props**
A component is a function that returns JSX (e.g. `ProductCard`). Props are the inputs we pass to it, like function arguments: `<ProductCard product={item} />`. Props are read-only.

**`useState`** stores data that changes over time and re-renders the component when it changes (e.g. the quantity on the details page).
**`useEffect`** runs code after rendering, for side effects such as timers or saving to LocalStorage. The dependency array controls when it runs.
**`useContext`** lets any component read shared data without passing props through every level. We use it for the cart and toast messages.
**`useMemo`** remembers a calculated value (like the filtered product list) and recalculates only when its inputs change.

**How React Router works**
It maps URLs to components without reloading the page. `<Routes>` and `<Route path="/shop" element={<Shop />} />` choose the page; `<Link>` and `<NavLink>` change the URL client-side. `/product/:id` is a dynamic route; `useParams()` reads the id. `path="*"` catches unknown URLs and shows the 404 page.

**How search and filtering work (Shop.jsx)**
Search text, category, min/max price and sort are stored in the URL query string (`useSearchParams`). A `useMemo` runs `products.filter(...)` (name contains text, category matches, price within range) and then `sort(...)` on a copy of the result. If the list is empty we show a "No products found" message.

**How the cart is managed (CartProvider.jsx)**
The cart is an array of `{ id, quantity }`. `CartProvider` wraps the app and shares functions: `addToCart`, `updateQuantity`, `removeFromCart`, `clearCart`. Name, price and image are looked up from the product catalog by id, so product data is stored in one place only. Quantity can never exceed stock.

**Why LocalStorage?**
Without it, the cart would vanish on every refresh. LocalStorage keeps small data in the browser. Our custom hook `useLocalStorage` works like `useState` but also saves and loads the value. Limitation: it is per-browser and not secure, so it is only suitable for demo data.

**How the checkout total is calculated**
`subtotal = sum(price × quantity)`. Shipping is free when the subtotal is $100 or more, otherwise $5.99. `total = subtotal + shipping`. These values are computed with `useMemo` in the cart context, so every page shows the same numbers automatically.

**How GitHub and Vercel are used**
Git tracks versions of the code; GitHub stores the repository online. Vercel connects to the GitHub repo, runs `npm run build`, and hosts the `dist` folder on a free URL. Every push to `main` redeploys. `vercel.json` makes refreshes on routes like `/cart` work.

**Limitations (say these honestly)**
No real backend or database, no user accounts, no real payment gateway, stock is not reduced after an order, and data lives only in the user's browser.

## Part 2: Likely viva questions and answers

1. **What is the virtual DOM?** A lightweight copy of the page kept in memory. React compares the old and new copy and updates only what changed in the real DOM.
2. **Why do list items need a `key`?** So React can identify each item and update lists efficiently. We use the unique product `id`.
3. **What is Context API for? Why not just props?** To avoid "prop drilling": the cart is needed by the navbar, product cards, cart and checkout, so we share it once through context.
4. **Why is the cart saved as `{id, quantity}` and not the whole product?** To avoid duplicated data and prevent stale prices; we always read product details from the catalog.
5. **How do you validate the checkout form?** On submit, a `validate` function checks each field (required, email regex, phone pattern, postal code) and returns an errors object. Errors are shown beside the fields with `role="alert"`; the order is only placed if there are none.
6. **What is a controlled component?** A form input whose value comes from React state (`value={...}` and `onChange`), so React is the single source of truth.
7. **What does `e.preventDefault()` do?** Stops the browser's default form submission, which would reload the page.
8. **What is the difference between `Link` and `<a>`?** `Link` changes the URL without a full page reload; `<a>` reloads the page.
9. **How is the app responsive?** CSS Grid with `auto-fill`, Flexbox, and `@media` queries; the navbar collapses into a hamburger menu on small screens.
10. **How is it accessible?** Labels on all inputs, alt text/aria-labels, a "skip to content" link, visible focus outlines, keyboard-operable buttons, `aria-live` toast messages.
11. **What happens if an image fails to load?** `ProductImage` listens to `onError` and shows a fallback placeholder.
12. **Is the payment real?** No. It is a demo selector; no card data is collected and nothing is charged.
13. **How would you add a real backend?** Replace `products.js` with API calls (e.g. Node/Express or Firebase), store orders in a database, add authentication and a payment provider like Stripe.
14. **What is Vite and why use it instead of create-react-app?** Vite is a fast build tool with instant dev server startup; create-react-app is no longer maintained.
15. **What is `npm run build` doing?** It bundles and minifies the code into the `dist` folder for production.

## Part 3: Project report outline

1. **Title page and abstract**
2. **Introduction** – problem statement, objectives, scope
3. **Technology stack** – React, Vite, React Router, Context API, CSS, LocalStorage, Git/GitHub, Vercel (with reasons)
4. **System design** – page list, folder structure, component diagram, data flow (catalog → components, cart context ↔ LocalStorage)
5. **Implementation** – product catalog, search/filter/sort, cart logic, checkout validation, routing, responsive design, accessibility
6. **Testing** – manual test cases table (search, filters, cart, persistence, checkout validation, 404, mobile layout) and `npm run build`/`npm run lint` results
7. **Deployment** – GitHub repository, Vercel steps, live URL
8. **Screenshots** – each page on desktop and mobile
9. **Limitations and future work** – backend, authentication, real payments, admin panel, reviews, wishlist
10. **Conclusion** and **References** (React, Vite, React Router, Lucide, Vercel documentation)

## Part 4: Quick demo script (about 3 minutes)

1. Home page: hero, categories, featured products.
2. Shop: search "watch", filter by category, set a price range, change sort.
3. Open a product: change quantity (try exceeding stock), add to cart (toast appears).
4. Cart: change quantity, remove an item, **refresh the page** to show LocalStorage persistence.
5. Checkout: submit empty form (validation), fill correctly, place order, show the order number.
6. Visit a wrong URL for the 404 page; shrink the browser window to show the mobile menu.
