### Static Pages:

1. Home /
2. About Us /about
3. Contact Us /contact

---

- A reusable component of SEO for all the static page is good option.

---

# Step-by-Step SEO Implementation for Static Pages in Next.js (App Router) with TypeScript

Here's a complete guide to implementing SEO for static pages using Next.js App Router and TypeScript:

## Step 1: Set Up Project Structure

```
/src
  /app
    /about
      page.tsx
    /contact
      page.tsx
  /types
    seo-types.ts
  /components
    Seo.tsx
```

## Step 2: Define TypeScript Types

Create a type definition file for your SEO metadata:

```typescript
// src/types/seo-types.ts
export type StaticPageMetadata = {
  title: string;
  description: string;
  keywords?: string[];
  openGraph?: {
    type?: "website" | "article";
    url?: string;
    title?: string;
    description?: string;
    images?: {
      url: string;
      width?: number;
      height?: number;
      alt?: string;
    }[];
    siteName?: string;
    publishedTime?: string;
    modifiedTime?: string;
  };
  twitter?: {
    card?: "summary" | "summary_large_image" | "app" | "player";
    site?: string;
    creator?: string;
    title?: string;
    description?: string;
    images?: {
      url: string;
      alt?: string;
    }[];
  };
  canonicalUrl?: string;
  robots?: {
    index?: boolean;
    follow?: boolean;
    nocache?: boolean;
    noarchive?: boolean;
    nosnippet?: boolean;
  };
};
```

## Step 3: Create Metadata Generator Utility

```typescript
// src/utils/generate-metadata.ts
import { Metadata } from "next";
import { StaticPageMetadata } from "@/types/seo-types";

export const generateStaticMetadata = (
  metadata: StaticPageMetadata
): Metadata => {
  return {
    title: `${metadata.title} | Your Site Name`,
    description: metadata.description,
    keywords: metadata.keywords?.join(", "),
    alternates: {
      canonical: metadata.canonicalUrl,
    },
    robots: metadata.robots || {
      index: true,
      follow: true,
    },
    openGraph: {
      title: metadata.openGraph?.title || metadata.title,
      description: metadata.openGraph?.description || metadata.description,
      url: metadata.openGraph?.url || metadata.canonicalUrl,
      siteName: metadata.openGraph?.siteName || "Your Site Name",
      images: metadata.openGraph?.images,
      locale: "en_US",
      type: metadata.openGraph?.type || "website",
      publishedTime: metadata.openGraph?.publishedTime,
      modifiedTime: metadata.openGraph?.modifiedTime,
    },
    twitter: {
      card: metadata.twitter?.card || "summary_large_image",
      site: metadata.twitter?.site || "@yourhandle",
      creator: metadata.twitter?.creator || "@yourhandle",
      title: metadata.twitter?.title || metadata.title,
      description: metadata.twitter?.description || metadata.description,
      images: metadata.twitter?.images,
    },
  };
};
```

## Step 4: Implement in Static Pages

### About Page Example:

```typescript
// src/app/about/page.tsx
import { generateStaticMetadata } from "@/utils/generate-metadata";
import { StaticPageMetadata } from "@/types/seo-types";

export const generateMetadata = (): Metadata => {
  const metadata: StaticPageMetadata = {
    title: "About Us",
    description: "Learn about our company history, mission, and values",
    keywords: ["about", "company", "history", "mission"],
    openGraph: {
      images: [
        {
          url: "/images/about-og.jpg",
          width: 1200,
          height: 630,
          alt: "About Our Company",
        },
      ],
    },
    canonicalUrl: "https://yourdomain.com/about",
  };

  return generateStaticMetadata(metadata);
};

export default function AboutPage() {
  return (
    <main className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">About Our Company</h1>
      <section className="prose max-w-none">{/* Your content here */}</section>
    </main>
  );
}
```

### Contact Page Example:

```typescript
// src/app/contact/page.tsx
import { generateStaticMetadata } from "@/utils/generate-metadata";
import { StaticPageMetadata } from "@/types/seo-types";

export const generateMetadata = (): Metadata => {
  const metadata: StaticPageMetadata = {
    title: "Contact Us",
    description: "Get in touch with our team for inquiries and support",
    keywords: ["contact", "support", "help", "email"],
    openGraph: {
      type: "website",
      images: [
        {
          url: "/images/contact-og.jpg",
          alt: "Contact Our Team",
        },
      ],
    },
    twitter: {
      card: "summary_large_image",
      images: [
        {
          url: "/images/contact-twitter.jpg",
          alt: "Contact Our Team",
        },
      ],
    },
    canonicalUrl: "https://yourdomain.com/contact",
  };

  return generateStaticMetadata(metadata);
};

export default function ContactPage() {
  return (
    <main className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">Contact Us</h1>
      {/* Contact form component */}
    </main>
  );
}
```
