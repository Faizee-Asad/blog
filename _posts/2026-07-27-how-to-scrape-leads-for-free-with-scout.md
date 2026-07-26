---
layout: post

title: "How to Scrape Leads from Instagram, TikTok & LinkedIn for Free"
description: "Learn how to scrape verified leads from Instagram, TikTok, LinkedIn, GitHub, YouTube, and more using Scout. Discover free email verification, SMTP validation, bulk scraping, lead scoring, and CSV export without any paid API."

date: 2026-07-27 10:00:00 +0530
author: Asad Faizee

categories:
  - Lead Generation
  - OSINT

tags:
  - scout
  - lead generation
  - instagram scraping
  - tiktok scraping
  - linkedin scraping
  - email verification
  - smtp verification
  - osint
  - python
  - github
  - appointment setters
  - csv export

image:
  path: /assets/img/posts/scout-lead-scraper.webp
  alt: "Scout Lead Scraper for Instagram, TikTok & LinkedIn"

pin: false
comments: true
---

# How to scrape leads from Instagram, TikTok, and LinkedIn for free

Appointment setters spend hours hunting contact info. Scout cuts that to minutes. It's a free, open-source Python CLI tool that pulls leads from 8 social media platforms, verifies email addresses without any paid subscription, scores each contact, and exports everything to a CSV file ready for outreach.

No Hunter.io credits. No PhantomBuster plan. Three commands and you're running.

---

## What is Scout?

Scout (v1.3.1) is a lead generation tool built specifically for appointment setters. It scrapes public profile data from social media, enriches each lead with a verified email address using SMTP verification, and exports a scored contact list to CSV.

It's built in Python, runs from the command line, and the full source code is on GitHub at [github.com/kiryano/Scout](https://github.com/kiryano/Scout).

The tool fills a specific gap. Paid tools like Hunter.io, Apollo, and PhantomBuster are good, but they all charge monthly fees and cap how many contacts you can pull. Scout does the same core job at $0, with no API key required for its main functionality.

---

## Which platforms does Scout scrape?

Scout covers 8 platforms: Instagram, TikTok, LinkedIn, GitHub, YouTube, Twitch, Pinterest, and Linktree (plus bio.link, Stan, and Linkr).

Most lead tools stop at LinkedIn because that's where the B2B buyers are. That's fair, but appointment setters working with creators, coaches, and online entrepreneurs know the real audience is spread across TikTok and YouTube. Scout covers that whole stack.

Here's what it captures per platform:

**Instagram:** profile, bio, follower count, email, phone, links  
**TikTok:** profile, bio, followers, total likes, email  
**LinkedIn:** profile, headline, bio, email (requires a session cookie)  
**GitHub:** profile, bio, repositories, email, website  
**YouTube:** channel name, description, subscriber count, email, links  
**Twitch:** profile, follower count, partner/affiliate status, social links  
**Pinterest:** profile, bio, follower count, pins, website  
**Linktree and similar:** every link in the bio stack

LinkedIn is the only platform that needs any setup. You log into LinkedIn in Chrome, open DevTools (F12), go to Application > Cookies > linkedin.com, copy the `li_at` cookie value, and paste it into a `.env` file. Every other platform works with no login at all.

---

## How does Scout find verified emails without a paid API?

This is where Scout does something most free tools don't. After scraping a profile, it runs a multi-step enrichment sequence to find and verify the email address.

The process goes like this:

1. Checks the bio text for a plainly listed email or phone number
2. Scrapes the lead's website, plus the `/contact` and `/about` pages
3. Detects the company name from the headline (for example, "CEO at BrandName")
4. Finds the company domain through a DNS MX record lookup
5. Reads existing emails on the site to detect the email pattern (first.last@, first@, etc.)
6. Generates email candidates based on that pattern
7. Verifies candidates against the actual mail server via SMTP

That last step is what separates verified from guessed. SMTP verification contacts the mail server directly to confirm whether the address exists, without actually sending an email. No paid API does this for you. Scout does it natively.

If you have a Hunter.io API key, you can add it to `.env` for extra coverage. But the tool functions end-to-end without one.

---

## How does Scout score leads?

Every lead gets a score from 0 to 100. The score is calculated from a set of weighted signals pulled directly from the enrichment data.

The scoring breakdown from the source code:

- **+30 points** for having an email address
- **+30 points** for having a phone number
- **+10 points** if the account is verified on the platform
- **+15 points** for followers in the 5,000 to 50,000 range (the micro-influencer sweet spot)
- **+10 points** for followers between 1,000 and 100,000
- **+10 points** for having a website
- **+5 points** if the bio contains keywords like "coach," "founder," "CEO," "consultant," or "agency"

The score caps at 100. A lead with an email, a phone number, 12,000 followers, and "founder" in the bio could hit 90+.

This matters practically. Appointment setters working bulk lists don't call every contact in order of scrape. They sort by score and work the top tier first. A 85-score lead who listed their email in their bio and has a verified website is a different conversation than a 25-score lead who just had a follower count.

---

## How do you install and run Scout?

Three commands:

```bash
git clone https://github.com/kiryano/Scout.git
cd Scout
pip install -r requirements.txt
```

Copy the example environment file:

