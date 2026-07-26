# AI SaaS Project Overview

This document outlines the end-to-end process used to set up the AI SaaS application, including the NestJS backend, Prisma database connection, and Next.js frontend.

## 1. Backend Setup (NestJS)
- **Initialization**: The backend was initialized using the NestJS CLI (`npx @nestjs/cli new backend`).
- **Dependencies**: Installed necessary packages including `@nestjs/common`, `dotenv`, and others.
- **CORS**: Enabled Cross-Origin Resource Sharing (CORS) in `main.ts` to allow the frontend to communicate with the backend API.
- **Environment Variables**: Integrated `dotenv/config` at the very top of `main.ts` to ensure environment variables (like `DATABASE_URL`) are loaded before the application or ORM initializes.
- **Modules & Services**: 
  - Created a `GenerationModule`, `GenerationController`, and `GenerationService`.
  - Configured a `GET /generation` endpoint to return mock AI generation data with status 200.
  - Added a `GET /generation/:id` endpoint designed to throw a `404 Not Found` exception for testing error handling on the frontend.

## 2. Database Connection (Prisma)
- **Installation**: Installed Prisma ORM. Encountered compatibility issues with Prisma v7 and NestJS dependency injection, so we successfully downgraded to **Prisma v5** (`prisma@5` and `@prisma/client@5`), which is stable and works seamlessly.
- **Configuration**: 
  - Defined a SQLite database in `prisma/schema.prisma` using the URL from `.env`.
  - Created a `PrismaService` to handle the database lifecycle (connecting on module initialization).
- **Environment Security**: Verified that `.env` files (containing the database URL and JWT secrets) are properly excluded from version control via both the backend and frontend `.gitignore` files.

## 3. Frontend Setup (Next.js)
- **Redesign**: Completely revamped the UI in `src/app/page.tsx` to remove irrelevant text inputs and focus on data visualization.
- **API Integration**: 
  - Implemented a `handleFetch` function that makes an actual HTTP GET request to the NestJS backend (`http://localhost:3000/generation`).
  - Added loading states (spinners) and error handling.
- **UI Components**:
  - Displays a clean status banner indicating the HTTP response (e.g., `200 OK`).
  - Renders the fetched data as styled "Data Cards" showing prompt details and status badges.
  - Includes a "Raw JSON" collapsible section for developers to inspect the exact payload returned by the API.
  - Added a prominent red "Test 404 Error" button to explicitly trigger the 404 route on the backend and verify the frontend's error rendering.

## Running the Application
1. **Backend**: Navigate to `backend/` and run `npm run start` (typically runs on port 3000).
2. **Frontend**: Navigate to `frontend/` and start the dev server (typically runs on port 3001).
3. Open the frontend in your browser and click "Fetch Generations" to see the end-to-end flow in action.
