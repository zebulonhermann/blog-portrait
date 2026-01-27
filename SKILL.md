---
name: blog-portrait
description: Research a person online and generate a personalized blog about them.
  Use this skill when the user requests creating a blog portrait, profile blog,
  or personal blog about someone. The skill researches the person's online presence,
  writes thoughtful blog posts based on their work and ideas, and deploys to Vercel.
license: MIT
metadata:
  author: vercel-labs
  version: "1.0.0"
---

# Blog Portrait

Generate a complete, personalized blog about any person by researching their online presence and synthesizing their ideas into thoughtful blog posts.

## How It Works

1. **Research** — Search for the person across social profiles (Twitter, LinkedIn, GitHub), interviews, podcasts, talks, and written content
2. **Plan** — Propose 3-5 blog post topics based on themes discovered in research
3. **Clone** — Clone rauchg's blog template and personalize it for the subject
4. **Write** — Generate essay-style blog posts drawing from multiple sources
5. **Deploy** — Build and deploy to Vercel, returning the live URL

## Usage

```
/blog-portrait [person-name]
```

**Examples:**
- `/blog-portrait Guillermo Rauch`
- `/blog-portrait Patrick Collison`
- `/blog-portrait Sarah Drasner`

## Phase 1: Research

Use WebSearch and WebFetch to gather:

**Social Profiles:**
- Twitter/X profile and notable tweets
- LinkedIn career history
- GitHub projects
- Personal website/blog
- Substack, Medium, or other writing

**Content:**
- Podcast appearances
- Conference talks
- Video interviews
- Published articles

**Identify:**
- Areas of expertise
- Recurring themes
- Notable opinions
- Career highlights
- Current role, location, education

Save findings to scratchpad for reference.

## Phase 2: Plan Posts

Propose **3-5 blog posts** focusing on distinct themes. Example angles:
- Career journey and lessons learned
- Technical philosophy or approach
- Domain expertise deep-dive
- Evolution of thinking on a topic

**Present plan to user for approval before writing.**

## Phase 3: Clone and Personalize

1. Clone the template:
```bash
git clone https://github.com/rauchg/blog.git blog-portrait-[name-slug]
cd blog-portrait-[name-slug] && pnpm install
```

2. **Personalize these files** (replace "Guillermo Rauch" with subject's name):
   - `app/logo.tsx` — Header name
   - `app/layout.tsx` — Metadata, description, Twitter handle, URL
   - `app/footer.tsx` — Name and social link
   - `app/header.tsx` — Twitter link
   - `app/opengraph-image/route.tsx` — Name and URL
   - `app/about/opengraph-image/route.tsx` — Name and bio

3. **Delete old posts:**
```bash
rm -rf app/(post)/2014 app/(post)/2015 app/(post)/2016 app/(post)/2017 app/(post)/2020 app/(post)/2021 app/(post)/2025 app/(post)/ja
```

4. **Rewrite `app/about/page.mdx`** with:
   - Bio from research
   - Current role
   - Career history
   - Social links

## Phase 4: Write Posts

Create posts in `app/(post)/[year]/[slug]/page.mdx`:

```mdx
export const metadata = {
  title: 'Post Title',
  description: 'Description',
  openGraph: {
    title: 'Post Title',
    description: 'Description',
    images: [{ url: '/og/post-slug' }]
  }
}

Post content in markdown...
```

**Replace** `app/posts.json` with only new posts:
```json
{
  "posts": [
    { "id": "post-slug", "date": "Jan 27, 2026", "title": "Post Title" }
  ]
}
```

## Phase 5: Deploy

```bash
pnpm build
git add -A && git commit -m "Add blog portrait of [name]"
npx vercel --prod --yes
```

## Writing Guidelines

- Essay-style tone, 800-1500 words per post
- Use specific quotes and examples from research
- Include source links where appropriate
- Subheadings to break up content
- Thoughtful conclusions

## Output

Return:
1. Research summary
2. List of posts created
3. **Live Vercel URL**
