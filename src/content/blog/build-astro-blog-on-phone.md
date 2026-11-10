---
title: "How to Build an Astro Blog Using Only a Phone and the GitHub Web Editor"
date: 2026-11-10
description: "No laptop? Build and publish a fast Astro blog from your phone using github.dev and Vercel. A step-by-step guide from someone who ships this way daily."
cover: "/assets/posts/astro-blog-phone-github-web-editor.jpg"
alt: "Smartphone showing the GitHub web editor with an Astro blog project open"
tags:
  - astro
  - github
  - vercel
---

## Overview 

You don't need a laptop to build a real website. I don't have one. Everything I've published, from the iSamuelDev theme to a live blog with 120+ posts, was built in a mobile browser using GitHub's web editor and Vercel. No terminal, no local dev server, no installed code editor.

This guide is the step-by-step version: how to go from nothing to a live Astro blog using only your phone. If you want the honest story of what's hard about this setup, read [how I build and maintain an Astro site using only GitHub's mobile web editor](/blog/github-mobile-web-editor-workflow/). This post is the how-to.

## What you need

- A phone with a modern browser (Chrome works well)
- A free GitHub account
- A free Vercel account
- An Astro theme or starter template

## Step 1: Turn on desktop site in your browser

GitHub's mobile layout hides some buttons you'll need. In Chrome, tap the three dots and enable **Desktop site**. Everything below is easier with it on.

## Step 2: Start from a template, not from scratch

Creating an Astro project from nothing normally needs a terminal and `npm`. On a phone, skip that by starting from a theme that's already built.

1. Open the theme's GitHub repository. You can browse Astro's official theme directory, or use the iSamuelDev theme. My walkthrough is here: [how to use the iSamuelDev Astro theme](/blog/how-to-use-isamueldev-astro-theme/).
2. Tap **Use this template** and then **Create a new repository**. If the repo has no template button, tap **Fork** instead.
3. Name your repo and create it.

You now own a complete Astro site, and you haven't typed a single command.

New to Astro itself? [Getting Started with Astro](/blog/getting-started-with-astro/) explains why it's such a good fit for blogs.

## Step 3: Learn the project structure

Open your new repo and find these:

- `src/pages/` holds your pages and routes
- Your blog folder (often `src/content/blog` or similar) holds your posts as Markdown files
- `public/` holds static files like images, favicon and `robots.txt`
- `astro.config.mjs` holds site-wide Astro settings, including your site URL
- `package.json` lists your dependencies

Folder names vary by theme, so check your theme's README.

## Step 4: Edit files in the GitHub web editor

There are two ways to edit on mobile:

- **Simple editor:** open any file, tap the pencil icon, edit, then tap **Commit changes**. Good for quick fixes.
- **github.dev:** open your repo and press `.` (on a phone browser, change `github.com` to `github.dev` in the address bar). You get a VS Code-style editor in your browser with a file tree, syntax highlighting and search across every file.

Use the simple editor for one-line fixes and github.dev for anything bigger.

## Step 5: Personalize your site

Open your theme's config file and change:

- Site title and description
- Author name
- Social links
- Your site URL

Then set your real domain in the `site` field of `astro.config.mjs`. This matters for your sitemap and canonical URLs, and getting it wrong causes SEO problems later.

## Step 6: Write your first post

1. Go to your blog folder.
2. Tap **Add file** and then **Create new file**.
3. Name it something like `my-first-post.md`.
4. Add frontmatter at the top, then your content below it.

```md
---
title: "My First Post"
pubDatetime: 2026-11-09T09:00:00Z
description: "A short summary for search results."
tags:
  - hello
---

Your content starts here.
```

Frontmatter must match what your theme expects. A missing required field or a badly formatted date will fail the build.

## Step 7: Deploy to Vercel from your phone

1. Log in to Vercel and tap **Add New** and then **Project**.
2. Import your GitHub repository.
3. Vercel detects Astro automatically. Keep the default settings.
4. Tap **Deploy**.

Within a minute or two your blog is live on a `.vercel.app` URL. From now on, every commit you make on GitHub triggers a new deploy automatically.

## Step 8: Use preview deployments as your dev server

You can't run `npm run dev` on a phone, so Vercel previews take its place:

1. When you commit, choose **Create a new branch** instead of committing straight to `main`.
2. Vercel builds a preview URL for that branch.
3. Check the preview on your phone. If it looks right, merge the branch.

This is the closest thing to a safety net you'll have, so use it for any risky change.

## Step 9: Add images

Tap **Add file** and then **Upload files**, and put images in the folder your theme uses (usually `public/` or `src/assets/`). Compress images before uploading. Large files slow your site and hurt your Core Web Vitals. For the full list of image and speed fixes, see [Astro performance tips that actually move the needle](/blog/astro-performance-tips/).

## Step 10: Fix build errors

Without a terminal, your Vercel build log is your error checker. If a deploy fails:

1. Open the failed deployment in Vercel.
2. Read the build log and find the **first** error.
3. Fix the file it names, then commit again.

The most common causes are missing frontmatter fields, bad date formats and typos in imports.

## Step 11: Get your blog indexed by Google

1. Make sure your theme generates a sitemap.
2. Add your site to Google Search Console.
3. Submit your sitemap.

New sites take time to earn Google's trust, so keep publishing consistently.

## Limits to know about

- There's no local dev server, so you test through preview deployments.
- Typing code on a phone is slow, so plan edits in advance and work in short focused sessions.
- You can't install and test a package before committing. New dependencies mean editing `package.json` by hand, so be careful with versions.

If you'll be doing heavy refactors or installing packages often, this isn't the best workflow. For building and maintaining a blog, it works well.

## FAQ

**Can you build an Astro website from a phone?**

Yes. Use a ready-made theme, edit with github.dev, and deploy with Vercel. No local setup is needed.

**Can you use github.dev on mobile?**

Yes. It's a browser-based VS Code editor that runs in mobile Chrome. It gives you a file tree and search across all files, which the basic GitHub editor doesn't.

**Do I need a terminal to use Astro?**

Not if you start from a template. Vercel runs the install and build steps for you in the cloud.

**Is Vercel free for a small Astro blog?**

Vercel has a free tier that suits small personal blogs. Check Vercel's current pricing page for limits.

**What's the hardest part of building on a phone?**

Typing and debugging without a local build. I cover the pain points and workarounds in [my full mobile workflow post](/blog/github-mobile-web-editor-workflow/).

## Final thoughts

A phone and a browser are enough to ship a real, fast, SEO-ready Astro blog. The hardest part is starting, and a template removes most of the setup. Pick a theme, make your first commit and watch it deploy.

*See also: [How to Use the iSamuelDev Astro Theme](/blog/how-to-use-isamueldev-astro-theme/) · [Astro Performance Tips](/blog/astro-performance-tips/) · [My Mobile Web Editor Workflow](/blog/github-mobile-web-editor-workflow/)*
