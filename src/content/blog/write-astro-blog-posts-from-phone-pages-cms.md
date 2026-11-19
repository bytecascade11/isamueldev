---
title: How to Write Astro Blog Posts From Your Phone With Pages CMS (Step by Step)
description: Set up Pages CMS on an Astro blog to write Markdown posts from your
  phone. Includes a working .pages.yml, schema matching, drafts and fixes for
  common errors.
date: 2026-11-19
updated: 2026-11-19
tags:
  - astro
  - cms
  - mobile
cover: /assets/posts/write-astro-blog-posts-pages-cms-cover.jpg
author: iSamuel
featured: true
draft: false
---
Editing Markdown files through GitHub's web editor works, but it gets tiring fast. You scroll through frontmatter, fight the keyboard, and one missing quote breaks the build. A small CMS on top of the same files fixes that, and you keep everything as plain Markdown in your repo.

This guide shows how I set up Pages CMS on an Astro blog, using only a phone. It covers the config file, how to match it to your Astro schema, how drafts really work, and what to do when something breaks.

<!-- Screenshot: Pages CMS dashboard open on your phone showing the Posts collection -->

## Before you start

You need four things:

1. An Astro site that uses content collections, with posts as Markdown files.
2. A GitHub repository for that site.
3. A host that rebuilds on every commit, such as Vercel. If you haven't connected one yet, see [how to connect a custom domain to Vercel](/blog/connect-custom-domain-to-vercel/).
4. A GitHub account you can sign in with from your phone browser.

> **What Pages CMS does and doesn't do.** Pages CMS edits files in your GitHub repo and commits them. It does not build or host your site. Your host sees the new commit and deploys it, and how long that takes depends on your host and your build.

If you're starting from nothing, [build the Astro blog from your phone first](/blog/build-astro-blog-on-phone/), then come back here.

## Step 1: Check your Astro schema

This step prevents most failed builds. Astro validates every post's frontmatter against the schema in your content config, so the CMS form must produce fields that schema accepts.

Open `src/content/config.ts` (or `src/content.config.ts` in newer projects) and find your blog collection. Mine looks like this:

```ts
const blog = defineCollection({
  type: "content",
  schema: z.object({
    title: z.string(),
    description: z.string(),
    date: z.date(),
    updated: z.date().optional(),
    tags: z.array(z.string()).default([]),
    cover: z.string().optional(),
    draft: z.boolean().default(false),
    author: z.string().default("Admin"),
    featured: z.boolean().default(false),
  }),
});
```

Read it with these rules:

| Schema says | What it means for the CMS form |
| --- | --- |
| No `.optional()` or `.default()` | Required. The form must include it and mark it required. |
| `.optional()` | Can be left blank. |
| `.default(...)` | Astro fills the value when the field is missing. |
| `z.date()` | Use a date field, and the saved value must be a real date like `2026-11-19`. |
| `z.array(z.string())` | Use a string field with `list: true`. |

Field names must match exactly. If your schema says `date`, the form field must be named `date`, not `pubDate` or `pubDatetime`.

## Step 2: Add the .pages.yml config

In GitHub, tap **Add file**, then **Create new file**. Name it `.pages.yml` in the repo root (not inside `src`) and paste this:

```yaml
media:
  input: public/assets/posts
  output: /assets/posts
content:
  - name: posts
    label: Posts
    type: collection
    path: src/content/blog
    filename: "{primary}.md"
    format: yaml-frontmatter
    view:
      fields: [title, date, draft]
      primary: title
      sort: [date]
      default:
        sort: date
        order: desc
    fields:
      - { name: title, label: Title, type: string, required: true }
      - { name: description, label: Description, type: text, required: true }
      - { name: date, label: Date, type: date, required: true }
      - { name: updated, label: Updated, type: date }
      - { name: tags, label: Tags, type: string, list: true }
      - { name: cover, label: Cover image, type: image }
      - { name: author, label: Author, type: string }
      - { name: featured, label: Featured, type: boolean }
      - { name: draft, label: Draft, type: boolean }
      - name: body
        label: Content (Markdown)
        type: code
        options:
          format: markdown
```

This is a starting point, not a drop-in file for every project. Change these to match your own site:

- `path` is the folder where your posts live.
- `media.input` is where uploaded images are saved in the repo, and `media.output` is the public path written into your posts.
- `fields` must follow your schema from Step 1.

