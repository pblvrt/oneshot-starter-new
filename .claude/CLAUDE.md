# Vibecoder Starter Template

You are helping a vibecoder (non-technical user) build their app idea. Your job is to read their app description and build it for them.

**The current code is just a starter template.** Feel free to completely modify, replace, or delete any existing pages and components to match what the user wants. Nothing is sacred—rebuild everything to fit their vision.

## Your Workflow

1. **Read the app spec** in `documentation/` folder
2. **Build the app** using the stack below
3. **Test it** in the browser at port 3000
4. **Iterate** based on feedback

## Environment

This repo runs in a **GitHub Codespace**. Everything is pre-configured:

- **Port 3000**: Next.js app (auto-opens)
- **Port 8090**: PocketBase database + Admin UI at `/_/`

### URL Routing (Important!)

The `.devcontainer/setup.sh` generates two PocketBase URLs:

| Variable                     | Value                               | Used By           |
| ---------------------------- | ----------------------------------- | ----------------- |
| `NEXT_PUBLIC_POCKETBASE_URL` | `https://{codespace}-8090.{domain}` | Browser           |
| `POCKETBASE_URL`             | `http://pocketbase:8090`            | Server (internal) |

Browser can't reach Docker internal network, so it uses the public Codespace URL.

## Stack

| Tool        | Purpose         | Location             |
| ----------- | --------------- | -------------------- |
| Next.js 16  | App framework   | `src/app/`           |
| Tailwind v4 | Styling         | Classes in JSX       |
| shadcn/ui   | UI components   | `src/components/ui/` |
| PocketBase  | Auth + Database | `@/lib/pocketbase`   |
| OpenRouter  | AI/LLM          | `@/lib/ai`           |

## Building Pages

Next.js App Router uses file-based routing:

```
src/app/
├── page.tsx              → /
├── login/page.tsx        → /login
├── dashboard/page.tsx    → /dashboard
├── posts/[id]/page.tsx   → /posts/123
└── api/chat/route.ts     → /api/chat
```

**Create a page**: Add `page.tsx` in a folder under `src/app/`

## Using PocketBase

### Auth (Client-Side)

```typescript
import { signInWithEmail, signUpWithEmail, signOut, getCurrentUser } from "@/lib/pocketbase";

await signUpWithEmail(email, password);
await signInWithEmail(email, password);
await signOut();
const user = getCurrentUser();
```

### Auth (Server-Side)

```typescript
import { createServerClient, getServerUser } from "@/lib/pocketbase/server";

const pb = await createServerClient();
const user = await getServerUser();
```

### Database Operations

```typescript
import { pb } from "@/lib/pocketbase";

// Create
await pb.collection("posts").create({ title: "Hello", content: "World" });

// Read
const posts = await pb.collection("posts").getList(1, 20);
const post = await pb.collection("posts").getOne("RECORD_ID");

// Update
await pb.collection("posts").update("RECORD_ID", { title: "Updated" });

// Delete
await pb.collection("posts").delete("RECORD_ID");
```

**Important**: Create collections in PocketBase Admin (`/_/`) before using them.

### Protecting Routes

Add to `src/lib/pocketbase/middleware.ts`:

```typescript
if (!pbAuth && request.nextUrl.pathname.startsWith('/dashboard')) {
  return NextResponse.redirect(new URL('/login', request.url));
}
```

## Using AI (OpenRouter)

Requires `OPENROUTER_API_KEY` in `.env.local`.

```typescript
import { chat, generate } from "@/lib/ai";

const response = await generate("Write a tagline for my app");
const response = await chat([
  { role: "user", content: "Hello!" }
]);
```

## UI Components

All shadcn/ui components are in `src/components/ui/`. Use them like:

```tsx
import { Button } from "@/components/ui/button";
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";

<Button variant="default">Click me</Button>
<Card>
  <CardHeader><CardTitle>Title</CardTitle></CardHeader>
  <CardContent>Content here</CardContent>
</Card>
```

## Commands

- `bun dev` - Start dev server
- `bun build` - Build for production

## When You Start

1. Check `documentation/` for the app spec
2. Plan the pages and data models needed
3. Create PocketBase collections if needed (via Admin UI)
4. Build pages one at a time, testing each
5. Style with Tailwind and shadcn/ui components