```bash
cp .env.example .env
```

Then run:

```bash
python scout.py
```

Scout launches an interactive CLI menu. You pick the platform, enter a username, or load a bulk list from a CSV or TXT file. Add `--verbose` if you want debug output while it runs.

For proxy setup, you have 3 options. Set `SCOUT_PROXY` in `.env` for a single proxy, `SCOUT_PROXY_FILE` to point at a file of rotating proxies (one per line), or `SCOUT_FREE_PROXY=true` to use free anonymous proxies. All scrapers work on a direct connection with no proxy at all, though higher-volume runs benefit from rotation.

---

## How does bulk scraping work?

Scout accepts a CSV or TXT file of usernames and runs the full enrichment pipeline on every entry automatically.

This is where the time savings become real. Load 300 TikTok usernames from a niche you're prospecting, run Scout overnight, wake up to a scored CSV. That used to take a full workday of manual research, or a $150 monthly tool subscription.

The scrape delay is randomized between 1.5 and 4.5 seconds between requests. This isn't just a courtesy to the platforms. It's what keeps your IP from getting rate-limited mid-run. Scout also rotates user agents across 10 browser signatures to reduce detection risk.

---

## Is scraping social media for leads legal?

The short answer is: scraping publicly available data is generally legal in the US, and a major court case settled this specific question.

In 2022, the Ninth Circuit Court of Appeals ruled in *hiQ Labs v. LinkedIn* that scraping publicly accessible profile data doesn't violate the Computer Fraud and Abuse Act. The ruling covered LinkedIn specifically, but the legal reasoning applies broadly: public data is public.

Scout collects what people have chosen to display openly. Bio text, listed emails, follower counts, website links. It doesn't access private messages, locked accounts, or anything behind authentication (other than LinkedIn, where you use your own account session).

Platform terms of service are a separate issue from law. Instagram and TikTok prohibit automated scraping in their ToS. Breaking ToS can get an account flagged or an IP blocked, but it isn't a legal violation. Most appointment setters and outreach agencies accept that tradeoff.

The practical advice: use proxies for volume, respect rate limits, and don't scrape at a scale that creates liability exposure for your business.

---

## What are Scout's real limitations?

Instagram can require retries depending on your IP or region. TikTok serves CAPTCHAs on some requests, particularly without a proxy. LinkedIn cookies expire periodically, so you'll need to refresh your `li_at` value when LinkedIn scrapes stop returning data. GitHub's API limits unauthenticated requests to 60 per hour; add a personal access token for higher volume.

Some mail servers block SMTP verification entirely. When that happens, Scout still generates the most likely email candidates based on the detected pattern, but flags them with a lower confidence score rather than marking them verified.

Free proxies are also unreliable. The tool is honest about this. If you're running hundreds of profiles per session, residential or rotating proxies give more consistent results.

---

## How does Scout compare to paid tools?

Here's a direct comparison across the features that matter most to appointment setters:

| Feature | Scout | Hunter.io | PhantomBuster | Apollo |
|---|---|---|---|---|
| Price | Free | Paid plans | Paid plans | Paid plans |
| Platforms | 8 | Web + LinkedIn | LinkedIn + others | LinkedIn + web |
| SMTP verification | Yes | Yes | No | Yes |
| No API key needed | Yes | No | No | No |
| CSV export | Yes | Yes | Yes | Yes |
| Open source | Yes | No | No | No |
| Bulk scraping | Yes | Limited | Yes | Yes |
| Lead scoring | Yes | No | No | Partial |

The honest tradeoff: Hunter.io and Apollo maintain large pre-verified databases. If a contact is in their index, you get the email instantly. Scout discovers contacts in real time by scraping and verifying fresh, which means current data but also occasional gaps on harder-to-find addresses.

For an appointment setter who's paying out of pocket, that tradeoff is usually worth it.

---

## Who should use Scout?

Appointment setters running outreach campaigns without a tool budget. Solo founders building an initial prospect list. Freelancers who need 200 verified leads and don't want to pay a monthly subscription to get them.

The tool runs in a terminal, so some comfort with a command line helps. But the interactive menu is clean enough that you don't need any programming background to use it. If you can follow the three-step install, you can run Scout.

Teams needing a shared dashboard, CRM integrations, or multi-user workflows will want something else for now. The Scout repo notes that a full web app with dashboard and team features is in development. The Discord community at [discord.gg/eneDNUbzcc](https://discord.gg/eneDNUbzcc) is where early access and feedback requests are being collected.

---

**Key facts at a glance:**
- Scout version 1.3.1, MIT license, free and open source
- 8 supported platforms: Instagram, TikTok, LinkedIn, GitHub, YouTube, Twitch, Pinterest, Linktree
- 7-step email enrichment with SMTP verification, no paid API required
- Lead scoring from 0 to 100 based on email, phone, followers, bio keywords, and website presence
- Bulk scraping from CSV or TXT username lists
- Randomized 1.5 to 4.5 second scrape delay with 10-signature user-agent rotation
- Optional Hunter.io integration, optional proxy support (single, file, or free)
- *hiQ Labs v. LinkedIn* (9th Circuit, 2022) established the legal basis for scraping public profile data
