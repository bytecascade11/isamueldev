---
title: "How to Connect a Custom Domain to Vercel (Step by Step, Even From Your Phone)"
description: "Connect your own domain to a Vercel project using A and CNAME records or Vercel nameservers. Includes DNS tables, fixes for common errors, and an FAQ."
date: 2026-11-18T06:30:00
author: iSamuel 
tags:
  - vercel
  - domains
  - dns
  - astro
cover: "/assets/posts/connect-custom-domain-vercel-cover.jpg"
alt: "Vercel dashboard showing a custom domain with a valid configuration status"
---

Your project already works at `yourproject.vercel.app`. The problem is that nobody trusts a link that looks like a test deployment, and nobody remembers it either. A custom domain fixes both, and the setup takes about ten minutes of actual work. The rest is waiting on DNS.

This guide covers the two ways to connect a domain, which one to pick, and what to do when Vercel keeps showing "Invalid Configuration."

## Before you start

You need three things:

1. A project already deployed on Vercel. Custom domains work on the free Hobby plan, but read the note below about what Hobby allows.
2. A domain you own. If you haven't bought one yet, read [what a blog domain really costs and which free options exist](https://www.isamueldev.blog/blog/free-developer-domain-or-blog-domain-cost/) first.
3. Access to the DNS settings at your registrar, such as Namecheap, GoDaddy, Cloudflare or Porkbun.

