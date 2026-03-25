---
title: "Strasza"
date: 2026-03-25
description: "A comprehensive platform for managing World of Warcraft boosting services, featuring a custom Discord UI framework, complex raid booking mechanics, and automated transactional settlement."
summary: "An enterprise-grade boosting marketplace for **World of Warcraft**, featuring a **Custom Discord UI Framework**, dynamic raid booking, and automated transaction settlement."
tags:
  [
    "typescript",
    "nestjs",
    "discord.js",
    "react",
    "postgresql",
    "docker",
    "caddy",
  ]
categories: ["projects"]
showTableOfContents: true
showHero: false
heroStyle: "big"
---

**Strasza** is a large-scale, enterprise-grade platform built to manage World of Warcraft boosting services. Operating as the bridge between Workers (service providers) and Advertisers (businesses/buyers), it manages thousands of real-time interactions daily.

It is comprised of a **NestJS Backend**, a **Custom Discord.js Bot Framework**, and a **React 18** web dashboard. The platform handles everything from complex Raider.io integrations and booking slots to full-fledged automated transaction settlements.

[Live Site](https://strasza.com)

---

## 1. The Custom Discord Framework

Instead of relying on scattered scripts, the Strasza Discord Bot is built on a highly modular, self-healing **Feature-Based Framework** modeled after enterprise backend patterns.

- **Auto-Bootstrapping Server Structure:** On startup, the bot executes an automated diff against the Discord server. Using a central source-of-truth configuration (`SERVER_STRUCTURE`), it automatically creates, updates, and syncs **Roles**, **Categories**, and **Channels**. It strictly applies granular permission overwrites (handling private vs. public visibility, Staff/Admin overrides, and bot-only zones) saving hours of manual setup.
- **Smart Interaction Manager:** Slash commands, buttons, and modals aren't hardcoded into monolithic switch statements. Instead, each "Feature" (e.g., Leveling, Dungeons, PvP, Roles) is injected dynamically. The `InteractionManager` parses incoming Discord events and routes them optimally to their registered handlers in O(1) time.

---

## 2. Dynamic Raid Booking & Simulation Models

World of Warcraft raids require extraordinarily precise team compositions (Armor types, Class tokens, and Roles). Strasza automates this entire orchestration.

- **Automated Slots & Backups:** When a user books a raid via the platform or Discord, the system evaluates real-time capacity. If the raid is full, the booking is automatically transitioned into a sophisticated **Backup State**.
- **VIP Constraints & Raider.io:** Premium "VIP" buyers receive exclusivity. The backend actively filters available classes, armor types (Cloth/Leather/Mail/Plate), and tier tokens based on confirmed bookings, ensuring no two VIPs compete for the same gear drops. Profiles integrate with **Raider.io** APIs to validate character accomplishments and lock in accurate pricing.
- **Complex Pot Ownership:** The system seamlessly handles branching logic for gold collection—whether the pot is held by the **Booker** or the **Raid Leader**. It recalculates projected deltas on the fly, accurately distributing cuts while accounting for House fees without floating-point math errors.

---

## 3. The Settlement Engine & Transaction Logic

Strasza operates effectively as an internal bank for its users. Its transactional engine processes payouts, cuts, and tiered limits invisibly.

- **Denormalized Credit & Tier Limits:** Advertisers are bound by `advertiserTier` limits. When initiating a job, the system calculates `pendingDebt` (the sum of pending transactions + projected raid negative deltas). If an Advertiser crosses their credit limit threshold, job creation is strictly locked down until balances are settled.
- **Automated Settlement & Reporting:** When an Admin settles a batch of requested payouts, the system calculates Revenue Recognition exclusively on negative-sum `JOB` type transactions. It updates denormalized balances, automatically closes parent Jobs, tracks total sales volumetrics, and finally fires an internal webhook summarizing the batch directly to a private Discord channel.
- **Adjustments & Fines:** The system allows Staff to issue granular adjustments (bonuses or fines) directly via Discord menus. These adjustments are instantly reflected on the React dashboard.

---

## 4. End-to-End Type Safety & Releases

Strasza is built with a **Zero-Tolerance Policy for Contract Drift** extending from the Postgres Database entirely down to the Discord Interface.

- **Auto-Documenting API Clients:** The NestJS API uses decorators to expose a live Swagger OpenAPI specification. Using `openapi-typescript-codegen`, both the **React Frontend** and the **Discord Bot** possess pipeline scripts that auto-generate strictly-typed Axios clients. If a DTO property changes in the backend, the bot compilation fails immediately, eliminating silent runtime errors.
- **Continuous Deployment (GHCR):** The CI pipeline builds segmented Docker images for the API, Bot, and Frontend, publishing them natively to the GitHub Container Registry.
- **Production Infrastructure:** The server relies on a `docker-compose` orchestrated setup behind **Caddy** (serving as a reverse proxy managing automated Let's Encrypt SSL certificates). For observability, the stack employs **Dozzle**—authenticating engineering staff to view real-time, low-latency container logs from any device.
