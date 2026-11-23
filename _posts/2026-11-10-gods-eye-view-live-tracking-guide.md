---
layout: post
title: "God's Eye View: track live flights, ships and satellites on an open-source 3D globe (2026 guide)"
description: "God's Eye View is a free, open-source 3D globe that tracks real flights, ships, satellites, earthquakes and storm systems live in your browser. Here's what it does, where its data comes from, and how to run it locally."
date: 2026-11-10 19:10:00 +0530
author: Asad Faizee

categories:
  - AI Tools
  - Open Source

tags:
  - God's Eye View
  - live flight tracker
  - flight tracking
  - ADS-B
  - ship tracking
  - AIS
  - satellite tracker
  - earthquake map
  - OSINT tools
  - 3D globe
  - Cesium
  - OpenStreetMap
  - OpenSky Network
  - geospatial intelligence
  - open source
  - Pinokio
  - developer tools
  - public data

image:
  path: /assets/img/posts/gods-eye-view-3d-globe-live-tracking.webp
  alt: "God's Eye View open-source 3D globe tracking live flights, ships and satellites in the browser"

pin: false
comments: true
---

## God's Eye View, in one paragraph

God's Eye View is a free, open-source project that turns your browser into a spy-movie tracking station. You get a photorealistic 3D globe where real aircraft, ships, satellites, earthquakes and storm systems move live, plus a voice agent you can ask questions about whatever you're watching.

It runs on your machine, starts without a single API key, and racked up 64,116 GitHub stars in under 5 months. It's part of the same local-first wave I liked in [Atomic Chat, the offline AI chatbot](https://blog.asadfaizee.is-a.dev/posts/atomic-chat-offline-ai-chatbot-local-llm/): inspectable code, your data stays yours, no account required.

## What is God's Eye View?

God's Eye View is a browser-based "spy satellite simulator," and every feed on screen is real. The README describes it as "live open source spatial intelligence on a photorealistic 3D globe," and that description is unusually honest marketing.

Bilawal Sidhu created the repo on June 22, 2026. By August it sat at #1 on GitHub Trending, both daily and weekly. It hit #8 Product of the Day on Product Hunt (Chris Messina hunted it), Brendan Eich, co-creator of JavaScript, called it "pretty cool," and demo videos pulled over 5 million YouTube views.

The vibe is pure cinema: a cockpit view that rides along tracked flights, a contacts roster of everything within 250 km, GLSL sensor skins that make the screen look like CRT, night vision, FLIR or noir film, and a 3D hangar with detailed models for a Boeing 787, an ATR-72, a Citation jet, a Bell 206 helicopter and an MQ-9 drone. There's even a recreation of the Bhote Koshi flood scene in Nepal you can fly through.

And here's the trick that makes it special: all of it runs on public data anyone can access.

## Where does God's Eye View's live data come from?

Public feeds. God's Eye View aggregates open data sources and renders them on a 3D globe, with a DATA_SOURCES.md file in the [GitHub repo](https://github.com/bilawalsidhu/gods-eye-view) that lists every feed and its license, so you can check the receipts yourself.

Here's the short version:

- **Flights:** the OpenSky Network and adsb.lol, both built on ADS-B signals that aircraft broadcast openly.
- **Weather:** NOAA's GFS model (US public domain) and ECMWF's IFS model (CC BY 4.0), plus NOAA nowCOAST radar and lightning feeds.
- **Storms:** live cyclone data from the National Hurricane Center and Central Pacific Hurricane Center.
- **Cameras:** license plate reader locations from OpenStreetMap (ODbL license), plus street-level imagery from Mapillary.
- **Maps:** OpenStreetMap for roads, buildings and camera positions.

The code itself is MIT-licensed, with carve-outs for third-party data that carries its own terms (more on that in the limitations section, because it matters).

## What can you actually track live on the 3D globe?

### Live flights (ADS-B)

Yes, real aircraft, moving in real time. Every airliner broadcasts ADS-B, a radio signal carrying its position, altitude, speed and callsign, and networks of volunteer receivers collect those signals and publish them.

