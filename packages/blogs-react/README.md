# DITBlogs React Components - @dishistech/blogs-react

[![NPM Version](https://img.shields.io/npm/v/@dishistech/blogs-react?color=blue)](https://www.npmjs.com/package/@dishistech/blogs-react)
[![License](https://img.shields.io/npm/l/@dishistech/blogs-react)](https://github.com/YourGitHub/DITBlogs-JS/blob/main/LICENSE)

The official, production-ready React & Next.js component library for the [blogs.dishis.tech](https://blogs.dishis.tech) API.

Instantly add a full-featured, beautifully themed, and performant blog to your React or Next.js application with minimal configuration. This library handles all the data fetching, state management, and rendering for you.

## Features

-   **Ready-to-Use**: Full-page components like `<BlogLayout>` and `<PostsListPage>` to get started in minutes.
-   **Deeply Customizable**: A powerful theming system supports `"light"`, `"dark"`, and your own custom color palettes via CSS variables.
-   **Component-Driven**: Use granular components like `<PostCard>` or `<CommentsSection>` to build custom layouts.
-   **State Management Handled**: Built-in loading states (with skeletons), error states, and empty states.
-   **Pagination Included**: The `<Pagination>` component is automatically rendered for post lists.
-   **Framework Agnostic Links**: Bring your own `Link` component from Next.js, React Router, or any other routing library.
-   **Fully Typed**: Written in TypeScript for a superior developer experience.

## Installation

This package has peer dependencies that must be installed alongside it.

```bash
npm install @dishistech/blogs-react @dishistech/blogs-sdk react react-dom swr lucide-react
# or
pnpm add @dishistech/blogs-react @dishistech/blogs-sdk react react-dom swr lucide-react
```

## Quick Start with Next.js App Router

Get a full blog up and running in 3 steps.

#### 1. Configure Tailwind CSS

Add the path to the library's components to your `tailwind.config.js` file so Tailwind can process its classes. You must also configure colors to use our CSS variables for theming to work.

```js
// tailwind.config.js
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx}',
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
    // Add the path to the installed library
    './node_modules/@dishistech/blogs-react/dist/**/*.js', 
  ],
  theme: {
    extend: {
      // Theming is done via CSS variables, so we define them here
      colors: {
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        card: 'hsl(var(--card))',
        cardForeground: 'hsl(var(--card-foreground))',
        primary: 'hsl(var(--primary))',
        primaryForeground: 'hsl(var(--primary-foreground))',
        muted: 'hsl(var(--muted))',
        mutedForeground: 'hsl(var(--muted-foreground))',
      },
    },
  },
  plugins: [],
}
```

#### 2. Wrap Your Root Layout

In your main `app/layout.tsx`, wrap the content with `<BlogLayout>`. This provides the theme, API client, and overall page structure.

```tsx
// app/layout.tsx
import { BlogLayout } from '@dishistech/blogs-react';
import Link from 'next/link'; // Your app's Link component
import './globals.css';

// Optional: Define nav links for the header
const navLinks = [
  { href: "/", label: "Home" },
  { href: "/categories", label: "Categories" },
];

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <BlogLayout
          apiKey={process.env.NEXT_PUBLIC_DITBLOGS_API_KEY!}
          theme="dark" // or "light", or a custom theme object
          navLinks={navLinks}
          linkComponent={Link} // Pass your Link component here
        >
          {children}
        </BlogLayout>
      </body>
    </html>
  );
}
```

#### 3. Create Your Blog Pages

Now, just use the pre-built page components.

**Blog Index Page:**
```tsx
// app/page.tsx
import { PostsListPage } from '@dishistech/blogs-react';

export default function HomePage() {
  return (
    <>
      <h1 className="text-5xl font-extrabold text-center mb-16">
        Our Latest Articles
      </h1>
      <PostsListPage />
    </>
  );
}
```

**Single Post Page:**
```tsx
// app/posts/[slug]/page.tsx
import { PostDetailsPage } from '@dishistech/blogs-react';

// You might get the user's session token here
// const userToken = await getUserSession(); 

export default function PostPage({ params }: { params: { slug: string } }) {
  return (
    <PostDetailsPage 
      slug={params.slug}
      // userToken={userToken} // Pass this to enable comment posting
    />
  );
}
```

## Custom Theming

You have three options for the `theme` prop on `<BlogLayout>`:

1.  `"light"` (Default)
2.  `"dark"`
3.  A custom theme object.

To create a custom theme, provide an object that matches the `CustomTheme` interface. The library will apply your colors as CSS variables.

```tsx
import { BlogLayout, CustomTheme } from '@dishistech/blogs-react';

const myCustomTheme: CustomTheme = {
  colors: {
    background: "#0d0d0d",         // A deep black background
    foreground: "#e5e5e5",         // A light gray text
    card: "#1a1a1a",               // A slightly lighter card background
    cardForeground: "#e5e5e5",     // Text on cards
    primary: "#ffa500",            // A vibrant orange for primary actions
    primaryForeground: "#0d0d0d",  // Text on primary buttons
    muted: "#262626",              // Muted backgrounds (e.g., for forms)
    mutedForeground: "#a3a3a3",    // Muted text (e.g., for dates)
  }
};

<BlogLayout theme={myCustomTheme} ... >
  {/* ... */}
</BlogLayout>
```

## Core Components API

### `<BlogLayout>`

The main wrapper for your application.

-   `apiKey: string` (Required)
-   `theme?: "light" | "dark" | CustomTheme` (Optional)
-   `linkComponent?: ElementType` (Optional) - Your routing library's Link component. Defaults to `<a>`.
-   `navLinks?: { href: string; label: string }[]` (Optional) - Links to display in the header.

### Page Components

These components are designed to be the primary content of your page routes.

-   **`<PostsListPage>`**: Displays a paginated list of all posts.
    -   `category?: string` - Filter by category slug.
    -   `tag?: string` - Filter by tag slug.
-   **`<PostDetailsPage>`**: Displays a single post and its comments.
    -   `slug: string` (Required)
    -   `userToken?: string` - Auth token of the logged-in user to enable commenting.
-   **`<CategoryPage>`**: Displays information about a category and a paginated list of its posts.
    -   `slug: string` (Required)
-   **`<TagPage>`**: Displays a paginated list of posts with a specific tag.
    -   `slug: string` (Required)
-   **`<CategoriesListPage>`**: Displays a grid of all available categories.
-   **`<TagsListPage>`**: Displays a list of all available tags.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any bugs or feature requests.

## License

[MIT](https://github.com/YourGitHub/DITBlogs-JS/blob/main/LICENSE)