The `body` field is special. It holds everything below the frontmatter. I used `type: code` with `format: markdown` so the writing area is a raw Markdown editor. If you prefer a visual editor, change it to `type: rich-text`, but you get less control over the Markdown that ends up in the file.

## Step 3: Connect Pages CMS

1. Open **app.pagescms.org** in your phone browser.
2. Sign in with GitHub.
3. Install Pages CMS on your repository when it asks.
4. Open the repo. You should see **Posts** in the menu, with your existing posts listed.

<!-- Screenshot: the .pages.yml file open in GitHub -->

Before writing anything new, open one old post and check that the date shows correctly and the body appears in the Markdown box. Don't save it: saving can reformat the frontmatter and create a commit you didn't plan.

## Step 4: Add it to your home screen

In your browser menu, choose **Add to home screen**. It now opens like an app, so writing a post is two taps from your home screen.

## Step 5: Write a post

Tap **Posts**, add a new entry, fill in the title, description and date, then write in the Markdown box. Pages CMS names the file from the title using `filename: "{primary}.md"`, so check the file name in GitHub afterwards and make sure it makes a clean URL.

<!-- Screenshot: the post editor with the frontmatter form and the Markdown box -->

When you save, Pages CMS commits the file to your repo. Your host then builds the site. Open the new post's URL after the deploy finishes to confirm it's live.

## How drafts work

The `draft` toggle only saves `draft: true` in the file. It doesn't hide anything by itself. Your Astro site has to filter drafts out, usually where you fetch posts:

```ts
const posts = await getCollection("blog", ({ data }) => !data.draft);
```

Check every place that lists posts: the blog index, tag pages, RSS, and the sitemap. If one of them forgets the filter, your draft can appear publicly. Test with a draft post and visit its URL directly before you trust the toggle.

## Test before you rely on it

Create one test post with `draft` turned on and run through this checklist:

1. The file appears in `src/content/blog` on GitHub.
2. The frontmatter looks right: real date, tags as a list, no empty required fields.
3. Your host's build passes.
4. The draft does not appear on the blog index, tag pages or RSS.
5. Turn `draft` off, save, and confirm the post appears after the next deploy.

## Common errors and fixes

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Posts don't appear in Pages CMS | `.pages.yml` is in the wrong place or `path` is wrong | Keep the file in the repo root and check the folder name |
| New file saves in the wrong folder | Wrong `path` | Match it to your content folder exactly |
| Astro build fails after saving | Form fields don't match the schema | Compare `.pages.yml` with your content config, especially required fields and date types |
| Date error in the build | Date saved as text, not a date | Use `type: date` and avoid quotes around the date in the file |
| Images upload to the wrong place | `media.input` and `media.output` don't match your folder | Set `input` to the repo folder and `output` to the public URL path |
| Saved post isn't on the live site | Host hasn't deployed, or the build failed | Check your host's deployment log |
| A draft shows up publicly | Pages are not filtering `draft` | Add the filter wherever you fetch posts |

## Limitations

Be honest with yourself about what this setup needs:

- You still depend on GitHub, so you need access to your repo.
- A broken schema match breaks the build, and the CMS can't catch that for you.
- Drafts only work if your site code filters them.
- Deploy time depends on your host, not on Pages CMS.

For me, the trade is worth it. The content stays as plain Markdown files in my own repo, with no database and no lock-in. If you use [the iSamuelDev Astro theme](/blog/how-to-use-isamueldev-astro-theme/), the schema above is the one it uses.

## FAQ

### Does Pages CMS host or build my site?

No. It edits files in GitHub and commits them. Your hosting provider builds and deploys the site.

### Can I use it from a phone only?

Yes. I set it up and use it from a mobile browser, with no laptop or local development environment.

### Do I need to change my Astro code?

Not for the CMS itself. You only add a config file. You may need to adjust your code if drafts aren't being filtered yet.

### Should the body be rich text or Markdown?

Use Markdown (`type: code`) if you want full control of the file. Use `rich-text` if you want a visual editor and don't mind less control over the Markdown output.

### Why did my build fail after saving a post?

Most often the form saved something your schema rejects, such as a missing required field or a wrong date type. Compare the post's frontmatter with your schema.

### Will it overwrite my existing posts?

Only if you save changes to them. Opening a post doesn't change it.

For more free tools that fit this workflow, see [the $0 developer stack I use to run two websites](/blog/free-developer-stack-two-websites/).
