# Implementation plan

## Phase 1: Environment Setup

1.  **Prevalidation:** Check if the current directory is already a project by verifying the existence of a `package.json` or similar project files. (Project Summary)
2.  **Check Node.js:** Run `node -v` to ensure Node.js v20.2.1 is installed; if not, install Node.js v20.2.1. (Tech Stack: Core Tools)
3.  **Cursor Prevalidation:** Verify if the `.cursor` directory exists; if it does, skip creation to avoid redundancy. (Selected Tools: Cursor)
4.  **Create Cursor Directory:** If not present, create a `.cursor` directory in the project root. (Selected Tools: Cursor)
5.  **Create MCP Config File:** Inside `.cursor`, create a file named `mcp.json` if it doesn’t exist. (Selected Tools: Cursor)
6.  **Add MCP Configuration (macOS):** Insert the following JSON in `.cursor/mcp.json`:

`{ "mcpServers": { "supabase": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-postgres", "<connection-string>"] } } } `(Tech Stack: Supabase, Project Summary: Data Security & Compliance)

1.  **Add MCP Configuration (Windows):** Alternatively, for Windows users, use:

`{ "mcpServers": { "supabase": { "command": "cmd", "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-postgres", "<connection-string>"] } } }`

1.  **Display Connection String Link:** Inform the user to retrieve the connection string at: <https://supabase.com/docs/guides/getting-started/mcp#connect-to-supabase-using-mcp> (Do not omit this link.)
2.  **Update Config:** Once the user supplies the connection string, replace `<connection-string>` in the configuration accordingly. (Tech Stack: Supabase)
3.  **Validate MCP Connection:** In Cursor, navigate to Settings → MCP and confirm that a green active status is displayed.

## Phase 2: Frontend Development

1.  **Initialize Next.js Project:** Use `npx create-next-app@14` to create a new project ensuring Next.js version 14 is used (Next.js 14 is preferred due to compatibility with AI coding tools). (Tech Stack: Next.js, React)
2.  **Verify Next.js Version:** Update `package.json` if needed to explicitly use Next.js 14.
3.  **Install Dependencies:** Set up required packages: React, TypeScript, Tailwind CSS, and Shadcn UI Components. (Tech Stack)
4.  **Setup Folder Structure:** Create folders as per the provided structure (e.g., `/app`, `/components`, `/hooks`, `/lib`, `/types`, `/utils`). (Folder Structure)
5.  **Create Landing Page:** In `/app/page.tsx`, build a landing/welcome page to guide new users (App Flow: Onboarding).
6.  **Implement Clerk Provider:** Create `/components/providers/clerk-client-provider.tsx` to integrate Clerk authentication. (Project Summary: User Roles & Permissions; Data Security)
7.  **Design Campaign Wizard Component:** In `/components/ui`, create UI components for the step-by-step campaign creation wizard (Project Summary: Campaign Creation Wizard).
8.  **Wizard Details:** Build the wizard to guide users through Meta OAuth connection, campaign objective selection, budget & schedule configuration, audience targeting, creative asset upload/generation, and ad copy generation (App Flow: Configuring a Campaign).
9.  **Style with Tailwind CSS:** Apply Tailwind CSS classes across components; reference `/tailwind.config.ts` as necessary. (Tech Stack: Tailwind CSS)
10. **Integrate AI Help Assistant:** Add an in-app GPT-4 chat component for user assistance. (Project Summary: AI-Driven Assistance)

## Phase 3: Backend Development

1.  **Configure Supabase Connection:** Set up your connection in `/utils/supabase/server.ts` and `/utils/supabase/context.tsx` to connect with Supabase. (Tech Stack: Supabase)

2.  **Define Database Schema:** Plan the following tables with row-level security (RLS):

    *   `users` (fields: id, clerk_id, role, etc.)
    *   `campaigns` (fields: id, user_id, campaign_objective, budget, schedule, target_audience, creative_assets, ad_copy, created_at) (Project Summary: Core Features: User Roles, Campaign Creation Wizard)

3.  **Deploy Schema via MCP:** Use the Supabase MCP server by running: `npx -y @modelcontextprotocol/server-postgres <connection-string>` (Tech Stack: Supabase)

4.  **Create Campaign API Endpoint:** In `/app/api/campaigns/route.ts`, build a POST endpoint for campaign submissions. (App Flow: Campaign Submission)

5.  **Build Analytics and Reports Endpoints:** In `/app/api/reports/route.ts` (if needed), create endpoints for performance analytics and report exports (PDF/CSV). (Project Summary: Performance Dashboards, Exportable Reports)

