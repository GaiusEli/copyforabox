# AgencyBox Project Requirements Document

This document explains in everyday language how AgencyBox works, what problems it solves, and what our initial version includes. AgencyBox is an AI-powered platform that helps users create and manage Meta (Facebook/Instagram) ad campaigns quickly and easily. By using smart automation and industry-tested ad strategies, the platform minimizes manual work and delivers consistently great results. It is built for solo entrepreneurs as well as mid-market and enterprise teams so that anyone from a small business owner to a large marketing department can use it.

The platform is being built to simplify the ad creation process by removing the complexity of Meta’s advertising ecosystem. By guiding users through an intuitive wizard and integrating advanced AI tools for generating ad copy and creative assets, AgencyBox aims to be the one-stop-shop for managing ad campaigns. Our key success criteria are ease of use, seamless integration with Meta’s API, robust security, and scalability that grows with the user’s business.

## In-Scope vs. Out-of-Scope

**In-Scope (for First Version):**\
• Full implementation of the step-by-step campaign creation wizard (connecting Meta account, defining campaign objectives/budget/schedule, creating a target audience, uploading or generating creative assets, generating ad copy, and reviewing ad variations).\
• Integration with Meta’s Marketing API for campaign creation, ad set configuration, and dynamic creative management.\
• AI-driven content generation for primary text and headlines using GPT-4 as well as creative asset generation (images or short videos) via Sora.\
• Role-based user permissions ranging from solo entrepreneurs (full control) to mid-market and enterprise teams (Admins, Editors, and Viewers) with added approval workflow for enterprise.\
• A modern, professional UI adopting a blue-purple gradient branding style and using Tailwind CSS.\
• Core backend functionalities implemented using Supabase (database, storage, serverless functions, and row-level security) with authentication via Clerk.dev and front-end deployed on Vercel.\
• Basic performance dashboard showing key metrics (impressions, clicks, CTR, CPA, ROAS) fetched from Meta’s Insights API and a preview of all 12 dynamic creative combinations.\
• Essential security and compliance features (GDPR/CCPA compliance with data export and deletion, secure communication over HTTPS, and robust authentication and audit logging).

**Out-of-Scope (for First Version):**\
• Advanced modifications or post-launch editing of campaigns (for example, adjusting campaigns after publishing will have limited functionality).\
• Full-fledged multi-language UI and ad copy generation support – initial launch may support English with plans for additional languages in future updates.\
• Comprehensive real-time alerts and email/SMS notifications – a basic performance dashboard suffices for MVP.\
• Third-party CRM integrations or automated lead management – focus remains on campaign creation and performance insights.\
• Container orchestration or on-premise deployments – the solution is built solely on cloud-native, serverless platforms.

## User Flow

A new user lands on AgencyBox and is greeted with a friendly onboarding screen explaining the AI-driven approach. The user signs up using Clerk (or via SSO for enterprise), then is prompted to connect their Facebook/Instagram Ads account through Meta’s secure OAuth process. Once connected, the user is guided step-by-step through the campaign creation wizard that starts with specifying campaign objectives, setting budget and schedules, and defining the target audience with preset options and simple input fields.

After these basic details are set, the user is moved to the creative stage where they either upload three creatives or generate them using our integrated AI service. Next, they input or generate two variations of the primary ad text and two headline variants using GPT-4, with on-screen guides and character counters to stay within Meta’s limits. At the end of the wizard, a preview screen shows all 12 ad variations (three creatives × 2 primary texts × 2 headlines) along with a campaign summary. Finally, based on their role (solo, Editor, or Admin), the user either directly launches the campaign or submits it for admin approval. Once submitted, the campaign is processed via serverless functions calling Meta’s API, and the user is taken to a performance dashboard where they can see metrics and receive AI-driven suggestions.

## Core Features

• **User Roles and Permissions:**\

*   Solo entrepreneurs with full admin rights.\
*   Mid-Market teams split into Admin, Editor, and Viewer roles.\
*   Enterprise teams with added features like approval workflows, audit logs, SSO integration, and white-labeling.

• **Campaign Creation Wizard:**\

*   Step-by-step guidance: connect Meta ads account, choose campaign objective, define budget and schedule, and set targeting rules.\
*   Built-around the “3:2:2” dynamic creative approach (3 creative assets, 2 text variations, 2 headline options).

• **Creative Asset Management:**\

*   Options to upload images and short videos.\
*   AI-powered creative generation through Sora driven by a text prompt.

• **AI-Driven Ad Copy Generation:**\

*   Use GPT-4 to generate multiple variations for ad copy and headlines based on user-provided descriptions and tone guidelines.\
*   Character count validation and editing capabilities.

• **Ad Combination Preview:**\

*   A dynamic preview page displays all 12 possible ad combinations and a campaign summary before submission.

• **Meta Marketing API Integration:**\

*   Secure OAuth connection to link Meta Ads accounts.\
*   API calls to create campaigns, ad sets, upload creative assets, and launch ads.

• **Performance Dashboard:**\

