**Type:** [[documentation]]  
**Topics:** #  
**Date:** 2024-08-01  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---
# Pre-requisite Knowledge List
- [ ] [React Foundations Course](https://nextjs.org/learn/react-foundations) – Brush up on react
- [ ] [building a dashboard application](https://nextjs.org/learn/dashboard-app) – Learn Next.js

# Notes

````ad-abstract
I will be taking notes on the [official documentation](https://nextjs.org/docs) of **Next.js**, a *React framework*. The following section is for the **App Router**.
```ad-note
title: App Router vs Pages Router
*App router* is the newer router which supports Next.js newer features including Server Components and Streaming.

*Pages Router* allowed for server-rendered React Applications.
```
````

## Overview

Some key points on Next.js:
- Next.js is a React framework
- Use React Components to build UI, and Next.js for additional features and optimizations.
- abstracts and automatically configures stuff

```ad-tip
title: Features of Next.js
The main features of Next.js include
- [[Routing]] : *file-system* based router, on top of *Server Components* that supports: *layouts*, *nested routing*, *loading states*, *error handling*, etc.
- [[Rendering]] : Client-side and Server-side Rendering with client and server components. *Further optimized* with **Static** or **Dynamic** rendering on the server with Next.js
- [[Data Fetching]] : Simplified data fetching with async/await in Server Components, and an extended `fetch` API for request memoization, data caching and revalidation.
- [[Styling]] : support for your preferred styling methods, including Tailwind CSS, CSS modules, and CSS-in-JS.
- [[Optimizations]] : Image, Fonts and script optimizations
```

## Installation

```ad-info
title: System Requirements
- [Node.js 18.17](https://nodejs.org/) or later – JS run-time environment
- macOS, Windows (including WSL), and Linux are supported.
```

For automatic installation run

```sh
npx create-next-app@latest
```
*manual installation is possible, but omitted*.

After the prompts, `create-next-app` will *create a folder* with your project name and install the required dependencies.

In `package.json` under `"scripts"`, the default scripts mean the following:
- `dev`: runs [`next dev`](https://nextjs.org/docs/app/api-reference/next-cli#development) to start Next.js in development mode.
- `build`: runs [`next build`](https://nextjs.org/docs/app/api-reference/next-cli#build) to build the application for production usage.
- `start`: runs [`next start`](https://nextjs.org/docs/app/api-reference/next-cli#production) to start a Next.js production server.
- `lint`: runs [`next lint`](https://nextjs.org/docs/app/api-reference/next-cli#lint) to set up Next.js' built-in ESLint configuration.

Next.js uses **file-system routing** – i.e. the routes are determined by how you structure your files (under the `app` or `pages` directory).

![[Pasted image 20240801124336.png]]
*`layout.tsx` and `page.tsx` will be rendered when a user visits the root of your application i.e. url : `[app.domain]/`*

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```
*layout.tsx*

- [!] **Pages Routing section omitted**

`npm run dev` to view any changes during development.

## Project Structure

```ad-summary
title: Top-Level
**Top-Level Folders** are used to organize your application's *code* and *static assets*.

![[Pasted image 20240801125349.png]]
- `app`    : App Router
- `pages`  : Pages Router
- `static` : Static Assets to be Served
- `src`    : *(Optional)* applicatio source folder.

**Top-Level Files** are used to 
1. *Configure* your application
2. Manage *Dependencies*
3. run *middleware*
4. Integrate *monitoring tools*
5. define *environment variables*

![[Screenshot 2024-08-01 at 12.59.57 PM.png]]
```

```ad-summary
title: `app` Routing Conventions
The following file conventions are used to *define routes* and *handle metadata* in the `app` router.

*these files must have `.js`, `.jsx` or `.tsx` extensions*
- `layout` : Layout
- `page` : Page
- `loading` : Loading UI
- `not-found` : Not Found UI
- `error` : Error UI
- `global-error` : Global Error UI
- `route` : API endpoint
- `template` : Re-rendered layout
- `default` : parallel route fallback page

### Nested Routes in Next.js
- `folder` : route segment
- `folder/folder` : nested route segment

### Dynamic Routes
- `[folder]` : Dynamic route segment
- `[...folder]` : Catch-all route segment
- `[[...folder]]` : Optional catch-all route segment

### Route Groups and Private Folders
- `(folder)` : Group Routes without affecting routing
- `_folder` : Opt folder and all child segments out of routing
	- Use for assets, components and utils to ensure proper functionality.

### Parallel and Intercepted Routes
- `@folder` : Named slot
- `(.)folder`: Intercept same level
- `(..)folder`: Intercept one level above
- `(..)(..)folder`: Intercept two levels above
- `(...)folder`: Intercept from root 
```

````ad-summary
title: Metadata File Conventions
```ad-success
title: App Icons
||||
|---|---|---|
|[`favicon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#favicon)|`.ico`|Favicon file|
|[`icon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#icon)|`.ico` `.jpg` `.jpeg` `.png` `.svg`|App Icon file|
|[`icon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#generate-icons-using-code-js-ts-tsx)|`.js` `.ts` `.tsx`|Generated App Icon|
|[`apple-icon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#apple-icon)|`.jpg` `.jpeg`, `.png`|Apple App Icon file|
|[`apple-icon`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons#generate-icons-using-code-js-ts-tsx)|`.js` `.ts` `.tsx`|Generated Apple App Icon|
```
```ad-success
title: Open Graph and Twitter Images
*OMITTED*
```
```ad-success
title: SEO (Search Engine Optimizations)
||||
|---|---|---|
|[`sitemap`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#sitemap-files-xml)|`.xml`|Sitemap file|
|[`sitemap`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#generating-a-sitemap-using-code-js-ts)|`.js` `.ts`|Generated Sitemap|
|[`robots`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#static-robotstxt)|`.txt`|Robots file|
|[`robots`](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#generate-a-robots-file)|`.js` `.ts`|Generated Robots file|
```
```ad-failure
title: `pages` Routing docs OMITTED
```
````
## Routing

### Fundamentals

````ad-summary
title: Terminology
![[Pasted image 20240801144028.png]]
*a sample diagram of the hierarchy of the `app` directory*
```ad-tip
to visualize it on your CLI, use `tree app` in the project root.
```
- **Tree:** A convention for visualizing a hierarchical structure. For example, a component tree with parent and children components, a folder structure, etc.
- **Subtree:** Part of a tree, starting at a new root (first) and ending at the leaves (last).
- **Root**: The first node in a tree or subtree, such as a root layout.
- **Leaf:** Nodes in a subtree that have no children, such as the last segment in a URL path.

![[Pasted image 20240801144320.png]]

- **URL Segment:** Part of the URL path delimited by slashes.
- **URL Path:** Part of the URL that comes after the domain (composed of segments).
````

### `app` Router

By default, components inside the `app` directory are *React Server Components*.

Next.js uses a file-system based routing where
- *Folders* are used to define routes. A *URL Path* is defined from the root directory to wherever a folder has a *page* `.js` – or equivalent – file.

![[Pasted image 20240801154920.png]]
Each folder represents a **route segment**. This is also known as *nested routes*.
1. `app` - The *root segment* `/`
2. `dashboard` - an *intermediate* route segment
3. `settings` – the *leaf* route segment.

### Component Hierarchy

The *React Components* defined in **special file naming convention** have a specific heirarchy:
![[Pasted image 20240801155319.png]]
1. `layout.js`
2. `template.js`
3. `error.js` (React error boundary)
4. `loading.js` (React suspense boundary)
5. `not-found.js` (React error boundary)
6. `page.js` or nested `layout.js`

```ad-important
title: Nested Route Inheritance
In a nested route, the (special named) components of a segment will be nested **inside** the components of its parent segments.

![[Pasted image 20240801155626.png]]

```

### Colocation

```ad-summary
title: Colocation of Files
In addition to special files, you have the ability to colocate your own files (e.g. components, styles, tests, etc) inside folders in the `app` directory.

This is because while folders define routes, only the contents returned by `page` or `route` are publicly addressable.

![[Pasted image 20240801160309.png]]
```

### Advanced Routing

```ad-summary
title: Parallel Routes
Allows you to *simultaneously* show two or more pages in the same view that can be *navigated independently*.

Example Uses:
- Split views that have their own *sub-navigation*. E.g. Dashboards.
```

```ad-summary
title: Intercepting Routes
Allows you to intercept a route and show it in the context of another route. You can use these when you want to keep the content of the current page the same.

Example Uses:
- Seeing all tasks while editing one task or expanding a photo in a feed.
```

### Layout (`layout`)

You can define a layout by default exporting a React component from a `layout.js` file. The component should accept a `children` prop that will be populated with a child layout (if it exists) or a page during rendering.

- *shared* between multiple routes.
- On navigation, layouts *preserve state*, *remain active*, *do not re-render*.

````ad-success
title: Example
In this example, layout will be shared in the `dashboard` and `dashboard/settings` route.

![[Pasted image 20240801161252.png]]
```tsx
export default function DashboardLayout({
  children, // will be a page or nested layout
}: {
  children: React.ReactNode
}) {
  return (
    <section>
      {/* Include shared UI here e.g. a header or sidebar */}
      <nav></nav>
 
      {children}
    </section>
  )
}
```
````

```ad-abstract
title: Nested Layouts
![[Pasted image 20240801162110.png]]

If you were to combine the two layouts above, the root layout (`app/layout.js`) would wrap the dashboard layout (`app/dashboard/layout.js`), which would wrap route segments inside `app/dashboard/*`.
```


---

## Questions

```ad-abstract
title: Common Questions
Taken from [App Router Docs](https://nextjs.org/docs/app)
- How to access *request object* in a *layout*?
- How to access URL in a *page*?
- How can I *redirect* from a Server Component?
- How can I handle *authentication* with App Router?
- How can I set *cookies*?
- How can I build *multi-tenant* apps?
- How can I *invalidate* the App Router *cache*?
- Are there any comprehensive open-source applications built on App Router?
```

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
