# Backend Structure Document

This document explains the backend structure for AgencyBox. It covers everything from overall architecture to security and maintenance. The language used is straightforward, making it easy to understand how the backend works, without needing a deep technical background.

## 1. Backend Architecture

AgencyBox uses a modern, serverless architecture for both performance and scalability. Here’s a brief breakdown:

*   **Overall Design:**

    *   Uses a combination of serverless functions, cloud-managed databases, and third-party APIs.
    *   Built around a microservices-like approach, where individual services (e.g., ad content generation, campaign management) handle distinct tasks.
    *   Emphasizes modularity, ensuring that individual subsystems can be updated without affecting the entire system.

*   **Frameworks and Tools:**

    *   **Frontend & API Routes:** Built with Next.js and Node API routes, deployed on Vercel.
    *   **AI Services:** Integrates GPT-4 for ad copy creation and Sora for creative generation via OpenAI services.
    *   **Authentication & Security:** Uses Clerk.dev for SSO and authentication, with Supabase enforcing row-level security (RLS) in the PostgreSQL database.

*   **Benefits:**

    *   **Scalability:** Serverless functions and managed cloud services automatically scale with user demand.
    *   **Maintainability:** Modular code and clear separation of concerns make updates and maintenance easier.
    *   **Performance:** Fast, edge-based deployments and lightweight APIs ensure a smooth user experience.

## 2. Database Management

The system uses Supabase, which offers a managed PostgreSQL database along with storage and serverless functions. Key points:

*   **Database Technology:**

    *   **SQL Database**: PostgreSQL via Supabase ensures robust data handling.

*   **Data Management Practices:**

    *   **Row-Level Security (RLS):** Ensures that users only see the data they’re allowed to access, enforced by Supabase and integrated with Clerk.dev's JWT.
    *   **Data Structuring:** Data is organized in logical tables, with relationships between users, campaigns, ad creatives, and performance logs.
    *   **Regular Backups & Encryption:** Data is backed up regularly and stored with encryption standards to protect sensitive information.

## 3. Database Schema

### Human Readable Overview

*   **Users:** Contains information about the users (e.g., entrepreneurs, team members, enterprise contacts) including roles and permissions.
*   **Campaigns:** Details for each ad campaign, such as objectives, budgets, schedules, and associated metadata.
*   **Ad Creatives:** Contains data about visual creatives, generated content, and variations (primary texts, headlines).
*   **Performance Metrics:** Stores performance logs, metrics fetched from Meta Ads and AI-driven recommendations.
*   **Audit Logs:** Keeps track of activity for enterprise teams including changes, approvals, and status logs.

### Sample PostgreSQL Schema

Below is an example schema in SQL for a PostgreSQL database (as implemented via Supabase):

`-- Users table CREATE TABLE users ( id UUID PRIMARY KEY DEFAULT gen_random_uuid(), email VARCHAR(255) UNIQUE NOT NULL, role VARCHAR(50) NOT NULL, -- e.g., 'admin', 'editor', 'viewer' created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP ); -- Campaigns table CREATE TABLE campaigns ( id UUID PRIMARY KEY DEFAULT gen_random_uuid(), user_id UUID REFERENCES users(id) ON DELETE CASCADE, name VARCHAR(255) NOT NULL, objective VARCHAR(100) NOT NULL, budget DECIMAL(10, 2) NOT NULL, schedule JSONB, -- e.g., start and end dates meta_campaign_id VARCHAR(255), -- Meta Ads reference created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP ); -- Ad Creatives table CREATE TABLE ad_creatives ( id UUID PRIMARY KEY DEFAULT gen_random_uuid(), campaign_id UUID REFERENCES campaigns(id) ON DELETE CASCADE, creative_type VARCHAR(50), -- e.g., image, video creative_url TEXT, -- URL to the creative asset primary_text VARCHAR(500), headline VARCHAR(255), created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP ); -- Performance Metrics table CREATE TABLE performance_metrics ( id UUID PRIMARY KEY DEFAULT gen_random_uuid(), campaign_id UUID REFERENCES campaigns(id) ON DELETE CASCADE, metric_data JSONB, -- Contains the metrics as reported by Meta Insights API recorded_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP ); -- Audit Logs table (for enterprise teams) CREATE TABLE audit_logs ( id UUID PRIMARY KEY DEFAULT gen_random_uuid(), user_id UUID REFERENCES users(id) ON DELETE SET NULL, action TEXT NOT NULL, details JSONB, created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP );`

## 4. API Design and Endpoints

The backend uses RESTful APIs to facilitate communication between the frontend and backend services.

*   **General Design:**

    *   Endpoints are designed to be simple and intuitive, following standard REST principles.
    *   All sensitive operations (such as Meta API calls or OpenAI services) are performed on the server to maintain security.

