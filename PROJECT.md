# Tool Use Podcast — Website Project Spec

> **Deadline: March 31, 2026**
> **Repo: ToolUse/website**
> **Stack: Astro + Vercel**
> **Domain: toolusepodcast.com**

## Overview

Build a content hub for the Tool Use Podcast. This is NOT just a blog — it's an SEO-optimized content library designed to be discovered by both search engines and AI agents (ChatGPT, Perplexity, Google AI Overviews). The site serves as a funnel: people discover episodes via search → land on the site → subscribe to the podcast.

Two primary content types:
1. **Episode Library** — All 84+ episodes with searchable descriptions, guest info, and full transcripts
2. **Blog** — Long-form articles (repurposed from video content, a.k.a. Twitter articles)

## Brand Context

- **Host:** Mike Bird (@MikeBirdTech) — AI & Engineering Lead at BoxOne Ventures
- **Podcast:** "AI tools and strategies to empower forward-thinking minds, featuring top experts."
- **Schedule:** New episodes every Tuesday
- **Audience:** Builders and operators, not AI influencers or spectators
- **Personality:** Competent friend who tests and knows what works. Practical over aspirational. Direct over diplomatic. Playful over precious. Present-focused, not futuristic.
- **Three Pillars:** Execution (real deployment), Concentration (serious builder community), Sovereignty (open-source AI vs big tech)

### Colors (current palette — may update with new logo)
- `#112935` (dark navy)
- `#33a4b2` (teal)
- `#f5732d` (orange)
- `#fecc2d` (yellow)
- `#e5ddd0` (cream)

### Logo
A new logo is being designed and will be provided before launch. Use a placeholder during development. The logo should be easy to swap via a single asset replacement.

## Tech Stack

