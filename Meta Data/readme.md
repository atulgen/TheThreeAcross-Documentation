# Meta Data in Next.js with Strapi Backend

## What is Meta Data?

Meta data is information about your web pages that isn't directly visible to users but is crucial for search engines and social media platforms. It includes:

- **Title tags**: The title of your page that appears in search results and browser tabs
- **Meta descriptions**: Brief summaries of your page's content
- **Open Graph tags**: For controlling how your content appears when shared on social media
- **Twitter cards**: Similar to Open Graph but specific to Twitter
- **Canonical URLs**: To prevent duplicate content issues
- **Viewport settings**: For responsive design
- **Charset declarations**: For character encoding

## Pages

1. Home /
2. About Us /about
3. Services /services
4. Careers /careers
5. Blogs /blog
6. Contact Us /contact

---

Now the development can be divided into 2 parallels:

1. Static Page Metadata
2. Dynamic Page Metadata

### Static Page Metadata

- Needs to modify the code base
- Creation of SEO Component for reducing repetition of code.
- Modify existing static pages.

- [Read more](./Static%20Page%20Metadata/readme.md)

### Dynamic Page Metadata

- Start with the source
  - CMS is our source
- Meta data will be fetched from CMS with existing data.
- Get Meta data
- Append it to the Dynamic page

- [Read More](./Dynamic%20Page%20Metadata/readme.md)

---

## Implementing Meta Data in Next.js with Strapi

### 1. Dynamic Pages (Blog, News, Events)

For dynamic content coming from Strapi, you'll want to:

#### Strapi Setup:

1. Create or modify your content types to include SEO fields:

   - title
   - metaDescription
   - openGraphImage (media field)
   - canonicalURL
   - etc.

2. Set up proper permissions in Strapi to allow your marketing team to edit these fields.

#### Next.js Implementation:

Use Next.js's built-in `next/head` component or the newer `metadata` API in App Router:

```jsx
// For Pages Router (pages/[slug].js)
import Head from "next/head";

export default function BlogPost({ post }) {
  return (
    <>
      <Head>
        <title>{post.seo.title || post.title}</title>
        <meta name="description" content={post.seo.metaDescription} />
        <meta property="og:title" content={post.seo.title || post.title} />
        <meta property="og:description" content={post.seo.metaDescription} />
        <meta property="og:image" content={post.seo.openGraphImage.url} />
        <link rel="canonical" href={post.seo.canonicalURL || currentURL} />
      </Head>
      {/* Your page content */}
    </>
  );
}

// For App Router (app/[slug]/page.js)
export async function generateMetadata({ params }) {
  const post = await getPost(params.slug);

  return {
    title: post.seo.title || post.title,
    description: post.seo.metaDescription,
    openGraph: {
      title: post.seo.title || post.title,
      description: post.seo.metaDescription,
      images: [post.seo.openGraphImage.url],
    },
    alternates: {
      canonical: post.seo.canonicalURL || currentURL,
    },
  };
}
```

### 2. Static Pages (About, Contact)

For static pages, you have two approaches:

#### Option 1: Manage through Strapi

1. Create a "Pages" content type in Strapi with SEO fields
2. Fetch this content in your Next.js static pages

#### Option 2: Hardcode in Next.js

Define metadata directly in your page components:

```jsx
// For Pages Router (pages/about.js)
import Head from "next/head";

export default function About() {
  return (
    <>
      <Head>
        <title>About Us | Company Name</title>
        <meta
          name="description"
          content="Learn about our company's mission and team"
        />
        {/* Other meta tags */}
      </Head>
      {/* Page content */}
    </>
  );
}

// For App Router (app/about/page.js)
export const metadata = {
  title: "About Us | Company Name",
  description: "Learn about our company's mission and team",
  openGraph: {
    title: "About Us | Company Name",
    description: "Learn about our company's mission and team",
    images: ["/images/about-og.jpg"],
  },
};
```

## Implementation Recommendations for Your Project

1. **Create an SEO component** to reuse across pages:

```jsx
const SEO = ({ title, description, image, canonical }) => {
  return (
    <Head>
      <title>{title}</title>
      <meta name="description" content={description} />
      <meta property="og:title" content={title} />
      <meta property="og:description" content={description} />
      {image && <meta property="og:image" content={image} />}
      {canonical && <link rel="canonical" href={canonical} />}
    </Head>
  );
};
```

2. **Set up default metadata** in `_app.js` or `layout.js` for fallback values.

3. **Consider using a library** like `next-seo` if you need more advanced SEO features.

4. **For Strapi integration**:

   - Ensure your API endpoints return the SEO fields
   - Consider creating a dedicated SEO service in your Next.js app to handle the data transformation

5. **Testing**:
   - Use tools like Google's Rich Results Test
   - Check social media sharing previews
   - Validate with SEO auditing tools

## Workflow for Marketing Team

1. **For dynamic content**:

   - Edit SEO fields directly in Strapi when creating/editing blog posts, news articles, etc.

2. **For static pages**:
   - Either give them access to edit the "Pages" content type in Strapi
   - Or provide a way for them to submit changes to developers (less ideal)
