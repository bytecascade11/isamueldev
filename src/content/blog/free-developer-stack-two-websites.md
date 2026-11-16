---
title: "The $0 Developer Stack: Free Tools I Use to Run Two Websites"
description: "The free tools behind ReviByte and iSamuelDev: hosting, code, comments, email and push notifications. What each one does, the real free-plan limits, and the one thing I still pay for."
cover: "/assets/posts/free-developer-stack-cover.jpg" # placeholder: replace with your cover image
alt: "A developer building a website on a smartphone using free tools"
tags: ["astro", "tools", "hosting", "beginners"]
date: 2026-11-16
author: iSamuel 
---

## Overview 

I run two websites. One is ReviByte, a tech and mobile gaming blog that has been live since December 2025. The other is this one, a small Astro blog where I write about building things. I build and maintain both from a phone, and I'm a final-year physics and electronics student in Nigeria, so the budget has always been close to zero.

The title says $0, and that is almost true. The software costs nothing. The domain names cost money, and I'll get to that at the end, because pretending otherwise would make this a worse post.

Free plans change without much warning, so every limit below was checked on 16 November 2026. Check the pricing page before you build around one.

## The stack at a glance

| Tool | What it does | Used on |
| --- | --- | --- |
| Astro | Builds the site as fast, mostly static HTML | Both sites |
| GitHub | Stores the code and gives me a browser editor | Both sites |
| Cloudflare Pages | Hosts and deploys ReviByte | ReviByte |
| Vercel | Hosts and deploys this blog | iSamuelDev |
| Supabase | Stores comments and page view counts | ReviByte |
| Google Search Console | Shows what Google has indexed and what people search | Both sites |
| Microsoft Clarity, Umami | Session recordings and simple traffic counts | ReviByte |
| EmailOctopus | Newsletter campaigns that I write and send by hand | ReviByte |
| OneSignal + Zapier | Push notifications to Chrome and installed-app users when a post goes live | ReviByte |
| Formspree | Receives contact form messages | iSamuelDev theme |

## Code and hosting: where everything lives