> **Check your plan first.** Vercel's Hobby plan is limited to non-commercial personal use. A portfolio, a learning project or a plain personal blog is fine, and simply asking for donations doesn't count as commercial. Advertisements (Google AdSense included) and sites built mainly around affiliate links do count. If your site sells something or runs ads, don't assume Hobby qualifies: check Vercel's [fair use guidelines](https://vercel.com/docs/limits/fair-use-guidelines) and choose a commercial plan if necessary.

Everything below works in a mobile browser. If a dashboard hides a menu, switch your browser to desktop mode. That is how many people, [myself included, build and ship sites without a laptop](https://www.isamueldev.blog/blog/build-astro-blog-on-phone/).

## Step 1: Add the domain inside Vercel

Open your project, go to **Settings**, then **Domains**, type your domain and press **Add**. Vercel will usually suggest adding both the bare domain (`example.com`) and the `www` version, with one redirecting to the other. Accept that. Pick one as your main address and let the other forward to it, otherwise search engines may see two copies of your site.

After you add it, Vercel shows the exact DNS values for your project. Use those values. Some tutorials show a generic CNAME target, but Vercel may assign a unique one to your project. Always copy the exact value displayed in your dashboard, and treat every value in this post as an example. Vercel's own [custom domain guide](https://vercel.com/docs/domains/working-with-domains/add-a-domain) covers the same flow.

## Step 2: Choose a DNS method

You can keep your registrar's DNS and add records, or hand DNS over to Vercel by changing nameservers.

| | A + CNAME records | Vercel nameservers |
|---|---|---|
| Where DNS lives | Your registrar | Vercel |
| Setup effort | Two records | Change nameservers, re-create other records |
| Propagation | Usually minutes | Can take 24 to 48 hours |
| Email records (MX) | Untouched | You must add them again |
| Best for | Keeping DNS where it is, minimal changes | Letting Vercel manage the whole DNS zone, wildcard domains |

Records are the lower-risk choice because nothing else about your domain changes. Switching nameservers hands the entire DNS zone to Vercel. That is a fine choice if you want everything managed in one place or need wildcard domains, but you must first copy every existing record (email MX records especially) into Vercel, or those services will break.

## Step 3: Add the DNS records

At your registrar, open the DNS zone for the domain and add:

| Type | Name / Host | Value | Purpose |
|---|---|---|---|
| A | `@` | The value in your dashboard (commonly `76.76.21.21`) | Points the bare domain to Vercel |
| CNAME | `www` | Your project's own value from the dashboard (it looks like `d1d4fc829fe7bc7c.vercel-dns-017.com`) | Points the www version to Vercel |

Two details trip people up:

- Remove conflicting or outdated A, AAAA or CNAME records for the specific hostname you're connecting, such as an old parking-page record. Don't delete unrelated DNS records, especially those used for email or other services.
- Some registrars want the trailing dot in a CNAME value and some add it for you. Copy the value exactly as Vercel shows it.

If you use Cloudflare and verification stalls, try setting these two records to **DNS only** (the grey cloud) temporarily. Removing the proxy from the picture makes it easier to tell whether the problem is your DNS records or something in the certificate step. Turn it back on afterwards if you want it.

## Step 4: Wait, then verify

Go back to **Settings > Domains** in Vercel and refresh. When the status turns to a green check, the domain is live and Vercel has issued an SSL certificate for you automatically. You don't buy or install anything for HTTPS.

Record changes are usually quick, and nameserver changes are the slow ones. If the status doesn't change, check the records first for typos or leftover conflicts, then give DNS caches extra time to catch up, since different resolvers update at different speeds. You don't need to redeploy your site for any of this. DNS changes happen outside your code.

## Common errors and fixes

Vercel also keeps an official [domain troubleshooting guide](https://vercel.com/docs/domains/troubleshooting) if your case isn't below.

| Symptom | Likely cause | Fix |
|---|---|---|
| "Invalid Configuration" stays on | Wrong or conflicting record | Compare your records to the dashboard values, remove old ones |
| Site loads on `www` but not the bare domain | Missing A record on `@` | Add the A record |
| Certificate pending for a long time | A proxy or CAA record getting in the way | Temporarily set DNS only to isolate it, check CAA records |
| Email stopped working after the change | You moved to Vercel nameservers | Re-add your MX records |
| "Domain is already in use" | Domain is attached to another Vercel account | Add the TXT verification record Vercel asks for |

## Redirect www to the bare domain

In **Settings > Domains**, open the edit option beside the `www` entry and choose to redirect it to your bare domain (for example `www.example.com` to `example.com`). Use a permanent redirect so search engines treat the bare domain as the main address. Do the reverse if you prefer `www` as your main address.

## After it's live

Update the canonical URL and `site` value in your project config so your sitemap, RSS feed and meta tags use the new domain, not the `vercel.app` one. If you run Astro, that is the `site` field in `astro.config.mjs`. Then push the change and re-submit your sitemap in Google Search Console.

Once the domain is stable, a quick performance pass pays off too. These [Astro performance tips](https://www.isamueldev.blog/blog/astro-performance-tips/) are a good next read, and if you're running more than one project on the free tier, see how to [run two websites on a free developer stack](https://www.isamueldev.blog/blog/free-developer-stack-two-websites/).

## FAQ

### Can I use a custom domain on Vercel's free plan?

Yes, Hobby supports custom domains with automatic HTTPS, and Vercel's docs list a limit of 50 per project. The catch is the plan's license: Hobby is for non-commercial personal use, so sites with ads or affiliate-first content need Pro. A plain donation button doesn't count as commercial use.

### Do I have to buy the domain from Vercel?

No. You can buy from any registrar and point it at Vercel. Buying through Vercel is just more convenient because the DNS is already connected.

### How long does it take for the domain to work?

Standard A and CNAME changes usually show up within minutes. Changing nameservers can take up to 24 to 48 hours to spread fully.

### Should I use www or the bare domain?

Either works. Choose one as your main address and redirect the other to it. Consistency matters more than which you pick.

### Why does Vercel say my domain is misconfigured when the site opens?

The dashboard can lag behind real DNS, or one record may be correct while the other is missing. Check both the `@` and `www` records and refresh after a few minutes.

### Do I need to set up SSL myself?

No. Vercel issues and renews the certificate automatically once DNS points to the project.

### Will connecting a domain break my email?

Not with the A and CNAME method, since your MX records stay where they are. If you move to Vercel nameservers, you must re-create your MX records or email will stop arriving.