*   Visual charts and summary cards showing metrics like impressions, clicks, CTR, CPA, and ROAS.\
*   Capabilities for filtering by date range and reviewing asset-level performance.

• **AI Advisor and Recommendations:**\

*   Post-launch analysis using GPT-4 to provide actionable marketing recommendations based on performance data.

## Tech Stack & Tools

• **Frontend:**\

*   Next.js and React for a modern, interactive UI.\
*   Tailwind CSS (with a custom theme for the blue-purple gradient and modern SaaS design) to build a responsive user interface.\
*   Shadcn UI components, as part of CodeGuide Starter Pro, to expedite UI development.

• **Backend:**\

*   Supabase for our PostgreSQL database, storage for creative assets, and built-in serverless functions (Edge Functions).\
*   Clerk.dev for secure authentication and optional SSO for enterprise users.\
*   Serverless functions deployed on Vercel (Next.js API routes) for custom backend logic that ties into Meta’s API and processes AI calls.

• **AI Integrations:**\

*   OpenAI’s GPT-4 for ad copy generation, strategic recommendations, and in-app AI assistant.\
*   OpenAI Sora for generating images or short videos for creatives.

• **Third-Party APIs:**\

*   Meta Marketing API for campaign creation, management, and retrieving performance metrics.\
*   Stripe (planned for Phase 2) for subscription and billing integration.

• **Developer Tools:**\

*   Lovable.dev and V0 by Vercel for generating modern web components.\
*   Cursor as our advanced IDE for AI-assisted coding.

## Non-Functional Requirements

• **Performance:**\

*   Fast load times through Vercel’s global CDN and edge network.\
*   Serverless functions are coded to respond within typical lambda request limits (under 10 seconds).

• **Security:**\

*   End-to-end HTTPS communication across all services.\
*   Secure authentication with Clerk and robust row-level security in Supabase.\
*   Secure storage of sensitive keys and tokens in environment variables and Supabase Vault.\
*   GDPR and CCPA compliance including user consent, data export, and deletion functionalities.

• **Scalability:**\

*   Use of serverless functions and Vercel/Supabase ensures the platform can scale automatically as the user base grows.\
*   CBO (Campaign Budget Optimization) and dynamic creative configurations are built to handle increasing campaign loads.

• **Usability:**\

*   Clean, modern design following a SaaS aesthetic with intuitive navigation and minimal clutter.\
*   Guidance at every step in the wizard with contextual help, tool-tips, and character counters.

## Constraints & Assumptions

• **Constraints:**\

*   MVP development focuses on a single language (English), although the architecture will later support multi-language UI and ad copy generation.\
*   Deep integration with Meta’s Marketing API requires careful management of OAuth tokens and API rate limits.\
*   AI generation costs must be tracked, so free tiers include strict daily/monthly quotas.\
*   Serverless function execution limits (e.g., cold starts, request timeouts) must be respected in our deployment on Vercel.

• **Assumptions:**\

*   Users have basic familiarity with Meta ad campaigns, though the wizard abstracts complexity.\
*   All sensitive backend calls (Meta, OpenAI) happen server-side to ensure key security.\
*   Third-party services like Clerk, Vercel, Supabase, and Stripe (future integration) are reliable and will handle scaling.\
*   Future features like real-time alerts, multi-language support, and advanced dashboards will build on the same core platform once the MVP is stable.

## Known Issues & Potential Pitfalls

• **API Rate Limits & Token Expiry:**\

*   Meta Marketing API and OpenAI API have rate limits. Ensure retries/backoff strategies are in place especially for ad creation and insights retrieval.\
*   OAuth tokens from Meta must be refreshed securely without exposing them to the client.

• **AI Generation Latency:**\

*   GPT-4 and Sora API calls may have variable response times. Use loading spinners and consider asynchronous processing (especially for video generation) if delays become noticeable.

• **User Role Enforcement and Security:**\

*   The combination of application-level checks and Supabase RLS must be thoroughly tested to prevent any unauthorized data access.\
*   Integrate proper audit logging to troubleshoot any issues in permissions or API calls.

• **Deployment & Environment Configuration:**\

*   Keeping environment variables secure on Vercel and ensuring smooth deployment with Supabase edge functions can be challenging if misconfigured; maintain clear documentation and automated tests.\
*   Monitor for potential cold start issues on serverless functions that could affect response times.

• **Third-Party Service Dependencies:**\

*   Downtime or changes in any integrated service (Clerk, Supabase, Meta API, OpenAI) might impact functionality. Maintain error handling and fallback messaging in these scenarios.

• **Compliance Handling:**\

*   Ensuring proper compliance with GDPR/CCPA means regular review of our data handling and deletion processes. Any errors in export or deletion flows should be monitored closely.

This document acts as the main brain for AgencyBox’s development. With each section clearly defined, subsequent technical design documents (tech stack, frontend guidelines, backend structure, app flowchart, and integration plans) will leverage this PRD to maintain consistency and clarity. Every feature, integration, and design decision is laid out in a way that leaves no guesswork, ensuring a smooth transition from planning to implementation.
