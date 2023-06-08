This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `pages/index.tsx`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.ts`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.

This project uses [`next/font`](https://nextjs.org/docs/basic-features/font-optimization) to automatically optimize and load Inter, a custom Google Font.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.



// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model User {
  id             String    @id @default(uuid())
  username       String    @unique
  name           String
  bio            String?
  email          String?   @unique
  email_verified DateTime?
  avatar_url     String?

  created_at DateTime @default(now())

  accounts      Account[]
  sessions      Session[]
  timeIntervals UserTimeInterval[]
  Scheduling    Scheduling[]

  @@map("users")
}

model Account {
  id                  String  @id @default(cuid())
  user_id             String
  type                String
  provider            String
  provider_account_id String
  refresh_token       String?
  access_token        String?
  expires_at          Int?
  token_type          String?
  scope               String?
  id_token            String?
  session_state       String?

  user User @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@unique([provider, provider_account_id])
  @@map("accounts")
}

model Session {
  id            String   @id @default(cuid())
  session_token String   @unique
  user_id       String
  expires       DateTime
  user          User     @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("sessions")
}

model UserTimeInterval {
  id                    String @id @default(uuid())
  week_day              Int
  time_start_in_minutes Int
  time_end_in_minutes   Int

  user    User   @relation(fields: [user_id], references: [id])
  user_id String

  @@map("user_time_intervals")
}

model Scheduling {
  id           String   @id @default(uuid())
  date         DateTime
  name         String
  email        String
  observations String?
  created_at   DateTime @default(now())

  user    User   @relation(fields: [user_id], references: [id])
  user_id String

  @@map("schedulings")
}