6.  **Integrate Meta Marketing API:** In `/app/api/webhooks/route.ts`, create a serverless function to handle Meta API callbacks and campaign processing. (Project Summary: Integrations: Meta Ads)

7.  **Implement Clerk Middleware:** Update `/middleware.ts` to add Clerk authentication middleware for route protection. (Project Summary: Data Security & Compliance)

8.  **Set RLS Policies:** Configure row-level security (RLS) rules on Supabase tables to enforce user permissions. (Project Summary: Data Security & Compliance)

9.  **Backend Validation:** Test all API endpoints via Postman or curl to ensure a 200 OK response and correct functionality. (Validation)

## Phase 4: Integration

1.  **Connect Frontend and API:** In your campaign wizard UI, implement fetch/axios calls to the backend API endpoints for campaign creation and management. (App Flow: Campaign Submission)
2.  **Link Clerk Authentication:** Ensure the frontend integrates with Clerk to manage user sessions by calling the Clerk provider functions. (Project Summary: User Roles & Permissions)
3.  **Integrate Meta OAuth:** Incorporate Meta’s OAuth flow in the frontend to connect users’ Facebook/Instagram Ads accounts. (App Flow: Onboarding and Account Setup)
4.  **Secure API Keys:** Store API keys (for Meta, OpenAI, etc.) securely in Supabase Vault and update references in `/utils/supabase/server.ts`. (Project Summary: Data Security)
5.  **Integrate AI Content Generation:** In `/utils/ai/openai.ts`, implement functions to call GPT-4 for ad copy, headline, and strategic recommendation generation. (Project Summary: AI-Driven Content Generation)
6.  **Integrate Sora for Visual Assets:** Similarly, update `/utils/ai/openai.ts` (or a dedicated file) to interface with the Sora API for generating images and short videos. (Project Summary: AI-Driven Content Generation)
7.  **Integration Validation:** Submit test campaign data via the frontend and verify that the backend correctly processes the request and interacts with the Meta Marketing API.

## Phase 5: Deployment

1.  **Set Up Vercel Deployment:** Connect your GitHub repository to Vercel for continuous deployment of both the frontend and serverless functions. (Hosting and Deployment: Vercel)
2.  **Configure Project Settings in Vercel:** Specify environment variables for Clerk, Supabase connection string, Meta API keys, and OpenAI credentials in the Vercel dashboard. (Project Summary: Data Security & Compliance)
3.  **Ensure Next.js Settings:** Verify that the Next.js project is deployed using version 14. (Tech Stack: Next.js 14)
4.  **Configure Continuous Deployment:** Set up automatic deployments on GitHub pushes to ensure CI/CD. (Infrastructure and Deployment)
5.  **Deploy Supabase Services:** Verify that Supabase (database, storage, edge functions) is correctly connected and operational. (Tech Stack: Supabase)
6.  **Deployment Validation:** Run end-to-end tests (using Cypress or equivalent) to verify that the complete campaign creation flow, authentication, and integrations function in production.

## Additional Steps for Future Enhancements

1.  **Stripe Integration Preparation:** Stub out endpoints for Stripe billing in `/utils/stripe` to support future subscription billing features. (Project Summary: Monetization Model)
2.  **Build Performance Dashboard:** Create a dashboard page in `/app/dashboard/page.tsx` to display campaign metrics and analytics. (Project Summary: Performance Dashboards)
3.  **Set Up Real-Time Alerts:** Configure Supabase edge functions to send real-time alerts based on campaign performance changes. (Project Summary: Real-Time Alerts)
4.  **Enable Multi-Language Support:** Integrate an internationalization library (e.g., i18next) and set up language resource files. (Project Summary: Multi-language Support)
5.  **Implement Report Exporting:** Build functionality in `/app/api/reports/route.ts` to generate and export campaign reports in PDF and CSV formats. (Project Summary: Exportable Reports)
6.  **Component Testing:** Run unit tests for UI components using your preferred testing framework (e.g., `npm run test`) and verify at least 100% coverage for critical components. (Validation)

## Final Prevalidation and Cleanup

1.  **Full Project Prevalidation:** Re-scan the project directory to ensure all necessary files, configurations, and integrations are completed and avoid redundant steps in future updates. (Prevalidation)
2.  **Final End-to-End Testing:** Conduct comprehensive tests (using Cypress or equivalent) across the entire user flow—from onboarding and campaign setup to submission and analytics—to verify system integrity.
