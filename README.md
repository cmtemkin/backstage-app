# Charlie's Developer Portal

A Backstage.io developer portal for managing and discovering all of Charlie Temkin's projects and shared services.

## Features

- **Software Catalog** — Browse all 14 projects organized into 4 systems (ai-applications, content-generation, shared-services, food-apps)
- **Dependency Mapping** — See which projects use which shared libraries
- **GitHub Integration** — Auto-discover repos from the cmtemkin GitHub organization
- **CI/CD Visibility** — View GitHub Actions status for each project
- **TechDocs** — Browse README documentation from all repos
- **Search** — Full-text search across all catalog entities

## Quick Start (Local Development)

```bash
# Install dependencies
yarn install

# Set up environment variables
export GITHUB_TOKEN="your_github_token_here"

# Start the development server
yarn dev
```

Visit http://localhost:3000 to see your Backstage app.

## Project Structure

```
backstage-app/
├── packages/
│   ├── app/          # Frontend React application
│   └── backend/      # Backend Node.js server
├── app-config.yaml   # Development configuration
├── app-config.production.yaml  # Production configuration
└── DEPLOYMENT.md     # Deployment guide
```

## Catalog Sources

The software catalog is populated from:
- [backstage-catalog](https://github.com/cmtemkin/backstage-catalog) — Central catalog definitions
- Auto-discovery of all repos in the cmtemkin GitHub organization (via catalog-info.yaml files)

## Configured Plugins

- `@backstage/plugin-catalog` — Software catalog
- `@backstage/plugin-catalog-backend-module-github` — Auto-discover GitHub repos
- `@backstage/plugin-techdocs` — Documentation
- `@backstage/plugin-search` — Full-text search
- `@backstage/plugin-github-actions` — CI/CD status

## Deployment

See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed deployment instructions to:
- Railway (backend + PostgreSQL)
- Vercel (frontend)
- Or Render + Netlify as alternatives

### Quick Deploy to Railway

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/backstage)

1. Click the button above
2. Add a PostgreSQL database
3. Set environment variables (see DEPLOYMENT.md)
4. Deploy!

## Environment Variables

### Required for Development

```bash
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"  # GitHub PAT with read:org and repo scopes
```

### Required for Production

See [DEPLOYMENT.md](./DEPLOYMENT.md#environment-variables-reference) for the complete list.

## What's in the Catalog?

### Systems (4)
- **ai-applications** — AI-powered apps (Needham Navigator, BriefMe, DocExplore)
- **content-generation** — Content creation tools (Scenestitch, Vid-Gen-2, Video Generation)
- **shared-services** — Reusable libraries (rag-core, scraper, supabase-kit, ai-client, ui-kit, video-pipeline)
- **food-apps** — Food-related applications (Forkful, Forkful Site)

### Applications (8)
- needham-navigator — AI-powered Q&A for Needham Middle School
- briefme — AI content briefing system
- scenestitch — Video scene analysis and stitching
- forkful — Recipe and meal planning backend
- forkful-site — Recipe and meal planning frontend
- docexplore — Document Q&A with RAG
- vid-gen-2 — Short-form video generation
- video-generation — Python video pipeline service

### Shared Libraries (6)
- @cmtemkin/rag-core — RAG pipeline (chunking, embedding, hybrid search)
- @cmtemkin/scraper — Web scraping utilities
- @cmtemkin/supabase-kit — Supabase helpers and patterns
- @cmtemkin/ai-client — Unified AI provider interface
- @cmtemkin/ui-kit — Shared UI components
- @cmtemkin/video-pipeline — Python video generation tools

## Contributing

To add a new project to the catalog:

1. Add a `catalog-info.yaml` file to your repo
2. Or add a YAML file to [backstage-catalog](https://github.com/cmtemkin/backstage-catalog)
3. The catalog will refresh every 30 minutes

## Tech Stack

- **Framework:** Backstage.io
- **Frontend:** React + TypeScript + Material-UI
- **Backend:** Node.js + Express
- **Database:** PostgreSQL
- **Hosting:** Railway (backend) + Vercel (frontend)

## Learn More

- [Backstage Documentation](https://backstage.io/docs)
- [Backstage Software Catalog](https://backstage.io/docs/features/software-catalog/)
- [TechDocs](https://backstage.io/docs/features/techdocs/)

## Support

For issues or questions about this Backstage instance, check the logs or refer to the [Backstage Community Discord](https://discord.gg/backstage-687207715902193673).
