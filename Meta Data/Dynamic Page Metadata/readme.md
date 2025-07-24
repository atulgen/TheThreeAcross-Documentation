### Dynamic Pages

3. Services /services
4. Careers /careers
5. Blogs /blog



- Here the data is coming from CMS. 
    - In 3 sections
        - Services
        - Careers
        - [Blogs](./Blogs/readme.md)

---

# Dynamic Metadata Implementation for Your Blog Pages

Based on your code, here's how to implement dynamic metadata for all your blog-related pages:

## 1. Blog Listing Page (`/blogs/`)

```typescript
// src/app/blogs/page.tsx
import { Metadata } from "next";

export const metadata: Metadata = {
  title: "Blog",
  description: "Explore our latest articles and insights",
  alternates: {
    canonical: "/blogs",
  },
};

// ... rest of your existing code
```

## 2. Category Listing Page (`/blogs/[category]/`)

```typescript
// src/app/blogs/[category]/page.tsx
import { Metadata } from "next";
import { fetchAPI } from "@/lib/fetch-api";

interface CategoryMeta {
  name: string;
  description: string;
  seo?: {
    metaTitle?: string;
    metaDescription?: string;
  };
}

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  try {
    const token = process.env.NEXT_PUBLIC_STRAPI_API_TOKEN;
    const path = `/categories`;
    const urlParamsObject = {
      filters: { slug: params.category },
      populate: "*",
    };
    const options = { headers: { Authorization: `Bearer ${token}` } };
    const response = await fetchAPI(path, urlParamsObject, options);

    const categoryData: CategoryMeta = response.data[0]?.attributes;

    return {
      title: categoryData?.seo?.metaTitle || `Blog - ${categoryData.name}`,
      description: categoryData?.seo?.metaDescription || categoryData.description,
      alternates: {
        canonical: `/blogs/${params.category}`,
      },
    };
  } catch (error) {
    console.error("Failed to fetch category metadata:", error);
    return {
      title: `Blog - ${params.category}`,
      description: `Articles in ${params.category} category`,
    };
  }
}

// ... rest of your existing code
```

## 3. Individual Blog Post Page (`/blogs/[category]/[slug]/`)

```typescript
// src/app/blogs/[category]/[slug]/page.tsx
import { Metadata } from "next";
import { fetchAPI } from "@/lib/fetch-api";

interface SeoData {
  metaTitle: string;
  metaDescription: string;
  keywords?: string;
  metaImage?: {
    data?: {
      attributes?: {
        url?: string;
      };
    };
  };
}

export async function generateMetadata({
  params,
}: {
  params: { slug: string; category: string };
}): Promise<Metadata> {
  try {
    const token = process.env.NEXT_PUBLIC_STRAPI_API_TOKEN;
    const path = `/articles`;
    const urlParamsObject = {
      filters: { slug: params.slug },
      populate: {
        seo: { populate: "*" },
        cover: { fields: ["url"] },
      },
    };
    const options = { headers: { Authorization: `Bearer ${token}` } };
    const response = await fetchAPI(path, urlParamsObject, options);

    const postData = response.data[0]?.attributes;
    const seoData: SeoData = postData?.seo;
    const coverImage = postData?.cover?.data?.attributes?.url;

    return {
      title: seoData?.metaTitle || postData.title,
      description: seoData?.metaDescription || postData.description,
      keywords: seoData?.keywords,
      alternates: {
        canonical: `/blogs/${params.category}/${params.slug}`,
      },
      openGraph: {
        title: seoData?.metaTitle || postData.title,
        description: seoData?.metaDescription || postData.description,
        url: `/blogs/${params.category}/${params.slug}`,
        images: [
          {
            url: seoData?.metaImage?.data?.attributes?.url || coverImage || "/default-og.jpg",
            width: 1200,
            height: 630,
            alt: seoData?.metaTitle || postData.title,
          },
        ],
      },
      twitter: {
        card: "summary_large_image",
        title: seoData?.metaTitle || postData.title,
        description: seoData?.metaDescription || postData.description,
        images: [
          {
            url: seoData?.metaImage?.data?.attributes?.url || coverImage || "/default-og.jpg",
            width: 1200,
            height: 630,
            alt: seoData?.metaTitle || postData.title,
          },
        ],
      },
    };
  } catch (error) {
    console.error("Failed to fetch post metadata:", error);
    return {
      title: "Blog Post",
      description: "Read our latest blog post",
    };
  }
}

// ... rest of your existing code
```

## Key Implementation Notes:

1. **For Blog Listing Page**:
   - Static metadata since it's a general listing page
   - Basic title, description, and canonical URL

2. **For Category Pages**:
   - Fetches category-specific metadata from Strapi
   - Falls back to basic info if SEO fields aren't set
   - Includes canonical URL with category slug

3. **For Individual Posts**:
   - Fetches comprehensive SEO data from Strapi
   - Includes Open Graph and Twitter Card metadata
   - Uses post cover image as fallback for social sharing
   - Proper canonical URL with full path

4. **Error Handling**:
   - Each `generateMetadata` has try-catch blocks
   - Falls back to basic metadata if API calls fail

5. **Performance Considerations**:
   - Metadata is generated at build time for static pages
   - For dynamic pages, it's fetched on each request (with Next.js caching)

## Strapi Content Type Recommendations

Ensure your Strapi content types have these fields:

1. **Article Content Type**:
   - SEO component with:
     - metaTitle
     - metaDescription
     - keywords
     - metaImage (media)
   - cover (media)
   - description (text)

2. **Category Content Type**:
   - SEO component (same as above)
   - description (text)
