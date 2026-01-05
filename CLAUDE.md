
# Project Overview

This is a Next.js starter template with:
- **Next.js 15** with App Router and TypeScript
- **Tailwind CSS v4** for styling
- **shadcn/ui** for UI components
- **Supabase** for authentication and database

## Project Structure

```
src/
├── app/                    # Next.js App Router pages
│   ├── auth/
│   │   └── callback/       # OAuth callback handler
│   └── page.tsx            # Home page
├── components/
│   └── ui/                 # shadcn/ui components
└── lib/
    ├── supabase/           # Supabase configuration
    │   ├── client.ts       # Browser client
    │   ├── server.ts       # Server client
    │   ├── middleware.ts   # Session refresh middleware
    │   └── auth.ts         # Auth helper functions
    └── utils.ts            # Utility functions (cn)
```

## Getting Started

1. Copy `.env.local.example` to `.env.local` and add your Supabase credentials
2. Run `npm install` to install dependencies
3. Run `npm run dev` to start the development server

## Supabase Setup

1. Create a project at https://supabase.com
2. Get your project URL and anon key from Settings > API
3. Add them to `.env.local`

### Auth Functions Available

- `signInWithEmail(email, password)` - Email/password sign in
- `signUpWithEmail(email, password)` - Email/password sign up
- `signOut()` - Sign out current user
- `signInWithOAuth(provider)` - OAuth sign in (google, github, discord)
- `resetPassword(email)` - Send password reset email
- `updatePassword(password)` - Update user password

## Adding shadcn Components

```bash
npx shadcn@latest add button
npx shadcn@latest add card
npx shadcn@latest add input
# etc.
```

## MCP Servers

This project includes MCP server configurations for:
- **Supabase MCP** - Database and auth operations
- **Context7** - Documentation lookup

Set `SUPABASE_ACCESS_TOKEN` in your environment to use the Supabase MCP.

## Commands

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run lint` - Run ESLint