The scale is wild. Flightradar24, which runs the largest ADS-B receiver network with over 50,000 receivers, tracked a record 153,359 commercial flights in a single day on July 23, 2026, and 289,586 total flights on April 8, 2026 ([Flightradar24 blog](https://www.flightradar24.com/blog/inside-flightradar24/july-2026-a-record-setting-month-for-flight-tracking/)). God's Eye View taps that same class of public signal.

In the app you click any aircraft to track it, pull its metadata, and jump into cockpit view. The contacts roster lists everything airborne within 250 km, and share links serialize your tracked targets so you can hand someone your exact scenario.

### Ships (AIS)

Yes, vessels too. Ships broadcast AIS, the maritime cousin of ADS-B, and international law makes most of them do it. SOLAS regulation V/19 requires AIS on all ships of 300 gross tonnage and up on international voyages, cargo ships of 500 GT and up not on international voyages, and every passenger ship regardless of size ([IMO](https://www.imo.org/en/ourwork/safety/pages/ais.aspx)). The rule took full effect on December 31, 2004.

So when a cargo ship blips across your globe, that position comes from a transponder the ship is legally required to run.

### Satellites

Yes, with pass times. ESA's space environment statistics from November 2026 count roughly 15,800 functioning satellites among 19,610 still in orbit, with surveillance networks tracking about 47,280 objects including rocket bodies and debris ([ESA statistics via New Space Economy](https://newspaceeconomy.ca/2026/11/03/updated-esa-space-environment-statistics/)). God's Eye View shows you satellite passes over your location so you know when to look up.

### Earthquakes

Yes, global seismic events land on the globe as they happen. The USGS National Earthquake Information Center locates about 20,000 earthquakes worldwide each year, roughly 55 per day ([USGS](https://www.usgs.gov/faqs/why-are-we-having-so-many-or-so-few-earthquakes-has-naturally-occurring-earthquake-activity)), so the globe has a steady pulse of real events to display.

### Cameras, weather and storm systems

Yes, and this is where it gets weirdly practical. God's Eye View maps license plate reader camera locations from OpenStreetMap, overlays live NOAA radar and lightning, tracks active cyclones, and pulls street-level imagery.

The data behind those layers is enormous. OpenStreetMap has been built by nearly 2.4 million contributors who've made over 3.2 billion edits since 2005 ([HeiGIT](https://heigit.org/new-user-statistics-in-ohsomenow/)), and Mapillary hosts well over 2.4 billion crowdsourced street-level images ([Mapillary](https://help.mapillary.com/hc/en-us/articles/115001770269-An-Introduction-to-Mapillary)).

There's also a keyless search: type a place or paste a `geo:` URI and the globe flies there. No account, no key.

## How do you install God's Eye View?

Three ways, from easiest to most manual.

**Option 1: Pinokio (one click).** If you run Pinokio 8.2 or later, install God's Eye View from its listing and press start. That's the whole tutorial.

**Option 2: Terminal.** You need Node.js 24.x or 26.x, then:

    git clone https://github.com/bilawalsidhu/gods-eye-view.git
    cd gods-eye-view
    npm ci
    npm run doctor
    npm run dev

Then open `http://localhost:4173`. The doctor command checks your setup before you flail around in the dark, which I appreciate more than I can say.

**Option 3: Docker.** Standard container setup if that's your workflow.

The app starts keyless. You can add a Cesium ion token or a Google Maps key later for photorealistic 3D tiles, but the base experience needs neither. Median cold start is 1.86 seconds on an M5 Mac with Chrome, which is faster than most web apps manage with zero globe rendering.

## Is God's Eye View legal to use?

Yes, and that's the point. Everything it displays comes from public feeds that governments, volunteers and research networks publish on purpose. ADS-B and AIS are broadcast in the clear by design. Weather models from NOAA are US public domain. OSM camera locations are things mappers photographed on public streets.

The license plate reader layer shows camera locations only, from OSM data, and doesn't pipe in any plate reads. It's a map of infrastructure that already exists around you, which some people find calming and others find motivating enough to attend a city council meeting.

Worth knowing: the repo is upfront that bundled TeleGeography data is NonCommercial, and the OpenSky feed is licensed for non-commercial research (their founding paper is Schäfer et al., IPSN 2014, if you want the citation for your own writeup). Casual use is fine. Building a paid product on top means reading DATA_SOURCES.md first.

## What are the honest limitations?

The README admits its own soft spots, which is rarer than it should be:

- **Road traffic is simulated** along real roads. Cars moving on the globe aren't live vehicles.
- **CCTV camera poses and rocket trajectories are coarse estimates**, not telemetry.
- **Some feeds have license strings attached**, like OpenSky's non-commercial terms and the TeleGeography carve-out.
- **The prettiest tiles want a key.** Keyless mode looks good; photorealistic 3D tiles ask for a Cesium ion or Google Maps key.
- **It's a young project.** The repo has 401 open issues and 3,497 commits as I write, so expect breakage and fast change.

None of that breaks the experience. It just means you should read it as a stunning visualization of real public data, with a few theatrical props labeled as such.

## Is God's Eye View worth trying in 2026?

Yes, if any of these describe you: flight nerd, OSINT learner, geospatial developer, or someone who wants a jaw-dropping demo of what public data plus open code can do.

The README says it best: "Half the magic is that it looks like a forbidden cockpit. The other half is that every line of code is inspectable." That second half is why I keep writing about this category. Tools like God's Eye View and the offline chatbots I covered before trade on transparency instead of lock-in, and they're better products for it.

Run it once with the FLIR skin on, track a red-eye over the Pacific, and you'll understand why 64,000 people starred a globe.

## FAQ

**Is God's Eye View free?**
Yes. The code is MIT-licensed and the app starts without any API key. Optional Cesium or Google Maps keys only add photorealistic tiles.

**Is God's Eye View's data real?**
Mostly, yes. Flights, ships, quakes, satellites and weather come from live public feeds. Road traffic is simulated, and CCTV/rocket positioning is approximate. The README labels all of this.

**Can I use God's Eye View commercially?**
The app's own use is fine, but its data sources carry mixed licenses. OpenSky is non-commercial research, and bundled TeleGeography data is NonCommercial. Read DATA_SOURCES.md before building anything paid on top.

**Does God's Eye View need a GPU?**
No. It renders in the browser and starts in about 1.86 seconds on a plain M5/Chrome setup. No local model download is involved.

## Sources

- [God's Eye View GitHub repository](https://github.com/bilawalsidhu/gods-eye-view)
- [IMO, AIS transponders and SOLAS V/19 carriage requirements](https://www.imo.org/en/ourwork/safety/pages/ais.aspx)
- [Flightradar24, July 2026 record flight tracking](https://www.flightradar24.com/blog/inside-flightradar24/july-2026-a-record-setting-month-for-flight-tracking/)
- [USGS FAQ on global earthquake frequency](https://www.usgs.gov/faqs/why-are-we-having-so-many-or-so-few-earthquakes-has-naturally-occurring-earthquake-activity)
- [ESA space environment statistics, November 2026](https://newspaceeconomy.ca/2026/11/03/updated-esa-space-environment-statistics/)
- [HeiGIT, OpenStreetMap contributor statistics](https://heigit.org/new-user-statistics-in-ohsomenow/)
- [Mapillary, platform introduction and image counts](https://help.mapillary.com/hc/en-us/articles/115001770269-An-Introduction-to-Mapillary)
