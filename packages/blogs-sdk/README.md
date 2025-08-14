# DITBlogs API SDK - @dishistech/blogs-sdk

[![NPM Version](https://img.shields.io/npm/v/@dishistech/blogs-sdk?color=blue)](https://www.npmjs.com/package/@dishistech/blogs-sdk)
[![License](https://img.shields.io/npm/l/@dishistech/blogs-sdk)](https://github.com/YourGitHub/DITBlogs-JS/blob/main/LICENSE)

Official, lightweight, and type-safe TypeScript SDK for the [blogs.dishis.tech](https://blogs.dishis.tech) API.

This SDK provides a simple, promise-based interface to interact with all API endpoints. It can be used in any JavaScript or TypeScript environment, including Node.js backends, serverless functions, or even in the browser.

## Features

-   **Type-Safe**: Fully written in TypeScript to provide excellent autocompletion and prevent common errors.
-   **Lightweight**: No external dependencies, keeping your project lean.
-   **Promise-Based**: Uses modern `async/await` syntax for clean, readable code.
-   **Isomorphic**: Works seamlessly in both Node.js and browser environments.
-   **Comprehensive**: Covers all available API endpoints.

## Installation

```bash
npm install @dishistech/blogs-sdk
# or
yarn add @dishistech/blogs-sdk
# or
pnpm add @dishistech/blogs-sdk
```

## Quick Start

Instantiate the client with your API key and start making requests.

```typescript
import { DITBlogsClient } from '@dishistech/blogs-sdk';

// It's recommended to store your API key in environment variables
const client = new DITBlogsClient(process.env.DITBLOGS_API_KEY!);

async function fetchRecentPosts() {
  try {
    console.log('Fetching the 5 most recent posts...');
    const response = await client.getPosts({ limit: 5 });

    console.log(`Found ${response.pagination.total} total posts.`);
    response.posts.forEach(post => {
      console.log(`- ${post.title} (slug: ${post.slug})`);
    });
  } catch (error) {
    console.error('Failed to fetch posts:', error.message);
  }
}

fetchRecentPosts();
```

## API Reference

All methods return a `Promise` that resolves with the data from the API.

### `new DITBlogsClient(apiKey)`

Creates a new API client instance.

-   `apiKey` (string, required): Your secret API key from the DITBlogs dashboard.

---

### Posts

#### `client.getPosts(params)`

Retrieves a paginated list of published posts.

-   `params` (object, optional):
    -   `category?: string` - Filter by category slug.
    -   `tag?: string` - Filter by tag slug.
    -   `page?: number` - The page number to retrieve. Defaults to `1`.
    -   `limit?: number` - The number of posts per page. Defaults to `10`.
-   **Returns**: `Promise<PostsResponse>`

#### `client.getPost(slug)`

Retrieves a single published post by its unique slug.

-   `slug` (string, required): The slug of the post.
-   **Returns**: `Promise<Post>`

---

### Categories

#### `client.getCategories()`

Retrieves a list of all categories.

-   **Returns**: `Promise<Category[]>`

#### `client.getCategory(slug, params)`

Retrieves a single category and a paginated list of its posts.

-   `slug` (string, required): The slug of the category.
-   `params` (object, optional):
    -   `page?: number`
    -   `limit?: number`
-   **Returns**: `Promise<CategoryResponse>`

---

### Tags

#### `client.getTags()`

Retrieves a list of all tags.

-   **Returns**: `Promise<Tag[]>`

#### `client.getTag(slug, params)`

Retrieves a single tag and a paginated list of its posts.

-   `slug` (string, required): The slug of the tag.
-   `params` (object, optional):
    -   `page?: number`
    -   `limit?: number`
-   **Returns**: `Promise<TagResponse>`

---

### Comments

#### `client.getComments(postSlug)`

Retrieves all comments for a post, structured hierarchically.

-   `postSlug` (string, required): The slug of the post.
-   **Returns**: `Promise<Comment[]>`

#### `client.postComment(params)`

Submits a new comment or a reply.

-   `params` (object, required):
    -   `postSlug: string` - The slug of the post being commented on.
    -   `content: string` - The text content of the comment.
    -   `userToken: string` - The authentication token (e.g., JWT) of the end-user posting the comment.
    -   `parentId?: string` - The ID of the parent comment if this is a reply.
-   **Returns**: `Promise<Comment>`

## Error Handling

If the API returns an error (any non-2xx status code), the promise will be rejected. You should wrap your API calls in a `try...catch` block to handle failures gracefully.

```typescript
async function fetchInvalidPost() {
  try {
    const post = await client.getPost('this-slug-does-not-exist');
    console.log(post);
  } catch (error) {
    // error.message will contain the JSON error response from the API
    console.error(error.message); 
    // Example output: API Error (404): "{\"error\":\"Post not found.\"}"
  }
}
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any bugs or feature requests.

## License

[MIT](https://github.com/YourGitHub/DITBlogs-JS/blob/main/LICENSE)