*   **Key Endpoints Include:**

    *   **User & Authentication:**

        *   POST `/api/auth/login` – Handle user login via Clerk.dev.
        *   POST `/api/auth/logout` – Logout users safely.

    *   **Campaign Management:**

        *   POST `/api/campaigns` – Create a new advertising campaign using the campaign creation wizard.
        *   GET `/api/campaigns` – Retrieve a list of campaigns for the logged-in user.
        *   GET `/api/campaigns/:id` – Get detailed information about a specific campaign.

    *   **Ad Creative Generation:**

        *   POST `/api/generate-ad-content` – Generate ad copy (primary text and headlines) with GPT-4 and creative visuals with Sora.

    *   **Meta Ads Integration:**

        *   POST `/api/meta/connect` – OAuth connection to the user’s Meta Ads account.
        *   POST `/api/meta/submit` – Submit campaign details (campaign, ad set, creatives) to Meta.

    *   **Performance and Dashboard:**

        *   GET `/api/performance/:campaignId` – Retrieve performance metrics and insights from Meta Ads and internal tracking.

    *   **Audit and Logs:**

        *   GET `/api/audit` – For enterprise users to view audit logs.

## 5. Hosting Solutions

AgencyBox leverages cloud hosting to ensure high availability and efficiency.

*   **Primary Hosting Providers:**

    *   **Vercel:** Hosts the Next.js frontend and serverless API routes. Benefits include:

        *   Quick deployments and efficient scaling.
        *   Built-in CDN and optimization features.

    *   **Supabase:** Manages the PostgreSQL database, storage, and serverless functions. Benefits include:

        *   Seamless integration with authentication and row-level security features.
        *   Automated scaling and backups for data security.

## 6. Infrastructure Components

A number of infrastructure components work together to keep the backend running smoothly:

*   **Load Balancing:**

    *   Automatically managed by Vercel and Supabase, ensuring requests are evenly distributed.

*   **Caching Mechanisms:**

    *   Use of edge caching provided by Vercel to reduce latency and improve response times.
    *   Potential integration of server-side caching for frequently accessed data.

*   **Content Delivery Network (CDN):**

    *   Static assets and frontend resources are served through Vercel’s CDN, ensuring fast delivery worldwide.

*   **Serverless Functions and Edge Functions:**

    *   Implement lightweight functions for processing sensitive API calls and data handling.

## 7. Security Measures

The project places a strong emphasis on data and user security. Here are the main measures:

*   **Authentication & Authorization:**

    *   Uses Clerk.dev for SSO, handling authentication and user session management.
    *   Implements role-based permissions to control data access.

*   **Data Encryption & Security:**

    *   PostgreSQL via Supabase employs encryption at rest and in transit.
    *   Supabase’s Row-Level Security (RLS) adds an extra layer of data access control.

*   **Sensitive API Calls:**

    *   All calls to Meta and OpenAI services are made from secure server-side endpoints.
    *   API keys and secret tokens are stored in secure vaults and managed carefully.

*   **Compliance:**

    *   Ensures GDPR and CCPA compliance via mechanisms for consent management, data export, and deletion.

## 8. Monitoring and Maintenance

Continuous monitoring is essential for the health of the backend. The following practices and tools are used:

*   **Monitoring Tools:**

    *   Vercel’s dashboard for real-time monitoring of deployments and API performance.
    *   Supabase analytics for tracking database performance and serverless function usage.
    *   Third-party logging services (like Sentry) to capture and alert on errors.

*   **Maintenance Strategies:**

    *   Regular updates to dependencies and security patches.
    *   Automated backups and disaster recovery processes.
    *   Scheduled reviews of API rate limits and performance metrics to ensure smooth operations.

## 9. Conclusion and Overall Backend Summary

In summary, AgencyBox’s backend is designed to support an AI-driven advertising campaign platform with high scalability, security, and performance. Key highlights include:

*   A modern, serverless architecture using Vercel and Supabase.
*   Secure user management and data access controls enforced through Clerk.dev and Supabase RLS.
*   A robust PostgreSQL database schema that efficiently manages campaigns, ad creatives, performance metrics, and audit logs.
*   Intuitive RESTful APIs that connect the frontend to backend services, ensuring smooth integration with Meta Ads and AI services.
*   Cloud hosting and infrastructure components (load balancers, caching, CDNs) that provide excellent reliability and global reach.

This well-rounded backend structure is aligned with AgencyBox’s goal of reducing manual work in ad campaign management while providing a secure, scalable, and user-friendly platform.

**Tech Stack Used in the Backend:**

*   Next.js
*   React
*   Tailwind CSS
*   Shadcn UI (components)
*   Typescript
*   Supabase (PostgreSQL, Storage, Edge Functions)
*   Clerk.dev (Authentication & SSO)
*   OpenAI (GPT-4, Sora APIs)
*   Vercel (Hosting for frontend and serverless API routes)
*   Stripe (Billing integration in Phase 2)

This concludes the backend structure overview for AgencyBox.
