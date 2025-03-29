# ValerieMail

An open-source email marketing solution that seamlessly integrates with your existing Next.js applications. Powered by [valerie-email-builder](https://github.com/theogenejr/valerie-email-builder) and Resend.

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Next.js](https://img.shields.io/badge/Next.js-15+-black)](https://nextjs.org)

![ValerieMail Screenshot](https://fakeimg.pl/400x400/25277d/ffffff?text=ValerieMail)

## Features

- 🧩 **Simple Integration**: Add comprehensive email marketing to your existing apps
- ✉️ **Beautiful Emails**: Create stunning responsive emails with our drag-and-drop builder
- 📊 **Campaign Management**: Create, schedule, and analyze email campaigns
- 👥 **List Management**: Organize subscribers into targeted lists
- 📈 **Analytics**: Track opens, clicks, and other engagement metrics
- 🔄 **Webhooks**: Integrate with your existing systems via webhooks
- 🔒 **Compliance Built-in**: GDPR-friendly with automatic unsubscribe handling

## Quick Start

### Installation

```bash
npx create-valeriemail-app
# or
npm install valeriemail
```

### Basic Setup

```tsx
// app/api/valeriemail/route.ts
import { createValerieMailApi } from 'valeriemail/api';

export const { GET, POST, PUT, DELETE } = createValerieMailApi({
  apiKey: process.env.RESEND_API_KEY,
  database: {
    type: 'cloudflare-d1', // or 'postgres', 'mysql', 'sqlite'
    connectionString: process.env.DATABASE_URL,
  },
  webhook: {
    secret: process.env.WEBHOOK_SECRET,
  }
});
```

```tsx
// app/emails/page.tsx
import { ValerieMailDashboard } from 'valeriemail/components';

export default function EmailsPage() {
  return (
    <main>
      <h1>Email Marketing</h1>
      <ValerieMailDashboard />
    </main>
  );
}
```

## Core Components

ValerieMail is built as a modular system, allowing you to use only what you need:

### UI Components

- `<ValerieMailDashboard />` - Full dashboard UI
- `<EmailBuilder />` - Standalone email builder component
- `<CampaignManager />` - Campaign creation and management
- `<SubscriberList />` - Subscriber management interface
- `<AnalyticsDashboard />` - Email performance metrics

### Backend Components

- `createValerieMailApi()` - Create API endpoints for ValerieMail
- `createValerieMailClient()` - Client for programmatic access
- `setupResendWebhooks()` - Configure Resend webhook handling

## Configuration Options

```tsx
// Full configuration example
createValerieMailApi({
  // Email provider (required)
  emailProvider: {
    type: 'resend', // Currently only 'resend' is supported
    apiKey: process.env.RESEND_API_KEY,
    fromEmail: 'newsletter@yourdomain.com',
    replyToEmail: 'contact@yourdomain.com'
  },
  
  // Database configuration (required)
  database: {
    type: 'cloudflare-d1', // Primary option
    // Alternative options: 'postgres', 'mysql', 'sqlite'
    connectionString: process.env.DATABASE_URL,
    // Optional database configuration
    poolSize: 10
  },
  
  // Webhook configuration (optional)
  webhook: {
    secret: process.env.WEBHOOK_SECRET,
    eventHandlers: {
      // Custom event handlers
      'email.delivered': async (event) => {
        console.log('Email delivered:', event);
      }
    }
  },
  
  // Authentication (optional, uses your app's auth by default)
  auth: {
    // Function to verify request authentication
    verifyRequest: async (request) => {
      // Return user object or null
    }
  },
  
  // Storage (optional)
  storage: {
    type: 'cloudflare-r2', // Primary option
    // Alternative options: 's3', 'vercel-blob'
    // Configuration specific to storage provider
  }
});
```

## Programmatic Usage

Send emails programmatically from anywhere in your application:

```tsx
import { createValerieMailClient } from 'valeriemail/client';

const emailClient = createValerieMailClient();

// Send a one-off email
await emailClient.sendEmail({
  templateId: 'welcome-template',
  to: 'user@example.com',
  data: {
    userName: 'John',
    activationLink: 'https://yourdomain.com/activate/token123'
  }
});

// Create a campaign
await emailClient.createCampaign({
  name: 'Monthly Newsletter',
  templateId: 'newsletter-template',
  listIds: ['active-users'],
  scheduledFor: new Date('2025-04-15T10:00:00Z')
});
```

## Development

```bash
# Clone the repository
git clone https://github.com/yourusername/valeriemail.git

# Install dependencies
cd valeriemail
npm install

# Start the development server
npm run dev

# Build for production
npm run build
```

## Tech Stack

- **Frontend**: Next.js 14 App Router, React 18, Tailwind CSS, Shadcn UI
- **Email Editor**: valerie-email-builder (Lexical-based rich text editor)
- **Backend**: Next.js API Routes with Edge Runtime
- **Database**: Cloudflare D1 (primary), with support for PostgreSQL, MySQL, SQLite
- **Email Provider**: Resend API
- **Storage**: Cloudflare R2 (primary), with support for S3, Vercel Blob
- **State Management**: React Context, Zustand
- **Form Handling**: React Hook Form, Zod validation
- **TypeScript**: End-to-end type safety

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT © Theogene Junior