- **Framework:** [Astro](https://astro.build/) (static site generator)
- **Hosting:** [Vercel](https://vercel.com/) (free tier)
- **Styling:** Tailwind CSS
- **Content:** Markdown files with YAML frontmatter (Astro Content Collections)
- **No JavaScript frameworks required.** Use Astro components. Only add client-side JS for search/filter functionality (use Astro islands if needed).

## Content Collections

### Episodes (`src/content/episodes/`)

Each episode is a markdown file named by number: `001.md`, `002.md`, etc.

**Frontmatter schema:**
```yaml
---
number: 52
title: "Advanced Claude Code"
slug: "advanced-claude-code"
description: "Ray Fernando and Eric Buess join Mike to deep-dive into Advanced Claude Code workflows, custom commands, and production-grade agent strategies."
date: 2025-09-16  # publish date
duration: "PT52M30S"  # ISO 8601 duration
guests:
  - name: "Ray Fernando"
    url: "https://twitter.com/user"  # optional
    title: "AI Developer"  # optional
  - name: "Eric Buess"
    url: ""
    title: ""
tags: ["claude-code", "coding-agents", "ai-tools"]
youtube_url: "https://www.youtube.com/watch?v=XXXXX"
youtube_id: "XXXXX"
apple_url: ""  # optional
spotify_url: ""  # optional
featured: false  # for homepage featured episodes
draft: false
---

## Summary

A 2-3 paragraph editorial summary of the episode, written for humans and SEO.
Include key topics discussed and main takeaways.

## Key Takeaways

- Takeaway one with specific actionable insight
- Takeaway two
- Takeaway three
- Takeaway four
- Takeaway five

## Timestamps

- 00:00 - Introduction
- 03:15 - Topic one
- 12:40 - Topic two
- 25:00 - Topic three

## Transcript

Mike: Welcome to Tool Use...

Guest: Thanks for having me...
```

### Blog Posts (`src/content/blog/`)

Each blog post is a markdown file named by slug: `grep-is-dead.md`, etc.

**Frontmatter schema:**
```yaml
---
title: "Why AI Agents Keep Failing at Simple Tasks"
slug: "why-ai-agents-keep-failing"
description: "Most AI agents fail not because the models are bad, but because the orchestration is wrong. Here's what actually works in production."
date: 2026-03-15
author: "Mike Bird"
tags: ["ai-agents", "production", "debugging"]
cover_image: "/images/blog/ai-agents-failing.jpg"  # optional
source_episode: 34  # optional — links back to the episode this was derived from
featured: false
draft: false
---

Article content in markdown...
```

## Pages & Routes

### Homepage (`/`)
- Hero section with podcast name, tagline, and subscribe links
- Featured/latest episode with embedded YouTube player
- Recent blog posts (3-4 cards)
- "Browse Episodes" CTA
- Brief "About" section
- Subscribe links (YouTube, Apple, Spotify)

### Episode Library (`/episodes/`)
- Grid/list of all episodes, newest first
- Client-side search/filter by:
  - Text search (title, description, guest names)
  - Tags/topics
  - Guest name
- Each card shows: episode number, title, guest(s), date, duration, tags
- Pagination or infinite scroll (paginate at ~20 per page for SEO — each page gets its own URL)

### Individual Episode (`/episodes/[slug]/`)
- H1: Episode title with number
- Episode metadata bar (date, duration, guest names, tags)
- YouTube embed player
- Listen on: Apple | Spotify | YouTube Music links
- Editorial summary (the "Summary" section from markdown)
- Key takeaways (bullet list)
- Timestamps (with links/anchors if possible)
- Full transcript (collapsible or always visible — test both)
  - Format as speaker-labeled dialogue
  - Use topic headings (H2/H3) to break up the transcript
- Related episodes (3-5 episodes with matching tags)
- Guest bio(s) with links
- CTA: Subscribe to the podcast

### Blog Listing (`/blog/`)
- Grid of blog post cards, newest first
- Each card: title, date, description, cover image, tags
- Pagination at ~10 per page

### Individual Blog Post (`/blog/[slug]/`)
- H1: Article title
- Author, date, reading time
- Cover image (if provided)
- Article content (full markdown rendering with code blocks)
- "Based on Episode X" link (if source_episode is set)
- Related posts
- Share buttons (Twitter/X, LinkedIn, copy link)
- CTA: Subscribe to the podcast

### About (`/about/`)
- About the podcast and host
- Mike Bird bio
- Mission statement (the three pillars)
- Link to BoxOne Ventures
- Social links

### Topic Hubs (`/topics/[tag]/`)
- Auto-generated pages for each tag that aggregate:
  - All episodes with that tag
  - All blog posts with that tag
- Brief description of the topic (can be added later, optional)
- These are critical for SEO "topical cluster" strategy

## SEO Requirements

### Critical — This Is The Whole Point

The site exists to be found. Every decision should optimize for discoverability.

### Meta Tags (per page)
- Unique `<title>` tag (format: "Page Title | Tool Use Podcast")
- Unique `<meta name="description">` with compelling copy
- Canonical URL
- Open Graph tags (og:title, og:description, og:image, og:type, og:url)
- Twitter Card tags (twitter:card=summary_large_image, twitter:site, twitter:title, twitter:description, twitter:image)

### Schema.org Structured Data (JSON-LD)

**Homepage — PodcastSeries:**
```json
{
  "@context": "https://schema.org",
  "@type": "PodcastSeries",
  "name": "Tool Use - AI Conversations",
  "description": "The latest AI tools and strategies, featuring top builders, entrepreneurs, and researchers. Every Tuesday, get actionable knowledge you can use right away.",
  "url": "https://toolusepodcast.com",
  "image": "https://toolusepodcast.com/podcast-artwork.jpg",
  "webFeed": "https://toolusepodcast.com/feed.xml",
  "author": {
    "@type": "Person",
    "name": "Mike Bird",
    "url": "https://mikebird.tech",
    "sameAs": [
      "https://twitter.com/MikeBirdTech",
      "https://github.com/MikeBirdTech",
      "https://youtube.com/@MikeBirdTech"
    ]
  },
  "genre": ["Technology", "Education", "Artificial Intelligence"],
  "inLanguage": "en",
  "isFamilyFriendly": true
}
```

**Episode Pages — PodcastEpisode:**
```json
{
  "@context": "https://schema.org",
  "@type": "PodcastEpisode",
  "url": "https://toolusepodcast.com/episodes/advanced-claude-code",
  "name": "Episode 52: Advanced Claude Code",
  "description": "...",
  "datePublished": "2025-09-16",
  "episodeNumber": 52,
  "duration": "PT52M30S",
  "transcript": "https://toolusepodcast.com/episodes/advanced-claude-code#transcript",
  "associatedMedia": {
    "@type": "VideoObject",
    "name": "Episode 52: Advanced Claude Code",
    "embedUrl": "https://www.youtube.com/embed/XXXXX",
    "thumbnailUrl": "https://img.youtube.com/vi/XXXXX/maxresdefault.jpg",
    "uploadDate": "2025-09-16",
    "duration": "PT52M30S"
  },
  "partOfSeries": {
    "@type": "PodcastSeries",
    "name": "Tool Use - AI Conversations",
    "url": "https://toolusepodcast.com"
  },
  "actor": [
    {
      "@type": "Person",
      "name": "Mike Bird"
    },
    {
      "@type": "Person",
      "name": "Ray Fernando"
    }
  ]
}
```

**Blog Posts — Article:**
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Why AI Agents Keep Failing at Simple Tasks",
  "author": {
    "@type": "Person",
    "name": "Mike Bird"
  },
  "datePublished": "2026-03-15",
  "publisher": {
    "@type": "Organization",
    "name": "Tool Use Podcast",
    "url": "https://toolusepodcast.com"
  }
}
```

**Add BreadcrumbList schema to every page.**

### Sitemap & Robots

**`/sitemap.xml`** — Auto-generated, include all episode pages, blog posts, topic hubs.

**`/robots.txt`:**
```
User-agent: *
Allow: /
Sitemap: https://toolusepodcast.com/sitemap.xml

# Explicitly allow AI crawlers
User-agent: GPTBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Google-Extended
Allow: /
```

### RSS Feed (`/feed.xml`)
- Auto-generated from episode content collection using `@astrojs/rss`
- Include all episode metadata
- Reference in HTML head: `<link rel="alternate" type="application/rss+xml" title="Tool Use Podcast" href="/feed.xml" />`

### Performance
- Astro outputs static HTML — leverage this for perfect Core Web Vitals
- Lazy load images and YouTube embeds
- Use `loading="lazy"` on all images below the fold
- YouTube embeds: use lite-youtube-embed or similar (loads a thumbnail, only loads the iframe on click)
- Target: 90+ on all Lighthouse categories

## Design Direction

### General
- Clean, modern, minimal. Content-forward, not design-heavy.
- Dark mode default (matches the current brand colors — dark navy base)
- Light mode toggle if straightforward to implement
- Mobile-first responsive design
- The vibe: developer-friendly, fast, no-nonsense. Think Astro's own docs site or Ghost's default themes — clean and readable.

### Typography
- System font stack or a clean sans-serif (Inter, Geist, or similar)
- Large, readable body text (18px base)
- Clear heading hierarchy

### Episode Cards
- Episode number prominently displayed
- Title, guest name(s), date
- Tags as small pills/badges
- Duration indicator

### Code Blocks (for blog posts)
- Syntax highlighting (Astro has built-in Shiki support)
- Copy button on code blocks
- Support for common languages: python, javascript, bash, json, yaml

## Distribution Links

These should be configurable in a single config file (`src/config.ts` or similar):

```typescript
export const SITE_CONFIG = {
  title: "Tool Use Podcast",
  description: "AI tools and strategies to empower forward-thinking minds, featuring top experts.",
  url: "https://toolusepodcast.com",
  author: "Mike Bird",
  social: {
    twitter: "https://twitter.com/MikeBirdTech",
    youtube: "https://youtube.com/@ToolUsePodcast",
    github: "https://github.com/ToolUse",
    discord: "https://discord.gg/PnEGyXpjaX",
  },
  podcast: {
    apple: "https://podcasts.apple.com/us/podcast/tool-use-ai-conversations/id1773693853",
    spotify: "https://open.spotify.com/show/4mYqyL7gGglZV7ByW8TVe9",
    youtube: "https://youtube.com/@ToolUsePodcast",
    rss: "https://toolusepodcast.com/feed.xml",
  },
};
```

## Content Publishing Workflow

Content is published by creating/editing markdown files and pushing to the `main` branch. Vercel auto-deploys on push.

**Adding a new episode:**
1. Create `src/content/episodes/NNN.md` with frontmatter + transcript
2. Git commit and push
3. Vercel builds and deploys (~30 seconds)

**Adding a blog post:**
1. Create `src/content/blog/slug-name.md` with frontmatter + content
2. Git commit and push
3. Vercel builds and deploys

**Draft content:** Set `draft: true` in frontmatter. Filter drafts from production builds but include in dev.

## Astro Packages to Use

- `@astrojs/sitemap` — auto sitemap generation
- `@astrojs/rss` — RSS feed generation
- `@astrojs/tailwind` — Tailwind CSS integration
- `astro-seo` or manual `<head>` — SEO meta tags
- Shiki (built-in) — code syntax highlighting

## Vercel Configuration

- Framework preset: Astro
- Build command: `npm run build`
- Output directory: `dist`
- Custom domain: `toolusepodcast.com` (DNS pointed from Cloudflare)
- No server-side rendering needed — fully static output (`output: 'static'` in astro.config)

## File Structure

```
ToolUse/website/
├── public/
│   ├── images/
│   │   ├── logo.svg          # Podcast logo (placeholder until new logo)
│   │   ├── podcast-artwork.jpg
│   │   └── blog/             # Blog post images
│   ├── robots.txt
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Header.astro
│   │   ├── Footer.astro
│   │   ├── EpisodeCard.astro
│   │   ├── BlogCard.astro
│   │   ├── YouTubeEmbed.astro  # Lite YouTube embed
│   │   ├── SEOHead.astro       # Meta tags + JSON-LD
│   │   ├── PodcastLinks.astro  # Subscribe buttons
│   │   ├── SearchFilter.astro  # Client-side search for episodes
│   │   ├── TagList.astro
│   │   └── RelatedContent.astro
│   ├── content/
│   │   ├── config.ts          # Content collection schemas (Zod)
│   │   ├── episodes/
│   │   │   ├── 001.md
│   │   │   ├── 002.md
│   │   │   └── ...
│   │   └── blog/
│   │       ├── first-article.md
│   │       └── ...
│   ├── layouts/
│   │   ├── BaseLayout.astro    # HTML shell, head, header, footer
│   │   ├── EpisodeLayout.astro
│   │   └── BlogLayout.astro
│   ├── pages/
│   │   ├── index.astro         # Homepage
│   │   ├── about.astro
│   │   ├── episodes/
│   │   │   ├── index.astro     # Episode library
│   │   │   └── [...slug].astro # Individual episode pages
│   │   ├── blog/
│   │   │   ├── index.astro     # Blog listing
│   │   │   └── [...slug].astro # Individual blog posts
│   │   ├── topics/
│   │   │   └── [tag].astro     # Topic hub pages
│   │   └── feed.xml.ts         # RSS feed
│   ├── styles/
│   │   └── global.css
│   └── config.ts               # Site-wide configuration
├── astro.config.mjs
├── tailwind.config.mjs
├── tsconfig.json
├── package.json
├── PROJECT.md                   # This file
└── README.md
```

## Episode Data Reference

The podcast has 84 episodes (plus 2 pre-series). Here's the full catalog for populating the content collection. Episodes marked with `*` had the highest view counts and should be prioritized for detailed summaries.

```
Pre-Series:
  -1: Auto Podcast and AI Commit
   0: Open Interpreter Obsidian & Convert Anything

Main Series:
   1: Synthetic Data Generation and Automated Research Assistant
   2: Video to Content Pipeline and YouTube Playlist Summarization
   3: Obsidian Plugin Generate and Scripts that make Scripts
   4: Activity Tracker and Calendar Automator
   5: The Future of Voice Agents (ft Killian Lucas)
   6: What Are Techfren's Favourite AI Tools?
   7: How do AI Engineers Use AI Tools?
   8: Use AI to Make Beautiful User Interfaces (ft Dillion Verma)
   9: Become a 10x Engineer With These AI Tools (ft Jake Koenig)
  10: Automate Your Desktop to Save Time With These FREE Tools (ft Richard Abrich) *
  11: Save 10+ Hours Each Week With These FREE AI Tools
  12: AI Tools and Workflows To Be More Efficient (ft Jason McGhee)
  13: Build Your AI Marketing Machine (ft Murat Koylan)
  14: 10 AI Tools That Actually Deliver Results (ft Ray Fernando)
  15: When AI Benchmarks Lie: A Better Way to Evaluate (ft Chris Hay)
  16: This is How You Are Supposed To Prompt LLMs (ft Brian Fioca) *
  17: Get Incredible Results From Local Models (ft Nisten)
  18: $100M CTO to AI Solo Founder (ft Neil Chudleigh)
  19: Modernize Your Business With LLMs (ft Francisco Ingham)
  20: What's Next For Tool Use?
  21: How to get #1 on SWE Bench (ft Graham Neubig)
  22: Single AI Agents Were Just The Start (ft Aaron Wong-Ellis)
  23: Learn AI Engineering 10x Faster (ft Hai Nghiem)
  24: We Built an AI Tool That Saves Us $360/yr
  25: The Hard Truth About RAG in Production (ft Kirk Marple)
  26: Building AI's Future: From Open Data to Robots (ft Victor Miller)
  27: Will AI Agents Be Your Automation Breakthrough? (ft NLW)
  28: Never Open Your Inbox Again (ft Sarah Allali)
  29: Why the Best AI Builders Ship Before They're Ready (ft Adam Cohen Hillel)
  30: How to Build AI Apps That Just Work (ft Viabhav Gupta)
  31: The AI Landscape Has Changed Again
  32: Control Your Computer with Your Voice (& Feet??) Using AI (ft CJ Pais)
  33: Build Your Next AI App in Minutes with Mozilla.AI Blueprints (ft Alex Meckes)
  34: Why AI Agents Keep Failing at Simple Tasks (ft Dexter Horthy) *
  35: The Henry Ford Approach To Building Modern AI (ft Minki Jung)
  36: Google Scientist's Insights on AI Education & Tools (ft Dr Stefania Druga)
  37: How The Best Product Managers Use AI (ft Elina Lesyk)
  38: How To Turn Text Into Amazing Music (ft Phlo)
  39: Why AI Agents Fail In Production And How To Fix It (ft Josh Purtell)
  40: Use AI To Build Your Own Tools (ft Manuel Odendahl)
  41: The Best Developer Experience For Building A.I. Agents (ft Ahmad Awais)
  42: Your Everyday Data Holds Life Changing Secrets (ft Michael Tiffany)
  43: Use AI to Catch Your Bad Leadership Habits (ft Ian Pilon)
  44: The Right Way to Do AI Evals (ft Freddie Vargus)
  45: The Blueprint For AI Agents That Work (ft Diamond Bishop)
  48: The AI Playbook Used By Global Companies (ft Olga Beregovaya)
  49: He's Building a Startup With AI (ft Ryan Carson)
  50: The Best AI Advice for 2025
  51: How to Build Secure MCP Servers (ft Craig McLuckie)
  52: Advanced Claude Code (ft Ray Fernando and Eric Buess) *
  53: Why Open Source AI Will Win (ft Alignment Lab)
  54: Fundamentals of AI Tooling (ft Hai Nghiem)
  55: How I Use AI for REAL Productivity Gains *
  57: Advanced AI Prompting (ft Michael Tiffany & Ty Fiero)
  58: Are AI Agents Ready for the Internet? (ft Bobbie Chen)
  59: A.I. For Non-Technical Founders (ft Marnie Wills)
  60: AI Coding Agents Can Do So Much More (ft Kiran)
  61: How to Become an AI-first Company (ft Csongor Barabasi)
  62: "AI allows you to be high tech and high touch" (ft Lee Russel)
  63: Practical RAG Advice for Businesses (ft Apurva Misra)
  64: "n8n is powerful enough to replace an entire department" (ft Richardson Dackam)
  65: Practical AI Safety (ft Kyle Clark)
  66: How To Build A Successful AI Product (ft Swyx)
  67: Coding Agents in Obsidian (ft Artem Zhutov) *
  68: Principles of Building AI Agents (ft Sam Bhagwat)
  69: How To Build an AI Business With a Moat (ft Mike Leslie)
  70: A.I. Builder Tips: Voice Control, Fast Orchestration, MCP (ft Jason Kneen)
  71: AI for Executive Function (ft Sam Julien)
  72: The State of A.I. 2025 (ft Richardson Dackam)
  73: How To Get a Job in AI in 2026
  74: How to Master Distribution For Your AI Apps (ft Sukh Saini)
  75: Advanced Claude Code Part 2 (ft Eric Buess)
  76: Ryan Carson Explains The Ralph Wiggum Loop (ft Ryan Carson)
  77: From Marketer to Growth Engineer Using AI (ft Justin Borge)
  78: Fine-Tune Your Own A.I. Video Model (ft Greg Schoeninger)
  79: Do You Need A Vector Database in 2026? (ft Arjun Patel)
  80: AI Sovereignty - Control Your Entire AI Architecture (ft Max McCrea)
  81: Exploring Any-LLM from Mozilla.AI (ft Nathan Brake)
```

**Note:** Episodes 46, 47, 56, and 82-84 titles were not confirmed during research. Mike will fill these in.

### Recurring Guests
- Ryan Carson: Ep 49, 76
- Eric Buess: Ep 52, 75
- Ray Fernando: Ep 14, 52
- Richardson Dackam: Ep 64, 72
- Hai Nghiem: Ep 23, 54
- Michael Tiffany: Ep 42, 57
- Ty Fiero: Co-host in many early episodes, featured Ep 57

### Top Tags (suggested taxonomy)
- ai-agents
- claude-code
- coding-agents
- open-source
- rag
- mcp
- productivity
- ai-tools
- local-models
- ai-business
- evals
- voice-ai
- ai-safety
- fine-tuning
- distribution

## What This Spec Does NOT Cover

- **Content writing:** Episode summaries, key takeaways, and blog articles will be written separately. The site should work with minimal content (just title + transcript) and look good with full content.
- **Logo design:** New logo coming from designer. Will be dropped in as an asset.
- **Domain transfer:** Being handled separately (Wix → Cloudflare).
- **Transcript formatting:** Raw transcripts will be provided. May need cleanup script.

## Success Criteria

1. All 82+ episodes accessible at unique, SEO-friendly URLs
2. Full transcripts indexable by search engines and AI crawlers
3. Schema.org structured data on every page (validate with Google Rich Results Test)
4. Lighthouse score 90+ across all categories
5. Blog functional for publishing articles
6. Search/filter works on episode library
7. Mobile responsive
8. Deploys automatically on git push via Vercel
9. Clean enough that Mike is proud to share the URL
