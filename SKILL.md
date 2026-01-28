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

## When This Skill Applies

This skill activates when the user:
- Asks to create a "blog portrait" or "profile blog" about someone
- Wants to generate a personalized blog based on a person's online presence
- Requests blog posts written in someone's voice/style
- Asks to research and write about a specific person's ideas and work

## Process Overview

1. **Research** — Search for the person across social profiles (Twitter, LinkedIn, GitHub), interviews, podcasts, talks, and written content
2. **Plan** — Propose 7-10 blog post topics based on themes discovered in research
3. **Clone** — Clone rauchg's blog template and personalize it for the subject
4. **Write** — Generate essay-style blog posts drawing from multiple sources
5. **Deploy** — Build and deploy to Vercel, returning the live URL

## Phase 1: Research

Use WebSearch and WebFetch to build a comprehensive profile. **Minimum: 10+ distinct sources before proceeding.**

### Primary Sources (Required)

**Social Profiles:**
- Twitter/X — profile bio, pinned tweets, notable threads, who they follow/interact with
- LinkedIn — full career history, recommendations, featured content
- GitHub — projects, contributions, starred repos, README philosophies, comments on public repos and PRs (reveals technical opinions and collaboration style)
- Personal website/blog — all published writing, about page, project descriptions

**Long-form Content:**
- Podcast appearances — search "[name] podcast interview" (Spotify, Apple, YouTube)
- Conference talks — YouTube, Vimeo, conference archives
- Video interviews — search "[name] interview" on YouTube
- Published articles — personal blog, Medium, Substack, company blogs

### Secondary Sources (At Least 3)

**Community & Discussion:**
- HackerNews — search "site:news.ycombinator.com [name]" for discussions, AMAs, comments
- Reddit — AMAs, mentions in relevant subreddits
- Twitter threads discussing their work

**Books & Academic:**
- Books authored or contributed to
- Academic papers (Google Scholar)
- Book recommendations they've made (Goodreads, tweets, interviews)

**Historical:**
- Wayback Machine — archived versions of personal sites, old blog posts
- Newsletter archives (if applicable)
- Company blogs from previous roles

### Research Synthesis

**Build a profile with:**
- Career timeline with key transitions and decisions
- Areas of expertise (primary and adjacent)
- Recurring themes across different mediums
- Evolution of thinking over time (how have their views changed?)
- Notable opinions or contrarian takes
- Influences — who shaped their thinking? (books, people, experiences)
- Impact — who have they influenced? What's their legacy so far?
- Communication style — formal/casual, technical/accessible, long-form/pithy

**Cross-reference:**
- Verify facts across multiple sources
- Note contradictions or evolution in their positions
- Distinguish public persona from deeper beliefs (interviews reveal more than tweets)

Save structured findings to scratchpad with source links for attribution.

## Phase 2: Plan Posts

Propose **7-10 blog posts** focusing on distinct themes. Example angles:
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
   - `app/posts.tsx` — Simplify the post list by replacing the `List` function with:
     ```tsx
     function List({ posts }) {
       return (
         <ul>
           {posts.map((post) => {
             return (
               <li key={post.id} className="group">
                 <Link href={`/${new Date(post.date).getFullYear()}/${post.id}`}>
                   <span className="flex py-2 items-center">
                     <span className="grow dark:text-gray-100">
                       <span className="group-hover:bg-neutral-200 dark:group-hover:bg-neutral-700 transition-all rounded-xl py-0.5 px-1.5">
                         {post.title}
                       </span>
                     </span>
                   </span>
                 </Link>
               </li>
             );
           })}
         </ul>
       );
     }
     ```
     Also remove the unused `getYear` function at the bottom of the file.

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

### Voice & Tone

**Match the subject's communication style:**
- Study how they write — formal or conversational? Technical or accessible?
- Mirror their sentence structure — short and punchy, or long and flowing?
- Use vocabulary consistent with their domain
- If they're known for humor, let that come through; if serious, stay substantive

**Write as if you are them** — these posts should feel like something they would write, not something written *about* them.

### Structure

**Opening (100-150 words):**
- Hook with a specific anecdote, quote, or surprising insight
- Establish stakes — why does this topic matter?
- No generic intros ("In today's world...") — start in the middle of the action

**Body (600-1000 words):**
- 3-5 sections with clear subheadings
- Each section makes one distinct point
- Concrete examples > abstract principles
- Use direct quotes with attribution: `"Quote here" — [Source Name](link)`
- Include specific numbers, dates, project names
- Show evolution: "In 2018, they believed X. By 2023, this shifted to Y because..."

**Closing (100-200 words):**
- Synthesize, don't summarize
- End with forward-looking insight or call to reflection
- Strong final sentence that resonates

### Quality Standards

**Word count:** 800-1500 words per post (aim for 1000-1200 sweet spot)

**Attribution:**
- Every significant claim should trace to a source
- Prefer direct quotes over paraphrasing for opinions
- Link to original content when available

**Editing passes:**
1. **Accuracy** — Verify all facts, dates, quotes against sources
2. **Voice** — Does this sound like them? Read aloud to check
3. **Flow** — Does each paragraph lead naturally to the next?
4. **Cut** — Remove anything that doesn't earn its place

### What to Avoid

- Generic tech-blog phrasing ("game-changer", "leveraging", "at the end of the day")
- Hagiography — show complexity, not just praise
- Unsourced claims about their beliefs or motivations
- Filler content to hit word count

## Output

Return:
1. Research summary
2. List of posts created
3. **Live Vercel URL**