Astro does most of the heavy lifting by shipping plain HTML with very little JavaScript. A static site is cheap to host almost anywhere, which is the real reason this whole stack works. The framework side is covered in [what actually helps Astro performance](https://www.isamueldev.blog/blog/astro-performance-tips/).

GitHub holds the code for both sites. I don't have a laptop setup. I edit files in GitHub's browser editor, commit, and the host rebuilds the site by itself. It sounds clumsy, and it is, a little, but it works. The full process is in [how to build an Astro blog on your phone](https://www.isamueldev.blog/blog/build-astro-blog-on-phone/).

ReviByte runs on Cloudflare Pages. The free plan gives unlimited bandwidth and 500 builds a month. That build count matters when you edit from a phone, because every commit triggers one, and a day of small fixes adds up faster than you'd expect.

This blog runs on Vercel's free Hobby plan, and that plan comes with a rule worth reading before you copy me. Hobby is for personal, non-commercial projects. Vercel counts a project as commercial if anyone involved earns money from it. If your site carries ads, sells something or does client work, you need the paid Pro plan or a different host. ReviByte shows ads, which is one reason it lives on Cloudflare.

## Data without a server

A static site has no database, but a blog still wants comments and view counters. I use Supabase for both on ReviByte. Visitors can leave anonymous comments, and each page records its own view count.

The free plan gives you a 500 MB database and two active projects. The catch is that a free project pauses after about a week of little or no database activity, and you have to wake it up by hand. A busy blog rarely goes quiet that long, but a side project can. The free plan also has no automatic backups, so the data is only as safe as the habits you keep. It works for my setup. I wouldn't call it a maintenance-free production database.

## Knowing what is happening

I use Google Search Console on both sites, and it is the most useful free tool I have. It tells me which pages Google has indexed, which queries bring clicks, and what is broken. Most of my real fixes started with a Search Console report, not a hunch.

On ReviByte I add Microsoft Clarity for session recordings, and Umami for simple traffic counts. I use Umami Cloud's free Hobby plan, so there is no server to maintain. Setup took one step: I copied its tracking code into my Astro layout file, and it worked straight away. That is more analytics than a site my size needs. Clarity helps when I want to see where mobile visitors get stuck. The rest mostly repeat each other, and every extra script has a speed cost. Start with one.

## Reaching readers who left

Two tools handle this on ReviByte. One is manual, and one runs on its own.

**EmailOctopus** runs the newsletter. Subscribers sign up through a form on the site, and I write and send each campaign myself, usually when something deserves a proper announcement. I don't automate it. The free plan covers 2,500 subscribers and 10,000 emails a month, and it puts EmailOctopus branding on your emails. Do the math before you send often: with 1,000 subscribers, 10,000 emails is only about ten campaigns a month.

**OneSignal** sends the push notifications. Anyone who allowed notifications in Chrome, or installed the site as an app, gets one when new content goes up. Zapier connects it to my feed. One Zap watches the RSS feed for a new post, and a second step tells OneSignal to send a notification. Each notification uses one of Zapier's free tasks. Publishing one or two posts a day uses roughly 30 to 60 of the 100 monthly tasks. Anything else you automate through Zapier draws from the same 100. The free plan checks for new data every 15 minutes, so the notification can arrive a little after the post goes live.

OneSignal also changed its free plan this autumn. Mobile push and in-app messages are now capped at 1,000 monthly active users. Web push, which is what a blog uses, was not affected, and it has its own free limit of 10,000 subscribers per send. It's a good reminder that free plans move.

## A contact form with no backend

The iSamuelDev theme uses Formspree for its contact form. The form posts to Formspree, and the message lands in my inbox. A static site has nowhere to receive form data on its own, so this small service fills the gap. The free plan allows 50 submissions a month, which is plenty for a personal blog.

## Where free runs out

| Tool | The limit that matters | What happens |
| --- | --- | --- |
| Vercel Hobby | Personal, non-commercial use only | Ads or income mean you need Pro |
| Cloudflare Pages | 500 builds a month | Builds stop until the month resets |
| Supabase | 500 MB database, pauses after about a week of inactivity | You wake the project manually |
| EmailOctopus | 2,500 subscribers, 10,000 emails a month | Upgrade, or stop sending |
| Zapier | 100 tasks a month, 15-minute checks | Automations wait until next month |
| Formspree | 50 submissions a month | Extra messages are blocked until you upgrade |

Check the numbers yourself on the official pages: [Cloudflare Pages limits](https://developers.cloudflare.com/pages/platform/limits/), [Vercel fair use guidelines](https://vercel.com/docs/limits/fair-use-guidelines), [Supabase pricing](https://supabase.com/pricing), [EmailOctopus pricing](https://emailoctopus.com/pricing), [Zapier pricing](https://zapier.com/pricing), [Formspree plans](https://formspree.io/plans) and [OneSignal's free plan announcement](https://onesignal.com/blog/whats-changing-on-onesignals-free-plan/).

The pattern is that volume and commercial rules are where free plans end. Support and price changes come next. I keep every post as a plain Markdown file in a repository I own, so if a host changes its terms I move the files and lose nothing.

## The one thing I pay for

Domains. I renew the ReviByte domain through Cloudflare, and I bought isamueldev.blog this month. There is no free route here that I'd trust for a site I want to keep. If you want to compare the real costs, including what happens when the first-year discount ends, read [what a free developer domain or a .blog actually costs](https://www.isamueldev.blog/blog/free-developer-domain-or-blog-domain-cost/).

Everything else on this list I could rebuild for nothing tomorrow.

## FAQ

### Can you really run a website for $0?

You can run the software side for $0: framework, code storage, hosting, analytics, forms and a small newsletter. A custom domain is the usual exception. A free subdomain such as `yourname.vercel.app` keeps the cost at zero while you learn.

### Do I need a laptop to use this stack?

No. I use a phone and GitHub's web editor for both sites. A laptop is faster and more comfortable, but nothing here requires one.

### Is free hosting good enough for a blog that earns money?

It can be, but read the terms first. Vercel's Hobby plan is for non-commercial use, so a monetized site belongs on Pro or on a host whose free plan allows commercial use. Cloudflare Pages is the one I use for that. Check the current terms before you commit, because moving later takes more work.

### Which free tool should I set up first?

Google Search Console. It shows whether Google can find and index your pages, and it costs nothing. Add analytics, email and push notifications later, once you have readers to measure.

### What happens if a free plan changes?

Your content should survive. Keep posts in Markdown inside your own repository, and any host or service can be swapped out with some effort.

### Do I need Supabase for comments?

No. It is one option for a static site with comments and view counts. If you don't need either, skip it and keep the site simpler.
