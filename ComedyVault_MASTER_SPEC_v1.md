# ComedyVault Master Spec Document
## Version 1.0 | Stage 2: Spec and Scope Lock | Gate 1 Sign-Off Required

**Document Authority:** DDBA / DeLuca Designs LLC  
**Platform Owner:** Adetona "Abe" Adewale | beatsbydummadrumma@gmail.com | (510) 575-8954  
**Project Manager:** Frank DeLuca | DeLuca Designs LLC  
**Target Launch:** July 4, 2026  
**Build Status:** Pre-build. Gate 1 sign-off required before Sprint 1.

> **HARD STOP:** This document is the single source of truth for the ComedyVault build. Every decision made during Sprints 1 through 7 traces back to a statement written here. If a question arises during the build that this document does not answer, stop. Contact DDBA. Do not make assumptions and build.

---

## Open Blockers — Read Before Any Session

The following blockers are active. Any work item that depends on these must be flagged before it is attempted.

| # | Blocker | Impact | Owner |
|---|---------|--------|-------|
| 1 | ComedyVault LLC not yet formed (California) | Blocks Stripe live mode, Apple Developer, Google Play Console, all live revenue collection | Abe |
| 2 | Platform trademark search not completed | "ComedyVault" not confirmed clear | Abe |
| 3 | Base44 pricing not confirmed | Architecture session required before budget planning at scale | DDBA |
| 4 | GHL Capability Audit Pass 2 not yet complete | Build team confirmation required before spec is locked | DDBA + Build Team |
| 5 | Apple Developer and Google Play Console accounts not created | Blocked by LLC | Abe |
| 6 | App Store IAP pricing decision not made | Mobile billing economics unresolved | DDBA |
| 7 | Privacy Policy, Terms of Service, Revenue Share Agreement, Refund Policy not yet live | Required before public launch | Abe + Legal |

---

## Table of Contents

1. [Platform Overview and Build Context](#1-platform-overview-and-build-context)
2. [Technology Stack](#2-technology-stack)
3. [GHL Platform Configuration](#3-ghl-platform-configuration)
4. [User Account Architecture](#4-user-account-architecture)
5. [Permission Matrix](#5-permission-matrix)
6. [Data Architecture and Custom Fields](#6-data-architecture-and-custom-fields)
7. [Automation Workflow Library](#7-automation-workflow-library)
8. [Monetization Logic](#8-monetization-logic)
9. [Content Rules and Moderation](#9-content-rules-and-moderation)
10. [Discovery Algorithm Specification](#10-discovery-algorithm-specification)
11. [MVP Feature Cards and Screen Inventory](#11-mvp-feature-cards-and-screen-inventory)
12. [Sprint Build Timeline](#12-sprint-build-timeline)
13. [Integration Specifications](#13-integration-specifications)
14. [Subscription Gating Architecture](#14-subscription-gating-architecture)
15. [Two-Tier Content Moderation Architecture](#15-two-tier-content-moderation-architecture)
16. [Analytics Dashboard Technical Spec](#16-analytics-dashboard-technical-spec)
17. [Mobile App and App Store Configuration](#17-mobile-app-and-app-store-configuration)
18. [Notification System](#18-notification-system)
19. [Edge Cases and Error Handling](#19-edge-cases-and-error-handling)
20. [Security and Compliance Requirements](#20-security-and-compliance-requirements)
21. [Deferred Feature Log](#21-deferred-feature-log)
22. [Dev Handoff Protocol and Build Team Operating Rules](#22-dev-handoff-protocol-and-build-team-operating-rules)
23. [Pricing Architecture Reference](#23-pricing-architecture-reference)
24. [Change Control Protocol](#24-change-control-protocol)
25. [Gate 1 Sign-Off](#25-gate-1-sign-off)

---

## 1. Platform Overview and Build Context

### 1.1 Platform Definition

ComedyVault is a dual-sided marketplace and community platform. Two distinct user types exist: Comedians (supply side) and Fans (demand side). The platform's core function is discovery — surfacing unknown comedians to fans based on content quality and engagement rate, not existing social capital.

**Differentiation Statement:** ComedyVault is the only platform built exclusively for underground comedy discovery where unknown comedians earn from their first subscriber and fans find talent before the algorithm does.

### 1.2 Platform Identity Table

| Field | Value |
|-------|-------|
| Working Title | ComedyVault (name to be confirmed after trademark search) |
| Platform Type | Dual-sided consumer marketplace and community |
| Primary Build Platform | GoHighLevel (GHL) — Agency Unlimited or SaaS Pro plan required |
| Supporting Platforms | Mux (video/audio), Daily.co (live rooms), Stripe Connect Express (payouts), Base44 (discovery algorithm), Claude Code (analytics dashboard) |
| Mobile Delivery | GHL white-label iOS and Android app. PWA available as launch-day fallback. |
| Web Delivery | GHL-hosted web app at platform domain |
| Target Launch Date | July 4, 2026 |
| Client Owner | Abe Adewale — ComedyVault LLC (California, in formation) |
| DDBA Role | Architecture, build oversight, QA sign-off, growth strategy |
| GHL Build Team Role | Execute build against this spec. Weekly sprint delivery. Contact DDBA for any spec deviation. |
| Claude Code Role | Finalization layer for features exceeding GHL native capability: analytics dashboard, custom payout logic |
| Base44 Role | Discovery algorithm database and engagement rate ranking engine |
| Document Version | v1.0 — Draft for Gate 1 sign-off |

### 1.3 Problem Statement

Underground comedians have no dedicated platform to build their brand, earn from their craft, or get discovered by fans at scale. Existing platforms reward existing social capital. Comedy clubs reward existing relationships. Neither rewards raw talent or authentic performance ability.

**Fan Problem:** Cannot find new underground talent. Existing platforms surface the already-famous. Fans of unfiltered comedy have no platform that allows adult humor without risk of algorithmic suppression or account bans.

### 1.4 Key Differentiators

| Differentiator | Description |
|----------------|-------------|
| Discovery Algorithm | Surfaces comedians by engagement rate per impression, not follower count. A comedian with 20 profile views and 18 engagements ranks higher than a comedian with 5,000 views and 50 engagements. |
| Comedy-Native Content Policy | Two-tier moderation: Kids Mode and Adult Mode. Adult Mode allows profanity and edgy material that social platforms suppress. |
| Direct Monetization from Day One | Subscription to individual comedian available from first fan. No follower threshold. |
| Two-Sided Value Lock | Comedians need fans to earn. Fans need comedians to discover. Each side growth fuels the other automatically. |

---

## 2. Technology Stack

### 2.1 Confirmed Stack

| System | Role | Plan / Tier | Monthly Cost | Account Owner | Status |
|--------|------|-------------|--------------|---------------|--------|
| GoHighLevel (GHL) | Primary build platform — frontend UI, accounts, membership, automation, payments, email, white-label app | Agency Unlimited or SaaS Pro | $297 or $497/mo | Abe Adewale | PENDING — plan tier to confirm |
| Mux | Video and audio hosting, transcoding, CDN delivery, play analytics | Pay-as-you-go | ~$150-$400/mo at scale | DDBA / ComedyVault | PENDING — account creation |
| Daily.co | Live room infrastructure — real-time video/audio, room management, recordings | Pay-as-you-go | ~$450/mo at 50 rooms | DDBA / ComedyVault | PENDING — account creation |
| Stripe (Platform) | Fan subscription collection, comedian unlock, direct tips | Standard | 2.9% + $0.30/transaction | Abe Adewale | PENDING — requires LLC |
| Stripe Connect Express | Automated 80/20 revenue split payouts to comedian accounts | Express | Per-payout Stripe fees | Abe Adewale | PENDING — requires Stripe platform |
| Base44 | Discovery algorithm database, engagement rate calculations, ranked list API | TBD | TBD | DDBA | PENDING — architecture session required |
| Claude Code | Custom analytics dashboard, any feature exceeding GHL native capability | N/A | Included in DDBA engagement | DDBA | ACTIVE — used in Sprint 5 |
| GHL White-Label App | iOS and Android delivery under ComedyVault brand | Included in GHL Agency plan | Included | Abe Adewale | PENDING — requires Apple Developer + Google Play |
| Apple Developer Program | Required to publish ComedyVault to iOS App Store | Organization account | $99/year | ComedyVault LLC | PENDING — requires LLC |
| Google Play Console | Required to publish ComedyVault to Android | Standard | $25 one-time | ComedyVault LLC | PENDING |

### 2.2 Stack Monthly Cost Estimate

| System | Pre-Launch | At 1,000 Users | At 10,000 Users |
|--------|------------|----------------|-----------------|
| GoHighLevel | $297/mo | $297/mo | $297-$497/mo |
| Mux (video/audio hosting) | $0 (no content yet) | ~$150/mo | ~$1,200/mo |
| Daily.co (live rooms) | $0 (no rooms yet) | ~$200/mo | ~$2,000/mo |
| Stripe (processing fees) | $0 (no revenue) | ~$50-$100/mo | ~$500-$1,000/mo |
| Base44 (discovery engine) | TBD | TBD | TBD |
| Apple Developer | $8.25/mo (annualized) | $8.25/mo | $8.25/mo |
| Google Play | $2.08/mo (annualized) | $2.08/mo | $2.08/mo |
| **TOTAL ESTIMATE** | **~$310/mo** | **~$720/mo** | **~$4,010/mo** |

---

## 3. GHL Platform Configuration

### 3.1 GHL Account Structure

ComedyVault operates as a single GHL sub-account under the DDBA agency account. All platform data, contacts, automation workflows, membership products, and web pages live in this sub-account.

**GHL Sub-Account Configuration Checklist:**

| Item | Value / Instruction |
|------|---------------------|
| Sub-Account Name | ComedyVault |
| Agency Account Owner | Frank DeLuca / DDBA |
| Sub-Account Owner | Abe Adewale (ComedyVault email) |
| Custom Domain | comedyvault.com or confirmed domain — connect after domain purchase |
| Timezone | Pacific Time (Abe's timezone) |
| Business Type | Entertainment / Media |
| Stripe Connected | Required before any subscription products are created |
| White-Label App | Activate in GHL Mobile App section. Requires Apple Developer and Google Play accounts. |
| SMS / Email Sending Domain | Configure branded sending domain before any automation workflows send emails |
| Twilio or LC Phone | Configure if SMS notifications are included in launch notification system |

### 3.2 GHL Membership Product Configuration

Two membership products must be created in GHL before any subscription workflow is built.

| Product | Configuration | GHL Setup Steps |
|---------|---------------|-----------------|
| Fan Free Tier | No charge. Auto-enrolled on fan registration. Grants: discovery browse, comedian profiles, 2-minute previews, follow/save/share, events, Kids or Adult mode. | 1. Create Membership Product: ComedyVault Free. 2. Set price $0. 3. Configure content access: preview only. 4. Auto-enroll new fan contacts via WF-001. |
| Fan Premium Tier | $6.99/month recurring. Activated via Stripe checkout. Grants full content access plus exclusive drops. | 1. Create Membership Product: ComedyVault Premium. 2. Set price $6.99/month recurring. 3. Connect to Stripe product. 4. Configure content access: full. 5. Tier upgrade triggered by WF-003 on Stripe payment confirmation. |
| Comedian Account | No subscription charge. One-time $16 monetization unlock. Grants: profile, content upload, analytics, live rooms, community, events. | 1. Create Contact Type tag: Comedian. 2. Create monetization unlock product: $16 one-time. 3. Comedian access controlled by AccountType field, not membership product. 4. WF-002 assigns Comedian tag on registration. |

---

## 4. User Account Architecture

ComedyVault has three account types with distinct access rights, data structures, and navigation experiences. Account type is set at registration and cannot be changed by the user. Admin accounts are created by ComedyVault only.

### 4.1 Account Types

| Account Type | Sub-Type | Created By | Primary Capabilities |
|--------------|----------|------------|---------------------|
| Fan | Free Tier | Self-registration | Browse discovery, follow/save comedians, 2-min content preview, attend events, Kids or Adult mode |
| Fan | Premium Tier | Upgrade from Free via Stripe | All Free capabilities plus: full content access, exclusive drops, early access, event discounts |
| Comedian | Standard | Self-registration with comedian flag | Profile creation, content upload (Mux), analytics dashboard, go live (Daily.co), community access, event listings |
| Admin | Platform Admin | ComedyVault internal only | All platform access, Platform News posting, comedian verification, content moderation override, platform settings |

### 4.2 Fan Registration Flow

> **GHL Implementation Note:** Build fan registration as a GHL custom form connected to a GHL contact record. On submission, trigger the onboarding automation workflow. Age verification logic runs against the date of birth field before any content access is granted.

| Step | Field / Action | Validation Rule | System Response |
|------|---------------|-----------------|-----------------|
| 1 | Email address | Must be valid email format. Must not already exist in GHL contacts. | If duplicate: show error "An account with this email already exists." Do not create duplicate contact. |
| 2 | Password | Minimum 8 characters. At least 1 number. | Password strength indicator shown during entry. |
| 3 | Display name | Required. 3 to 30 characters. No special characters except underscore. | Name visible on fan's public activity (follows, attending events). |
| 4 | Date of birth | Required. Must be valid date. System calculates age. | If age under 18: Kids Mode set automatically. Mode toggle hidden. If 18+: Mode selection screen shown next. |
| 5 | Mode selection (18+ only) | Fan selects Kids Mode or Adult Mode. Required for 18+ accounts. | Selected mode saved to GHL contact custom field: ContentMode. Persists across sessions. |
| 6 | Terms of Service acceptance | Checkbox required. ToS link must be tappable. | Cannot proceed without checkbox checked. ToS acceptance timestamp logged. |
| 7 | Account created | All fields pass validation. | GHL contact record created. Welcome email triggered (WF-001). Fan routed to Discovery home screen. |

### 4.3 Comedian Registration Flow

> **Note:** Comedian registration is a separate flow from fan registration. The platform does not auto-verify comedian accounts at MVP. Any user who registers via the comedian path receives comedian access.

| Step | Field / Action | Validation Rule | System Response |
|------|---------------|-----------------|-----------------|
| 1 | Email address | Must be valid format. No duplicate contacts. | Duplicate check same as fan registration. |
| 2 | Password | Minimum 8 characters, 1 number. | Same as fan. |
| 3 | Stage name / display name | Required. Public comedian name on platform. | Saved as ComedianStageName in contact record. |
| 4 | City and state | Required. Used for discovery filter and event listings. | Saved as GHL standard location fields. |
| 5 | Primary comedy genre | Required. Select from: Stand-Up, Improv, Storytelling, Roast, Sketch, Other. | Saved as ComedianGenre custom field. |
| 6 | Short bio | Required. 10 to 280 characters. | Saved as ComedianBio custom field. Visible on public profile. |
| 7 | Date of birth | Required. Must be 18 or older. | Under-18 registration rejected with error message. |
| 8 | Stripe Connect Express onboarding | Required to activate earnings. Comedian prompted to connect Stripe Express account. | If skipped: account created but EarningsActive = No. Reminder automation WF-009 fires after 72 hours. |
| 9 | Terms of Service and Revenue Share Agreement | Required. Both must be accepted. | ToS and RSA acceptance timestamps logged. Cannot proceed without both. |
| 10 | Account created | All required fields pass. | GHL contact record created with AccountType = Comedian. Profile setup screen shown. WF-002 fires. |

---

## 5. Permission Matrix

Every access rule in the build is derived from this table. If a question arises about what a user can do, this table is the answer.

| Action / Capability | Fan (Free) | Fan (Premium) | Comedian | Admin |
|---------------------|-----------|--------------|---------|-------|
| Register and create an account | YES | YES | YES | NO — admin accounts created internally |
| Browse discovery page (ranked comedian list) | YES | YES | YES | YES |
| View comedian profile page | YES | YES | YES | YES |
| Watch content preview (first 2 minutes) | YES | YES | YES | YES |
| Watch full content (beyond 2 minutes) | NO | YES | YES — own content only | YES |
| Follow a comedian | YES | YES | NO — comedians do not follow fans | YES |
| Save a comedian to saved list | YES | YES | NO | YES |
| Share a comedian's profile link | YES | YES | YES | YES |
| Subscribe to premium fan tier ($6.99/month) | YES — can upgrade | N/A — already premium | NO | NO |
| Cancel premium subscription | N/A — free | YES | N/A | N/A |
| Browse and attend events | YES | YES | YES | YES |
| Add an event listing | NO | NO | YES | YES |
| Join a live Comedy Room as audience | YES | YES | YES — can join others' rooms | YES |
| Start a live Comedy Room | NO | NO | YES — own room only | YES |
| Upload video or audio content | NO | NO | YES | YES |
| View personal analytics dashboard | NO | NO | YES — own data only | YES — all data |
| Access comedian community boards | NO | NO | YES | YES |
| Post to comedian community boards | NO | NO | YES | YES |
| Post to Platform News board | NO | NO | NO | YES — admin only |
| Edit or delete own posts in community | NO | NO | YES — own posts only | YES — any post |
| Toggle content mode (Kids/Adult) — 18+ only | YES | YES | YES | YES |
| Access Stripe Express payout dashboard | NO | NO | YES — own payouts only | YES |
| Moderate or remove any content | NO | NO | NO | YES — admin only |
| Access GHL backend / admin panel | NO | NO | NO | YES — admin only |

---

## 6. Data Architecture and Custom Fields

### 6.1 Data Entity Overview

ComedyVault stores data across:
- **GHL:** User accounts, contact records, membership tiers, payments, automations (system of record)
- **Mux:** Video and audio files, play events, engagement data
- **Daily.co:** Live room sessions, recordings
- **Stripe:** Payment transactions, payouts
- **Base44:** Engagement rate calculations, discovery rankings

GHL is the system of record for all user identity, membership, and payment data. All other systems pull data into GHL via webhooks and API calls.

### 6.2 GHL Custom Fields — Required at Build

All 29 custom fields must be created in GHL before any automation workflows are built. All field names are case-sensitive. These are the canonical names for the build.

| Entity | Field Name | Type | Required | Purpose / Notes |
|--------|-----------|------|----------|-----------------|
| Contact (Fan/Comedian) | AccountType | Dropdown: Fan, Comedian, Admin | YES | Set at registration. Cannot be changed by user. |
| Contact (Fan) | MembershipTier | Dropdown: Free, Premium | YES | Updated by Stripe webhook on payment. Default: Free. |
| Contact (Fan) | ContentMode | Dropdown: Kids, Adult | YES | Set at registration. Modifiable by 18+ users only. |
| Contact (Fan) | DateOfBirth | Date | YES | Age verification for content mode assignment. |
| Contact (Fan) | SavedComedians | Multi-line text | NO | Comma-separated list of comedian GHL contact IDs saved by this fan. |
| Contact (Fan) | FollowedComedians | Multi-line text | NO | Comma-separated list of comedian contact IDs this fan follows. |
| Contact (Fan) | ToSAcceptedDate | Date/Time | YES | Timestamp of ToS acceptance at registration. |
| Contact (Fan) | LastViewedComedian | Text | YES | GHL contact ID of last comedian profile viewed. Used for subscription attribution. |
| Contact (Comedian) | ComedianStageName | Text | YES | Public display name on profile and discovery results. |
| Contact (Comedian) | ComedianGenre | Dropdown: Stand-Up, Improv, Storytelling, Roast, Sketch, Other | YES | Used for discovery filter. |
| Contact (Comedian) | ComedianBio | Text (280 char limit) | YES | Displayed on public profile page. |
| Contact (Comedian) | ComedianCity | Text | YES | Used for discovery city filter and event listings. |
| Contact (Comedian) | EarningsActive | Dropdown: Yes, No | YES | Set to Yes when Stripe Express onboarding is complete. |
| Contact (Comedian) | StripeConnectID | Text | NO (until Stripe setup) | Comedian's Stripe Connect Express account ID for payout routing. |
| Contact (Comedian) | TotalFollowers | Number | NO | Incremented by GHL workflow when a fan follows this comedian. |
| Contact (Comedian) | TotalSubscribers | Number | NO | Incremented by GHL workflow when a premium fan's subscription is attributed to this comedian. |
| Contact (Comedian) | MonthlyEarnings | Currency | NO | Calculated monthly from Stripe payout data. Displayed in analytics dashboard. |
| Contact (Comedian) | AllTimeEarnings | Currency | NO | Running total from all Stripe payouts. Displayed in analytics dashboard. |
| Contact (Comedian) | RSAAcceptedDate | Date/Time | YES | Revenue Share Agreement acceptance timestamp. |
| Contact (Comedian) | ProfileViews | Number | NO | Incremented by WF-017 on each profile page load. |
| Content (GHL Object) | ContentTitle | Text | YES | Display name of the uploaded video or audio piece. |
| Content (GHL Object) | ContentType | Dropdown: Video, Audio | YES | Determines player type on fan-facing content screen. |
| Content (GHL Object) | ContentRating | Dropdown: Clean, Adult | YES | Adult content hidden from Kids Mode users. |
| Content (GHL Object) | MuxAssetID | Text | YES | Mux asset identifier for playback. Set by Mux webhook after upload processing. |
| Content (GHL Object) | PreviewDurationSeconds | Number | YES | Default 120 (2 minutes). Defines preview cutoff for free-tier fans. |
| Content (GHL Object) | PlayCount | Number | NO | Incremented by Mux play event webhook. |
| Event (GHL Calendar) | EventFormat | Dropdown: In-Person, Online, Open Mic | YES | Used for event filter on fan events browse screen. |
| Event (GHL Calendar) | EventPerformers | Text | YES | Comma-separated comedian stage names. Displayed on event detail screen. |
| Room (GHL Custom Object) | RoomFormat | Dropdown: Solo Set, Open Mic | YES | Determines room type configuration in Daily.co. |
| Room (GHL Custom Object) | DailyCoRoomURL | Text | NO (until room starts) | Daily.co room URL. Generated when comedian starts the room. |
| Room (GHL Custom Object) | RoomRecordingURL | Text | NO (until room ends) | Daily.co recording URL. Set by webhook after room ends. |

---

## 7. Automation Workflow Library

Every GHL automation workflow required for the MVP is defined here. Each workflow has a unique ID (WF-###), a trigger event, a condition, a sequence of actions, the platform(s) involved, and a priority level. Build all workflows before any user-facing features go live.

> **HARD STOP:** No workflow is modified, disabled, or deleted during the build or post-launch without DDBA written authorization. Workflows are the operational backbone of the platform. An unauthorized workflow change can break subscription gating, comedian payouts, or user notifications.

| WF ID | Trigger | Condition | Action Sequence | Platform | Priority |
|-------|---------|-----------|-----------------|---------|---------|
| WF-001 | Fan completes registration | AccountType = Fan | 1. Send welcome email (EM-001). 2. Tag contact: NewFanOnboarded. 3. If ContentMode = Kids: tag KidsMode. If Adult: tag AdultMode. | GHL | CRITICAL |
| WF-002 | Comedian completes registration | AccountType = Comedian | 1. Send comedian welcome email (EM-002). 2. Tag: NewComedianOnboarded. 3. Route to profile setup screen. 4. If EarningsActive = No: start WF-009 timer. | GHL | CRITICAL |
| WF-003 | Fan upgrades to Premium (Stripe payment confirmed) | MembershipTier changes Free to Premium via Stripe webhook | 1. Update GHL membership to Premium immediately. 2. Remove paywall content restrictions. 3. Send premium confirmation email (EM-003). 4. Tag: PremiumFan. 5. Increment TotalSubscribers on attributed comedian record. | GHL + Stripe | CRITICAL |
| WF-004 | Fan subscription payment fails | Stripe webhook: payment_failed for Premium subscription | 1. Send payment failure email (EM-004) with retry link. 2. After 3 days no payment: downgrade MembershipTier to Free. 3. Send downgrade notification email (EM-005). 4. Tag: PaymentFailed. | GHL + Stripe | CRITICAL |
| WF-005 | Fan cancels Premium subscription | Stripe webhook: subscription cancelled | 1. Keep Premium access active until end of billing period. 2. On billing period end: downgrade MembershipTier to Free. 3. Send cancellation confirmation email (EM-006). 4. Tag: CancelledPremium. | GHL + Stripe | HIGH |
| WF-006 | Fan follows a comedian | Fan taps Follow on comedian profile | 1. Add comedian ID to fan's FollowedComedians field. 2. Increment comedian's TotalFollowers field by 1. 3. Send follow notification to comedian (EM-007). 4. Log follow event to Base44 via webhook. | GHL + Base44 | HIGH |
| WF-007 | Fan saves a comedian | Fan taps Save on comedian profile or card | 1. Add comedian ID to fan's SavedComedians field. 2. Log save event to Base44 via webhook. | GHL + Base44 | MEDIUM |
| WF-008 | Comedian uploads new content (Mux processing complete) | Mux webhook: asset.ready event received | 1. Update content record MuxAssetID field. 2. Set content status to Live. 3. Send upload complete notification to comedian (EM-008). 4. Log new content event to Base44 for discovery recalculation. 5. If comedian has followers: send new content notification to all followers (EM-009 or push notification). | GHL + Mux + Base44 | HIGH |
| WF-009 | Comedian has not completed Stripe Express onboarding after 72 hours | EarningsActive = No AND time since registration > 72 hours | 1. Send reminder email to comedian (EM-010): "Complete your payout setup to receive earnings." 2. Repeat at Day 7 if still not completed. 3. After Day 14 no action: tag ComedianPayoutBlocked. Flag for admin review. | GHL | HIGH |
| WF-010 | Comedian goes live (Daily.co room created) | Room record created with RoomFormat populated | 1. Generate Daily.co room URL via API. 2. Save URL to DailyCoRoomURL field. 3. Send live notification to all of comedian's followers (push notification or EM-011). 4. Log room start event to Base44. | GHL + Daily.co + Base44 | CRITICAL |
| WF-011 | Live room ends (Daily.co room closed) | Daily.co webhook: room ended event | 1. Retrieve recording URL from Daily.co API. 2. Save URL to RoomRecordingURL field. 3. Create replay content record linked to comedian profile. 4. Send recording available notification to comedian (EM-012). | GHL + Daily.co | HIGH |
| WF-012 | Monthly payout calculation | Scheduled trigger: 1st of every month at 8am EST | 1. Query Stripe for all subscription payments in prior month attributed to each comedian. 2. Calculate 80% share per comedian. 3. Trigger Stripe Connect Express payout to each comedian's connected account. 4. Update MonthlyEarnings and AllTimeEarnings fields. 5. Send payout confirmation email (EM-013). | GHL + Stripe Connect Express | CRITICAL |
| WF-013 | Fan plays content (Mux play event received) | Mux webhook: play event | 1. Increment PlayCount on content record. 2. Log play event to Base44. 3. If fan is Free tier and play duration exceeds PreviewDurationSeconds: trigger paywall overlay on frontend. | GHL + Mux + Base44 | HIGH |
| WF-014 | Event start reminder (1 hour before event) | GHL calendar trigger: 1 hour before EventStartTime for events with attending fans | 1. Query all fan contacts tagged as attending this event. 2. Send push notification or email (EM-014): "[Event Name] starts in 1 hour." | GHL | MEDIUM |
| WF-015 | New fan shares a comedian profile link | Fan taps Share and copies the UTM-tagged link | 1. Log share event to Base44 for engagement rate calculation. 2. Increment share count metric on comedian record. | GHL + Base44 | MEDIUM |
| WF-016 | Under-18 user attempts to change to Adult Mode | User taps Adult Mode toggle with DOB-confirmed age under 18 | 1. Block the toggle action. 2. Show age gate modal: "Adult Mode is not available for users under 18." 3. Log the attempt (admin visibility only). | GHL | CRITICAL |
| WF-017 | Comedian profile view logged | Any user views a comedian's profile page | 1. Log profile view event to Base44 for engagement rate denominator. 2. Increment ProfileViews counter on comedian record. | GHL + Base44 | HIGH |

---

## 8. Monetization Logic

### 8.1 Fan Subscription Logic

| Rule | Value |
|------|-------|
| Premium Price | $6.99 per month. No annual option at MVP launch. |
| Free Trial | No free trial at MVP launch. Fan enters payment immediately on Premium selection. |
| Content Access Change | Immediate on payment confirmation. GHL membership tier updates in real time via Stripe webhook WF-003. No manual processing. |
| Billing Cycle | Monthly, on the date of original subscription. |
| Failed Payment Response | Payment failure email sent immediately. 3-day grace period before downgrade. Stripe handles retry logic (3 attempts over 3 days). |
| Cancellation | Fan can cancel in Account Settings without contacting support. Access continues until end of current billing period. No partial refunds. |
| Refund Policy | No refunds for the current billing period. Refunds at platform discretion only, processed manually by admin through Stripe. |
| Comedian Attribution | When a fan subscribes to Premium, the premium subscription revenue is attributed to the last comedian whose profile the fan viewed before subscribing. |
| Attribution Logic | GHL tracks the LastViewedComedian custom field on the fan contact. When Premium purchase completes, this field value is used for payout attribution. |

### 8.2 Comedian Monetization Unlock

| Rule | Value |
|------|-------|
| Unlock Fee | $16.00 one-time. Processed through Stripe via a GHL order form. |
| What It Unlocks | Activates EarningsActive field. Enables: subscription earnings attribution, direct tips, access to earnings analytics panel. |
| Without Unlock | Comedian can still upload content, go live, and build a following. They cannot earn revenue from the platform. |
| Stripe Express Requirement | Comedian must complete Stripe Connect Express onboarding before any payout is processed, regardless of monetization unlock status. |

### 8.3 Revenue Share Payout Logic (80/20 Split)

| Rule | Value |
|------|-------|
| Split | 80% to comedian. 20% retained by ComedyVault platform. |
| Payout Frequency | Monthly. Paid on the 1st of each month for the prior month's subscription revenue. |
| Minimum Payout Threshold | $10.00. If comedian's earned amount is below $10 in a month, the balance rolls over to the next month. |
| Payout Method | Stripe Connect Express direct deposit to comedian's connected bank account. |
| Payout Calculation | Total Premium subscription revenue attributed to comedian in the month, multiplied by 0.80, minus Stripe processing fees (2.9% + $0.30 per transaction, deducted by Stripe before payout). |
| Platform 20% Use | Platform operations, hosting costs (GHL, Mux, Daily.co), DDBA ongoing support, future feature development. |
| Tax Responsibility | Comedian is responsible for their own tax obligations. ComedyVault provides a 1099 via Stripe for earnings exceeding $600 in a calendar year. |
| Dispute Resolution | Comedian disputes a payout by contacting ComedyVault admin via email within 14 days of payout date. Admin reviews Stripe records. Decision is final. |

> **CRITICAL:** Automated 80/20 revenue split payouts are NOT a native GHL feature. GHL collects subscription payments into the platform Stripe account. The payout distribution to comedian Express accounts requires a custom integration between GHL (via webhook) and Stripe Connect Express API. This integration must be built and tested before any comedian is promised automated payouts.

### 8.4 Direct Tips Logic

| Rule | Value |
|------|-------|
| Available To | Any fan (free or premium) can tip any comedian. |
| Tip Amounts | Preset options: $1, $3, $5, $10. Custom amount input available (minimum $1, maximum $100). |
| Processing | Stripe. Fan enters card details on tip checkout. Single transaction per tip. |
| Comedian Receives | 100% of tip minus Stripe processing fee (2.9% + $0.30). Tips are not subject to the 80/20 revenue share. |
| Tip Payouts | Processed to comedian's Stripe Connect Express account on the same monthly schedule as subscription payouts. |
| Availability Gate | Comedian must have EarningsActive = Yes and a connected Stripe Express account to receive tips. |

---

## 9. Content Rules and Moderation

### 9.1 Content Rating System

| Rating | Comedian Sets This For | Platform Behavior |
|--------|----------------------|-------------------|
| Clean | Content with no profanity, no adult themes, appropriate for all ages. | Visible to all users regardless of content mode. |
| Adult | Content containing profanity, adult humor, explicit language, or mature themes. | Hidden from all users in Kids Mode. Visible to users in Adult Mode only. |

### 9.2 Content Moderation Rules

| Rule | Definition |
|------|-----------|
| Comedian responsibility | Comedian is responsible for accurately rating their content. Misrating content is a Terms of Service violation. |
| Admin moderation | Admin can override any content rating. Admin can remove any content from the platform without notice if it violates the ToS. |
| Prohibited content | Illegal content, content depicting minors in sexual context, content that incites violence against specific individuals, defamatory content. These are prohibited regardless of rating. |
| Comedy content latitude | Profanity, adult humor, edgy political commentary, and mature themes are permitted in Adult Mode content. Do not over-moderate these content types. |
| Content reporting | Fans can report content using a Report button. Reports are logged to admin. Admin reviews within 48 hours. No automated takedowns at MVP. |
| App Store compliance | Adult content must be behind age verification AND behind the Adult Mode toggle. This two-gate system satisfies Apple and Google content policy requirements. |

---

## 10. Discovery Algorithm Specification

The discovery algorithm is the core product differentiator. Base44 implements the calculation. GHL implements the display layer and consumes the API response.

### 10.1 Engagement Rate Formula

```
Engagement Rate = (Plays + Follows + Saves + Shares) / Profile Views
```

**Time Windows:**
- Calculated over rolling 7-day window AND rolling 30-day window
- Discovery ranking uses the 7-day window
- All-time engagement is tracked but not used for ranking

**Example:**
- Comedian A: 20 profile views, 18 total engagements in 7 days. Engagement rate = 0.90
- Comedian B: 5,000 profile views, 100 total engagements. Engagement rate = 0.02
- Comedian A ranks higher.

**Minimum Threshold:** A comedian must have at least 5 profile views in the 7-day window to appear in ranked results. Comedians with fewer than 5 profile views are excluded from the ranked list but remain accessible via direct search or profile URL.

### 10.2 Events Sent from GHL to Base44 (Webhook Definitions)

| Event Name | Trigger Source | Data Payload Sent | Used In Algorithm As |
|-----------|---------------|-------------------|---------------------|
| profile_view | GHL — comedian profile page load | comedian_id, fan_id (or anonymous_id), timestamp | Profile view count (denominator) |
| content_play | Mux webhook — play event | comedian_id, content_id, fan_id, timestamp | Play count (numerator engagement) |
| follow | GHL workflow WF-006 | comedian_id, fan_id, timestamp | Follow count (numerator engagement) |
| save | GHL workflow WF-007 | comedian_id, fan_id, timestamp | Save count (numerator engagement) |
| share | GHL workflow WF-015 | comedian_id, fan_id (if logged in), timestamp | Share count (numerator engagement) |

### 10.3 Webhook Payload Definitions for Base44 Ingest

```json
// profile_view event
POST {BASE44_INGEST_URL}/events
{
  "event_type": "profile_view",
  "comedian_id": "{ghl_contact_id_of_comedian}",
  "fan_id": "{ghl_contact_id_of_fan_or_anonymous}",
  "timestamp": "{ISO8601_datetime}"
}

// content_play event (from Mux via GHL WF-013)
POST {BASE44_INGEST_URL}/events
{
  "event_type": "content_play",
  "comedian_id": "{comedian_id}",
  "content_id": "{mux_asset_id}",
  "fan_id": "{fan_contact_id_or_anonymous}",
  "timestamp": "{ISO8601_datetime}"
}

// follow event (from GHL WF-006)
POST {BASE44_INGEST_URL}/events
{
  "event_type": "follow",
  "comedian_id": "{comedian_id}",
  "fan_id": "{fan_contact_id}",
  "timestamp": "{ISO8601_datetime}"
}
// save and share events follow same pattern with event_type: 'save' or 'share'
```

### 10.4 API Contract: GHL Display Layer Calls Base44

| Field | Value |
|-------|-------|
| Endpoint (Base44 provides) | GET /api/discovery/ranked-comedians |
| Request Parameters | time_window (7d or 30d), genre (optional filter), city (optional filter), limit (default 50, max 200) |
| Response Format | JSON array of comedian objects sorted by engagement rate descending |
| GHL Consumes Response | GHL discovery browse page reads the ranked comedian list from Base44 API response and renders comedian cards in that order. GHL does not apply its own sort logic. |
| Refresh Frequency | Base44 recalculates rankings every 24 hours. Rankings are cached. API response is from cache, not calculated in real time per request. |
| Response Time SLA | Base44 must return the ranked list within 2 seconds for any request with up to 200 comedians. GHL discovery page load target is 3 seconds or under. |
| Error Handling | If Base44 API is unavailable or returns an error: GHL shows comedians in fallback order (most recently joined first). Error is logged. DDBA is notified. |

**Base44 API Response Structure:**

```json
{
  "ranked_at": "{ISO8601_datetime}",
  "comedians": [
    {
      "comedian_id": "{ghl_contact_id}",
      "rank": 1,
      "engagement_rate_7d": 0.87,
      "profile_views_7d": 45,
      "total_engagements_7d": 39,
      "genre": "stand-up",
      "city": "Los Angeles"
    }
  ]
}
```

---

## 11. MVP Feature Cards and Screen Inventory

### 11.1 MVP Feature Summary

ComedyVault MVP contains 10 features across 37 screens. Every feature is tied to a specific user problem. If the user problem disappears, the feature disappears with it. Features are not added after scope lock without DDBA written approval.

| Feature Group | MVP Count | Phase 2 | Phase 3 |
|---------------|-----------|---------|---------|
| Comedian Side Features | 5 features | 4 features | 3 features |
| Fan Side Features | 4 features | 3 features | 2 features |
| Platform Infrastructure | 1 feature (Discovery Engine) | 0 features | 1 feature |
| **TOTAL** | **10 features** | **7 features (deferred)** | **6 features (deferred)** |

### 11.2 Feature Cards

---

**Feature 1 of 10 — Comedian Profile Page**
Sprint 1 | Platform: GHL | User: COMEDIAN

**User Problem:** Comedians have no dedicated professional home online. Social profiles are generic creator pages. Comedy clubs only list comedians they have already booked. There is no platform that presents a comedian as a full entertainment brand with content, bio, shows, and fan engagement in one place.

**Delivers:** A dedicated comedian profile page containing: profile photo, bio, location, genre tags, upcoming show listings, content uploads (video and audio), follower count, subscription CTA, and direct tip button.

**Screens in this Feature:** Comedian profile page (public view), Comedian profile edit screen (logged in), Profile photo upload modal, Bio and genre tag editor, Show listing add/edit screen

**User Flow:**
1. Comedian signs up or logs in. System routes to profile setup flow (first login only).
2. Comedian uploads profile photo from device.
3. Comedian completes bio (280 char limit), selects genre tags (max 3 from predefined list), enters location (city/state).
4. Comedian saves profile. Profile page is now publicly visible at comedyvault.com/[username].
5. Fan visits comedian profile. Sees photo, bio, genre, location, content grid, follower count, Subscribe button, and Tip button.
6. Fan can tap Follow. Follow count increments on profile.

**Acceptance Criteria:**
- PASS: Comedian can upload a profile photo from device. Photo displays at correct aspect ratio on profile.
- PASS: Bio field accepts up to 280 characters. Character counter is visible during editing.
- PASS: Genre tag selector displays all available genres. Comedian can select a maximum of 3.
- PASS: Public profile URL is live and accessible without login.
- PASS: Subscribe button is visible on comedian profile to all visitors (free and premium).
- PASS: Follow button increments follower count in real time after tap.

---

**Feature 2 of 10 — Video and Audio Content Upload and Hosting**
Sprint 1 | Platform: GHL + Mux | User: COMEDIAN

**User Problem:** Comedians upload work to YouTube or TikTok and it gets suppressed or removed for content policy violations. There is no platform where a comedian can upload a full set and have it live permanently on their profile, accessible to paying fans.

**Delivers:** Comedians can upload video files (full sets, clips, specials) and audio files (stand-up recordings, audio-only sets) directly to their profile. Files are processed by Mux, hosted on CDN, and streamed to fans. Free-tier fans see a 2-minute preview. Premium fans see full content.

**Screens:** Content upload screen, Upload progress modal, Content settings screen (title, description, preview length, content rating), Content grid on comedian profile (public), Content player screen (fan-side), Content preview overlay for free tier fans

**User Flow:**
1. Comedian taps Upload in their dashboard.
2. Comedian selects video or audio file from device. GHL upload UI accepts the file.
3. GHL sends file to Mux API for processing. Upload progress bar shown.
4. Mux transcodes the file. Comedian sees Processing status. When complete, status changes to Live.
5. Comedian adds title, description, genre tag, and sets content rating (Clean or Adult).
6. Comedian sets preview length (default 2 minutes) for free-tier fans.
7. Content appears in content grid on comedian's profile.
8. Fan visits profile. Taps a video or audio item. Free tier: preview plays for 2 minutes then Subscribe CTA overlay appears. Premium: full content plays.

**Acceptance Criteria:**
- PASS: Comedian can upload video files. File is processed by Mux and plays back at correct quality.
- PASS: Comedian can upload audio files. File plays as audio with static thumbnail.
- PASS: Upload progress bar is visible during upload. Processing status updates when Mux completes transcoding.
- PASS: Content appears in comedian's content grid on their profile after upload.
- PASS: Free-tier fan sees 2-minute preview. Subscribe overlay appears at exactly 2 minutes.
- PASS: Premium fan sees full content without interruption.
- PASS: Adult-rated content is not visible to users in Kids Mode.

---

**Feature 3 of 10 — Free vs. Premium Fan Access Tiers**
Sprint 2 | Platform: GHL | User: FAN

**User Problem:** Comedy content online is either fully free (YouTube) or fully locked behind a studio paywall (Netflix). There is no middle ground where fans can sample a comedian for free, decide they love them, and then pay directly to support that comedian and get full access to their catalog.

**Delivers:** Two distinct access tiers: Free (browse, follow, 2-minute previews) and Premium ($6.99/month: full content access, exclusive drops, early access, event discounts). Tier is managed by GHL membership. Subscription processed through Stripe. Content gating fires immediately on subscription confirmation.

**Screens:** Fan registration screen, Tier selection screen, Subscription checkout screen (Stripe-powered), Premium confirmation screen, Content paywall overlay, Account settings screen (tier status, billing, cancellation)

**User Flow:**
1. Fan downloads app or visits web. Taps Sign Up.
2. Fan enters email, creates password, selects display name.
3. Fan selects Free or Premium at tier selection screen. Free proceeds to browse. Premium proceeds to Stripe checkout.
4. Fan enters payment details on Stripe checkout. Payment confirmed.
5. Fan sees Premium Confirmation screen. Access upgrades immediately.
6. Fan browses and plays full content on any comedian profile.
7. If free-tier fan hits the 2-minute paywall, the Subscribe to Premium overlay appears. Fan can tap Subscribe Now to go directly to Stripe checkout.
8. Fan can manage subscription, view billing date, and cancel anytime in Account Settings.

**Acceptance Criteria:**
- PASS: Free-tier fan can register, browse, follow comedians, and watch 2-minute previews without payment.
- PASS: Stripe checkout processes payment and returns success confirmation. GHL membership tier upgrades immediately on payment confirmation.
- PASS: Premium fan can access full content on any comedian's profile without interruption.
- PASS: Paywall overlay appears exactly at the 2-minute mark for all video and audio content for free-tier fans.
- PASS: Cancellation is accessible in Account Settings without contacting support. Cancellation takes effect at end of current billing period.
- PASS: Subscription billing recurs monthly. Failed payment triggers a retry notification to fan.

---

**Feature 4 of 10 — Comedian Analytics Dashboard**
Sprint 3 | Platform: GHL + Claude Code | User: COMEDIAN

**User Problem:** Comedians on other platforms have no direct visibility into who is watching, which content performs, how many fans have subscribed, or how much they have earned. Without data, comedians cannot make decisions about what content to make more of.

**Delivers:** A dedicated analytics dashboard visible only to the comedian. Shows: total followers, total subscribers, content play counts per video/audio, engagement rate per piece of content, total earnings this month and all-time, and a subscriber growth chart over the last 30 days.

**Screens:** Analytics dashboard screen (comedian account only), Individual content performance modal (tappable from dashboard), Earnings summary panel, Subscriber growth chart panel

**User Flow:**
1. Comedian logs in to their account.
2. Comedian taps Analytics in their dashboard navigation.
3. Analytics dashboard loads. Top row shows: Total Followers, Total Subscribers, This Month's Earnings, All-Time Earnings.
4. Content performance table shows each uploaded video and audio with: play count, completion rate, and engagement score.
5. Comedian taps any content row to see individual content performance detail.
6. Subscriber growth chart shows 30-day fan subscription trend.
7. Earnings panel shows breakdown: this month's subscription revenue attributed to this comedian's subscribers, pending payout date, and payout history.

**Acceptance Criteria:**
- PASS: Analytics dashboard loads within 3 seconds of navigation tap.
- PASS: Total Followers and Total Subscribers reflect accurate real-time counts.
- PASS: Play count on each content item increments with every play event from Mux analytics API.
- PASS: Earnings figures reflect the comedian's 80% share of subscription revenue attributed to their subscribers.
- PASS: Subscriber growth chart renders a 30-day line graph with no blank data states (shows zero if no activity).
- PASS: All dashboard data refreshes automatically on each load. No manual refresh required.

---

**Feature 5 of 10 — Discovery Engine**
Sprint 2 | Platform: Base44 | User: FAN

**User Problem:** Every existing platform surfaces comedians based on existing follower count or paid promotion. A comedian with 10 followers who posts a genuinely funny set has zero chance of being discovered. Fans who want to find new talent before anyone else has no tool that helps them do it.

**Delivers:** A discovery browse page that ranks comedians by engagement rate (total fan interactions divided by total profile views, calculated over a rolling 7-day window), not follower count. Ranking is recalculated every 24 hours. Browse is filterable by genre, city, and vibe tag.

**Screens:** Discovery browse page (main fan landing screen), Filter panel (genre, city, vibe, content type), Comedian card component (used in browse results), Trending Now section (top 5 by engagement rate in last 7 days)

**User Flow:**
1. Fan opens app. Discovery browse page is the default home screen.
2. Browse page loads. Shows Trending Now row (top 5 comedians by 7-day engagement rate). Below that: full ranked list of active comedians.
3. Fan can apply filters: genre, city, or vibe tag.
4. Each comedian card shows: profile photo, name, location, genre tags, follower count, and a short bio snippet.
5. Fan taps a comedian card to go to that comedian's profile.
6. Ranking refreshes every 24 hours.

**Acceptance Criteria:**
- PASS: Discovery page loads within 3 seconds.
- PASS: Comedian ranking order is determined by Base44 engagement rate API response, not GHL sort order.
- PASS: Filter panel applies filters and refreshes results without a full page reload.
- PASS: Trending Now row shows exactly 5 comedians. Updates every 24 hours.
- PASS: A comedian with zero content does not appear in discovery results.
- PASS: Results are not alphabetical or chronological by default. Engagement rate order is the only default sort.

---

**Feature 6 of 10 — Follow, Save, and Share Comedians**
Sprint 2 | Platform: GHL | User: FAN

**User Problem:** Fans can follow creators on other platforms but have no way to bookmark comedians they want to come back to later without following, and no way to share a comedian's profile in a way that tracks back to the platform for growth.

**Delivers:** Three distinct fan actions: Follow (subscribes fan to comedian updates in their feed), Save (bookmarks comedian to a private Saved list), Share (generates a shareable link with UTM tracking for acquisition attribution).

**Screens:** Comedian profile page (Follow, Save, Share buttons visible), Comedian card in browse (Follow, Save actions accessible), Fan saved list screen, Share modal, Fan activity feed

**User Flow:**
1. Fan is on comedian profile or discovery browse result.
2. Fan taps Follow. Follow button changes to Following. Comedian appears in fan's feed.
3. Fan taps Save. Comedian is added to fan's Saved list. Checkmark appears on Save button.
4. Fan taps Share. Share modal appears with a generated link that includes a UTM source parameter. Fan copies the link or shares to system share sheet.
5. Recipient of shared link opens ComedyVault to the comedian's profile. UTM parameter is logged as a share-sourced visit.
6. Fan navigates to their Saved tab in account. All saved comedians displayed. Fan can remove from saved list.

**Acceptance Criteria:**
- PASS: Follow button changes state immediately on tap. Comedian appears in fan's feed on next load.
- PASS: Save action updates Saved list immediately. Fan's saved list is accessible from their account menu.
- PASS: Share link is unique per comedian profile. UTM parameter is present in every generated share link.
- PASS: Fan feed shows content uploads from all followed comedians in reverse chronological order.
- PASS: Saved list persists across sessions.

---

**Feature 7 of 10 — Comedy Rooms (Live Performance Rooms)**
Sprint 3 | Platform: GHL + Daily.co | User: BOTH

**User Problem:** Underground comedians have no way to perform live for a digital audience without going through YouTube Live or Instagram Live, where their content competes with millions of other creators and there is no comedy-specific context.

**Delivers:** Live performance rooms where a comedian can go live in front of their fan audience. Fans join to watch. Comedian can see the fan count and audience reactions in real time. Each room has a title, genre, and start time. Followers receive a push notification when comedian goes live. Rooms can be solo (one comedian performing) or open mic format (multiple comedians taking turns). Recordings are saved automatically.

**Screens:** Go Live screen (comedian dashboard), Room setup screen (title, format, genre, start time), Live room screen — comedian view (live controls, audience count, end room button), Live room screen — fan view (player, reaction controls, viewer count), Room listing screen, Room replay screen

**User Flow:**
1. Comedian taps Go Live in their dashboard.
2. Comedian sets room title, selects format (Solo Set or Open Mic), selects genre, and optionally sets a future start time.
3. If starting now: room opens immediately. Comedian's followers receive a push notification.
4. Daily.co session initializes within the GHL room page. Comedian's camera and microphone are active.
5. Fans tap the notification or find the room in the Room Listing screen. Fan enters the room.
6. Fan sees comedian performing live. Reaction buttons (laugh, fire, clap) are visible. Reaction counts increment in real time.
7. Comedian ends the room. Recording is automatically saved by Daily.co. Recording appears on comedian's profile under Past Performances within 15 minutes.
8. Fans who were not in the live room can watch the recording on the comedian's profile.

**Acceptance Criteria:**
- PASS: Comedian can start a live room from their dashboard. Room is visible to followers in the Room Listing screen within 30 seconds.
- PASS: Push notification fires to all followers of the comedian when a room goes live.
- PASS: Daily.co session loads within the GHL page with functioning audio and video.
- PASS: Fan reaction buttons are visible and reaction counts increment in real time during the session.
- PASS: Room recording is automatically saved and appears on the comedian's profile within 15 minutes of room ending.
- PASS: Room Listing screen shows all currently live and upcoming rooms, sorted by start time.

---

**Feature 8 of 10 — Comedian Community and Collaboration Space**
Sprint 3 | Platform: GHL | User: COMEDIAN

**User Problem:** Comedians working independently have no community infrastructure to connect with other comedians, share material for feedback, find collaboration partners, or ask operational questions. Every other platform puts comedians in competition. ComedyVault puts them in community.

**Delivers:** A dedicated community space visible only to verified comedian accounts. Contains: a general discussion board, a collaboration board (comedians post partnership requests), a material workshop section (comedians share sets for peer feedback), and a platform news board (admin posts only).

**Screens:** Community home screen (comedian account only), General discussion board, Collaboration board (posts and replies), Material workshop board (posts and replies), Platform news board, New post composer screen

**User Flow:**
1. Comedian navigates to Community in their account menu.
2. Community home screen loads with four board tiles: General, Collaboration, Workshop, Platform News.
3. Comedian selects a board. Board shows posts in reverse chronological order.
4. Comedian can create a new post: tap New Post, select board, write post, tap Publish.
5. Other comedians can reply to posts. Reply thread is nested under the post.
6. Platform News board is write-restricted to ComedyVault admin accounts. All comedian accounts can read but not post.
7. Fan accounts cannot access or view the Comedian Community space.

**Acceptance Criteria:**
- PASS: Community space is not accessible to fan accounts. Only verified comedian accounts can view or post.
- PASS: All four boards are visible and functional on community home screen.
- PASS: Comedian can create a new post in General, Collaboration, or Workshop boards.
- PASS: Reply threads nest correctly under the parent post.
- PASS: Platform News board shows only admin posts. No comedian can post to Platform News.
- PASS: Posts sort by most recent by default.

---

**Feature 9 of 10 — Event Discovery (Local Shows and Open Mics)**
Sprint 3 | Platform: GHL | User: FAN

**User Problem:** Fans who want to attend live comedy shows near them have no single reliable source. Ticketmaster and Eventbrite list mainstream venues only. Comedy club websites are outdated. Open mics have no online presence.

**Delivers:** An event listings section where comedians can post upcoming shows, open mic sets, and online performances. Fans can browse events by city, date, and format. Each event listing includes: event name, date, time, location or link, comedian(s) performing, and a short description. Fans can mark attending and receive a reminder notification before the event.

**Screens:** Event listings browse screen (fan-facing), Event detail screen, Attending confirmation modal, Event add screen (comedian dashboard), Comedian's upcoming shows section on their profile

**User Flow:**
1. Fan navigates to Events in the main navigation.
2. Events browse screen loads with upcoming events sorted by date.
3. Fan can filter by city, date range, and format (In-Person, Online, Open Mic).
4. Fan taps an event card to see Event Detail: full description, location or link, performing comedians, and Attending button.
5. Fan taps Attending. Event is saved to fan's calendar in their account. Fan receives a reminder notification 1 hour before the event.
6. Comedian navigates to their dashboard and taps Add Event. Enters event details and publishes. Event appears in their profile's Upcoming Shows section and in the Events browse.

**Acceptance Criteria:**
- PASS: Events browse screen loads all upcoming events sorted by soonest date.
- PASS: City filter returns events where the location field matches the city text.
- PASS: Format filter shows only matching format events.
- PASS: Fan Attending action saves the event and triggers a reminder notification 1 hour before event start.
- PASS: Comedian can add an event from their dashboard. Event appears on their profile and in Events browse within 60 seconds of publishing.
- PASS: Past events do not appear in the default browse view.

---

**Feature 10 of 10 — Two-Tier Content Moderation (Kids Mode and Adult Mode)**
Sprint 4 | Platform: GHL | User: FAN

**User Problem:** Comedy content that includes profanity, adult themes, or edgy material is suppressed on mainstream social platforms. This forces comedians to self-censor. At the same time, parents with children using the platform need assurance that kids are not exposed to adult content. These two requirements need to coexist in the same app without one destroying the other.

**Delivers:** A user-selectable content mode set at account setup and changeable in settings. Kids Mode: all Adult-rated content is hidden. Adult Mode: all content is visible including Adult-rated content. Adult Mode requires age verification at account creation (date of birth required, users under 18 are automatically set to Kids Mode and cannot change to Adult Mode).

**Screens:** Account creation screen (age/DOB input), Mode selection screen (shown at first login for users 18+), Kids Mode indicator (visible in header when Kids Mode is active), Content settings screen (mode toggle for 18+ users), Age gate modal

**User Flow:**
1. New user creates an account. Date of birth is a required field.
2. If date of birth confirms user is under 18: Kids Mode is set automatically. Mode toggle is hidden in settings.
3. If date of birth confirms user is 18 or older: Mode selection screen appears. User selects Kids Mode or Adult Mode.
4. User's selected mode is saved to their account and persists across sessions.
5. In Kids Mode: all content tagged Adult is hidden from browse, feed, and comedian profiles.
6. In Adult Mode: all content is visible. No filtering is applied.
7. 18+ user can toggle between modes in Account Settings at any time.
8. Under-18 user cannot access Adult Mode toggle. The toggle is not visible in their settings.

**Acceptance Criteria:**
- PASS: Date of birth is required at account creation. Account cannot be created without it.
- PASS: Users with a date of birth resulting in age under 18 are permanently set to Kids Mode. Mode toggle is not visible in their settings.
- PASS: Adult-rated content is fully hidden in Kids Mode. It does not appear in browse, feed, or on comedian profile pages.
- PASS: Mode selection persists across login sessions.
- PASS: Adult Mode toggle in settings is only visible to confirmed 18+ accounts.

---

### 11.3 Screen Inventory

Every screen required to deliver the 10 MVP features. No screen is built that is not on this list. No screen on this list is skipped.

| ID | Screen Name | User Type | Feature | Platform | Notes |
|----|-------------|-----------|---------|---------|-------|
| S01 | App Launch / Splash Screen | BOTH | All | GHL | Brand screen. Loads 1.5 seconds then routes to login or home based on session status. |
| S02 | Fan Registration Screen | FAN | F3 Tiers | GHL | Email, password, display name, date of birth. ToS acceptance required. |
| S03 | Comedian Registration Screen | COMEDIAN | C1 Profile | GHL | Separate registration flow for comedians. Includes comedian verification step. |
| S04 | Login Screen | BOTH | All | GHL | Email + password. Forgot password link. Separate entry points for fan and comedian. |
| S05 | Fan Home / Discovery Browse | FAN | F1 Discovery | Base44 + GHL | Default landing screen for fans. Trending Now row + ranked comedian list. Filter panel. |
| S06 | Filter Panel | FAN | F1 Discovery | GHL | Slides in from right. Genre, city, vibe. Apply button. Reset button. |
| S07 | Comedian Profile Page | BOTH | C1 Profile | GHL | Public-facing. Photo, bio, genre, content grid, follow/save/share, subscribe CTA. |
| S08 | Comedian Profile Edit Screen | COMEDIAN | C1 Profile | GHL | Logged-in comedian view. Edit bio, photo, genre, location. Add show listings. |
| S09 | Content Upload Screen | COMEDIAN | C2 Content | GHL + Mux | File picker, upload progress, title/description fields, content rating selector. |
| S10 | Content Player Screen | BOTH | C2 Content | GHL + Mux | Full-screen video player. Audio player with static thumbnail. Quality controls. |
| S11 | Content Paywall Overlay | FAN | F3 Tiers | GHL | Appears at 2-minute mark for free fans. Subscribe CTA. Dismissible. |
| S12 | Tier Selection Screen | FAN | F3 Tiers | GHL | Shown at first login for all fans. Free vs. Premium comparison. CTA buttons. |
| S13 | Subscription Checkout Screen | FAN | F3 Tiers | GHL + Stripe | Stripe-powered. Card entry. Billing summary. Confirm button. |
| S14 | Premium Confirmation Screen | FAN | F3 Tiers | GHL | Success state after payment. Welcome to Premium message. Navigate to browse. |
| S15 | Fan Account Screen | FAN | F3 Tiers | GHL | Tier status, billing date, cancellation option, saved list link, settings. |
| S16 | Fan Saved List Screen | FAN | F6 Follow/Save | GHL | All saved comedians. Tap to visit profile. Remove from saved option. |
| S17 | Fan Activity Feed Screen | FAN | F6 Follow/Save | GHL | New content and events from followed comedians. Reverse chronological. |
| S18 | Share Modal | BOTH | F6 Follow/Save | GHL | Generated shareable link. Copy button. System share sheet trigger. |
| S19 | Comedian Analytics Dashboard | COMEDIAN | C3 Analytics | GHL + Claude Code | Followers, subscribers, earnings panels. Content performance table. Subscriber growth chart. |
| S20 | Content Performance Detail Modal | COMEDIAN | C3 Analytics | GHL + Claude Code | Tapped from S19. Individual content stats: plays, completion rate, engagement score. |
| S21 | Go Live Screen | COMEDIAN | C7 Rooms | GHL + Daily.co | Room setup: title, format, genre, start time. Start Now button. |
| S22 | Live Room — Comedian View | COMEDIAN | C7 Rooms | GHL + Daily.co | Camera/mic controls, audience count, end room button. |
| S23 | Live Room — Fan View | FAN | C7 Rooms | GHL + Daily.co | Player, reaction buttons (laugh/fire/clap), viewer count. |
| S24 | Room Listing Screen | BOTH | C7 Rooms | GHL | Currently live and upcoming rooms. Filter by genre. Sorted by start time. |
| S25 | Room Replay Screen | FAN | C7 Rooms | GHL + Daily.co | Recorded session playback. Accessible on comedian profile. |
| S26 | Comedian Community Home | COMEDIAN | C8 Community | GHL | Four board tiles: General, Collaboration, Workshop, Platform News. |
| S27 | Community Board Screen | COMEDIAN | C8 Community | GHL | Post list for each board. Reverse chronological. New Post button. |
| S28 | Community Post Detail Screen | COMEDIAN | C8 Community | GHL | Full post and reply thread. Reply composer. |
| S29 | New Post Composer Screen | COMEDIAN | C8 Community | GHL | Board selector, text input, post button. |
| S30 | Event Listings Browse Screen | FAN | F9 Events | GHL | Upcoming events sorted by date. Filter by city, date, format. |
| S31 | Event Detail Screen | FAN | F9 Events | GHL | Full event info. Performing comedians. Attending button. |
| S32 | Add Event Screen | COMEDIAN | F9 Events | GHL | Comedian dashboard. Event name, date, time, location/link, description. Publish. |
| S33 | Mode Selection Screen | FAN | F10 Moderation | GHL | First login for 18+. Kids Mode vs. Adult Mode selection. |
| S34 | Age Gate Modal | FAN | F10 Moderation | GHL | Shown if under-18 user attempts to toggle Adult Mode. |
| S35 | Content Settings Screen | FAN | F10 Moderation | GHL | Mode toggle (18+ only). Notification preferences. Account details. |
| S36 | Comedian Dashboard Home | COMEDIAN | All | GHL | Navigation hub for comedian account. Analytics, Upload, Go Live, Community, Events. |
| S37 | Push Notification System | BOTH | C7 Rooms, F9 Events | GHL | Notifications for: new live room (to followers), event reminder (1hr before). |

---

## 12. Sprint Build Timeline

The MVP build runs across 7 sprints from Week 4 through Week 14. Each sprint is approximately 1 to 2 weeks. DDBA reviews each sprint output against this document before Sprint n+1 begins. A sprint does not advance if DDBA review finds blocking deviations from spec.

> **Note:** Sprint start dates assume Gate 1 sign-off by Week 4 of the engagement. If Gate 1 is delayed, all sprint dates shift by the number of weeks of delay. Sprints do not compress to make up time.

| Sprint | Weeks | Focus | Features / Screens | Owner | Milestone / Gate |
|--------|-------|-------|-------------------|-------|-----------------|
| S1 | 4-5 | Foundation + Registration | Screens S01-S08, S33-S36. All 29 GHL custom fields created. GHL membership products. Stripe connected. Custom domain active. | GHL Build Team | All Sprint 1 screens APPROVED in Build Tracker |
| S2 | 5-6 | Content Upload + Mux | Screens S09-S11. Mux API integration live. Upload flow working. Webhooks configured. | GHL Build Team | Mux integration end-to-end APPROVED |
| S3 | 6-7 | Subscription + Stripe | Screens S12-S15. Stripe subscription checkout. WF-003, WF-004, WF-005 all live. | GHL Build Team | Subscription flow APPROVED. Stripe test transactions confirmed. |
| S4 | 7-9 | Discovery + Fan Actions | Screens S05-S06, S16-S18. Base44 webhooks live. Discovery page consuming Base44 API. WF-006, WF-007, WF-015, WF-017 live. | GHL Build Team | Base44 integration APPROVED. Fan actions confirmed. |
| S5 | 9-10 | Analytics + Live Rooms | Screens S19-S25. Claude Code analytics endpoint live. Daily.co rooms functional. WF-010, WF-011 live. | GHL Build Team + DDBA | Analytics and live rooms APPROVED. |
| S6 | 10-12 | Community + Events | Screens S26-S32, S37. Community boards live. Event listings live. WF-014 reminder notifications. | GHL Build Team | Community and events APPROVED. Notifications confirmed. |
| S7 | 12-14 | QA + App Store Prep | All 37 screens re-tested. All 17 workflows re-tested. Integration end-to-end. Beta prep. | GHL Build Team + DDBA | Gate 2 sign-off issued. App Store submission authorized. |

---

## 13. Integration Specifications

### 13.1 Mux Integration

GHL handles all UI: upload buttons, file picker, progress display, content player page, content grid on comedian profiles. Mux handles all media: file ingestion, transcoding, CDN storage, playback URLs, and analytics. No video or audio file is stored in GHL media library for public content.

> **Note:** GHL media library has a 500MB API upload limit per file and is not optimized for CDN video streaming. All comedian-uploaded content routes to Mux. The GHL media library is only used for platform UI assets (logos, icons, background images), not user-generated content.

**Mux Upload Flow:**

```
// Step 1: Comedian selects file in GHL upload UI
// Step 2: GHL triggers Mux Direct Upload API to get an upload URL
POST https://api.mux.com/video/v1/uploads
Authorization: Basic {MUX_TOKEN_ID:MUX_TOKEN_SECRET}
Content-Type: application/json
{
  "new_asset_settings": {
    "playback_policy": ["signed"],
    "max_resolution_tier": "1080p"
  },
  "cors_origin": "https://comedyvault.com"
}
// Response: { "data": { "id": "upload_id", "url": "upload_url" } }
// Step 3: GHL frontend PUT the file to the upload_url (direct from browser)
// Step 4: Mux processes file. Sends webhook on completion.
// Step 5: GHL receives Mux webhook and updates content record.
```

**Mux Webhook Events:**

| Mux Webhook Event | When It Fires | GHL Action on Receipt | Automation Triggered |
|-------------------|--------------|----------------------|---------------------|
| video.asset.ready | Mux finishes transcoding. Content is ready to play. | Update content record: set MuxAssetID to asset.id field. Set content status to Live. | WF-008: Notify comedian. Notify followers. |
| video.asset.errored | Mux processing fails. | Update content record: set status to Failed. | WF-008 error branch: email comedian upload failure notice (EM-015). |
| video.asset.deleted | Asset deleted from Mux. | Update content record: set status to Removed. | None — administrative event. |
| video.asset.track.ready | A content play event occurs. | Increment PlayCount on content record. Log play event to Base44. | WF-013: Paywall check for free-tier users. |

**Mux Playback Configuration:**

| Setting | Value |
|---------|-------|
| Playback Policy | Signed. All content requires a signed playback token. Prevents hotlinking. |
| Signed Token Generation | GHL backend (or Claude Code layer) generates a signed JWT token using Mux signing key. |
| Token Expiry | 24 hours. A new token is generated each time a fan loads a content page. |
| Max Resolution | 1080p. Mux serves adaptive bitrate based on viewer connection speed. |
| Audio Playback | Mux supports audio-only assets. Audio files served with a static thumbnail image (comedian profile photo used as thumbnail). |
| Player Type | Mux Player (open source). Embedded as iframe or JS player within GHL content page template. |
| Thumbnail Generation | Mux auto-generates video thumbnails at the 5-second mark. Comedian can set a custom thumbnail by uploading an image separately. |

### 13.2 Daily.co Integration

Daily.co provides the real-time video and audio infrastructure for Comedy Rooms. GHL handles room creation, scheduling, notifications, and post-room recording display. Daily.co handles the live session itself.

**Room Creation Flow:**

```
// Triggered when comedian starts or schedules a room in GHL UI
POST https://api.daily.co/v1/rooms
Authorization: Bearer {DAILY_API_KEY}
Content-Type: application/json
{
  "name": "comedyvault-{comedian_id}-{timestamp}",
  "properties": {
    "exp": {unix_timestamp_4hrs_from_now},
    "max_participants": 1000,
    "enable_recording": "cloud",
    "owner_only_broadcast": true,
    "enable_chat": false
  }
}
// Response: { "url": "https://comedyvault.daily.co/{room_name}" }
// Save url to GHL Room record DailyCoRoomURL field via WF-010
```

**Room Configuration Rules:**

| Setting | Value |
|---------|-------|
| owner_only_broadcast | Set to true for Solo Set format. Audience is audio/video muted. |
| enable_recording | Set to cloud. Daily.co records the full session automatically. |
| enable_chat | Set to false. Chat is not a feature at MVP. |
| max_participants | Set to 1,000 for MVP. |
| Room Expiry | Set to 4 hours from creation. Prevents orphaned rooms. |
| Open Mic Format | For Open Mic rooms: owner_only_broadcast is set to false to allow multiple comedians to take turns. Maximum active speakers: 2 at a time. |

**Post-Room Recording Retrieval:**

```
// Daily.co sends webhook when recording is ready
// recording.ready event payload:
{
  "action": "recording-ready",
  "roomName": "comedyvault-{comedian_id}-{timestamp}",
  "recordingId": "{recording_id}",
  "duration": {duration_seconds},
  "s3Key": "{s3_path_to_recording}"
}

// GHL receives webhook via WF-011:
// 1. Call Daily.co GET /v1/recordings/{recordingId} to get playback URL
// 2. Save playback URL to Room record RoomRecordingURL field
// 3. Create replay content record linked to comedian profile
// 4. Send EM-012 notification to comedian
```

### 13.3 Stripe Integration

ComedyVault uses two distinct Stripe components: the Platform Account (where all fan payments are collected) and Stripe Connect Express (which routes 80% of subscription revenue to individual comedian connected accounts).

**Stripe Platform Account Setup:**

| Item | Value |
|------|-------|
| Account Type | Standard Stripe account. Connected to GHL via GHL Stripe Integration OAuth. |
| Account Owner | Abe Adewale / ComedyVault LLC. Business bank account required. |
| Connect to GHL | Settings > Payments > Stripe > Connect via OAuth in GHL sub-account. |
| Webhook Endpoint | Configure in Stripe Dashboard. Events to listen for: payment_intent.succeeded, invoice.payment_failed, customer.subscription.deleted, customer.subscription.updated. |
| GHL Stripe Products | Two products: (1) ComedyVault Premium — $6.99/month recurring. (2) Comedian Monetization Unlock — $16.00 one-time. |
| Stripe Connect Settings | Enable Connect in Stripe Dashboard (Connect > Settings). Set platform profile to Marketplace. Required before any comedian can create an Express account. |

**Comedian Stripe Express Onboarding Flow:**

```
// Step 1: Comedian taps 'Set Up Payouts' in their account
// Step 2: GHL/Claude Code calls Stripe Connect onboarding link API
POST https://api.stripe.com/v1/accounts
Authorization: Bearer {STRIPE_SECRET_KEY}
{
  "type": "express",
  "country": "US",
  "email": "{comedian_email}",
  "capabilities": {
    "transfers": { "requested": true }
  }
}
// Response: { "id": "acct_XXXXXXXXXX" }  <-- Save to GHL StripeConnectID field

// Step 3: Generate onboarding link for comedian to complete KYC
POST https://api.stripe.com/v1/account_links
{
  "account": "acct_XXXXXXXXXX",
  "refresh_url": "https://comedyvault.com/payout-setup",
  "return_url": "https://comedyvault.com/payout-setup/complete",
  "type": "account_onboarding"
}
// Redirect comedian to the returned URL to complete Stripe KYC
// On return: update EarningsActive = Yes in GHL contact record
```

**Monthly Payout Flow (WF-012 — Runs 1st of Each Month):**

```
// Step 1: Query Stripe for all Premium subscription charges in prior month
GET https://api.stripe.com/v1/charges
  ?created[gte]={first_day_prior_month_unix}
  &created[lte]={last_day_prior_month_unix}
  &limit=100

// Step 2: For each charge, read metadata.attributed_comedian_id
// (Set by GHL when fan subscribes — see WF-003)

// Step 3: Sum total charges per comedian_id
// comedian_revenue = sum(charge.amount_captured - stripe_fees) * 0.80

// Step 4: Create transfer to comedian's Express account
POST https://api.stripe.com/v1/transfers
{
  "amount": {comedian_revenue_in_cents},
  "currency": "usd",
  "destination": "{comedian_StripeConnectID}"
}

// Step 5: Update GHL MonthlyEarnings and AllTimeEarnings fields
// Step 6: Send EM-013 payout confirmation email via GHL WF-012
```

---

## 14. Subscription Gating Architecture

Subscription gating is the technical mechanism that enforces the Free vs. Premium content access tiers. A broken gate means free users access premium content or premium users are blocked. Both are revenue failures.

### 14.1 Gating Decision Matrix

| Scenario | User State | Content Rating | GHL Logic Check | Result |
|----------|-----------|----------------|-----------------|--------|
| Fan accesses 2-min preview | Free tier, any content mode | Clean or Adult | MembershipTier = Free AND play_duration < PreviewDurationSeconds | ALLOW — preview plays |
| Fan hits preview limit | Free tier, any content mode | Clean or Adult | MembershipTier = Free AND play_duration >= PreviewDurationSeconds | BLOCK — paywall overlay. Content pauses. Subscribe CTA shown. |
| Fan accesses full content | Premium tier | Clean | MembershipTier = Premium | ALLOW — full content plays |
| Fan accesses full content | Premium tier, Adult Mode | Adult | MembershipTier = Premium AND ContentMode = Adult | ALLOW — full content plays |
| Fan in Kids Mode views Adult content | Any tier, Kids Mode | Adult | ContentMode = Kids | BLOCK — content hidden entirely. Not shown on profile or browse. |
| Under-18 user | Any tier, forced Kids Mode | Adult | DateOfBirth confirms age < 18 | BLOCK — content hidden. Mode toggle not available. |
| Not logged in user | Anonymous | Clean | No session / no MembershipTier | ALLOW 2-minute preview only. Register/Login CTA shown after preview. |
| Not logged in user | Anonymous | Adult | No session, ContentMode unknown | ALLOW no Adult content. Adult content is hidden for anonymous users. |
| Comedian viewing own content | Comedian account | Clean or Adult | AccountType = Comedian | ALLOW — full access to own content regardless of tier. |
| Admin viewing any content | Admin account | Clean or Adult | AccountType = Admin | ALLOW — full access to all content. |

### 14.2 GHL Content Gating Implementation

Build a GHL page template for the content player. The template reads the logged-in contact's MembershipTier and ContentMode fields. If MembershipTier = Free, the player wrapper includes a JavaScript timer that fires a GHL overlay element at the PreviewDurationSeconds value (default 120 seconds). The overlay is a GHL element block containing the Subscribe CTA. If MembershipTier = Premium, the timer and overlay are not rendered in the page HTML. This prevents any client-side workaround.

---

## 15. Two-Tier Content Moderation Architecture

ComedyVault's content moderation system uses two gates to enforce age-appropriate content access. Gate 1 is age verification at registration. Gate 2 is the explicit content mode toggle. Both gates must be bypassed for a user to access Adult content. This two-gate system is the App Store compliance architecture.

### 15.1 Gate 1 — Age Verification

| Field | Value |
|-------|-------|
| Where It Happens | Fan and Comedian registration forms. Date of birth is a required field. |
| Calculation | GHL calculates age from DateOfBirth field using a workflow date formula. Age is compared to 18. |
| If Age < 18 | ContentMode is set to Kids automatically. The mode toggle control is not rendered in the user's account settings. WF-016 blocks any attempt to access Adult Mode. |
| If Age >= 18 | Mode selection screen is shown at first login. User selects Kids Mode or Adult Mode. Selection is saved to ContentMode field. |
| Date of Birth Storage | DateOfBirth is stored as a GHL standard date field on the contact record. It is never displayed publicly. It is only used for age verification logic. |
| Comedian Age Requirement | Comedian registration requires age 18 or older. Under-18 comedian registration is rejected at the form level with an error message. |

### 15.2 Gate 2 — Content Mode Toggle

| Field | Value |
|-------|-------|
| Who Can Toggle | Users with DateOfBirth-confirmed age of 18 or older only. Toggle is not visible in account settings for under-18 users. |
| Default Mode | Kids Mode for all users until they explicitly select Adult Mode. |
| Mode Selection Timing | First login for 18+ users. Can be changed at any time in Account Settings. |
| Mode Persistence | ContentMode field on contact record. Persists across all sessions and devices. |
| Kids Mode Behavior | All content with ContentRating = Adult is hidden from: discovery browse, comedian profiles, fan activity feed, search results. The content exists in the system but is not rendered for this user. |
| Adult Mode Behavior | All content is visible. ContentRating field has no filtering effect. |
| Mode Indicator | A visual indicator (small badge or label) is shown in the app header when Kids Mode is active so the user knows which mode they are in. |

### 15.3 Content Rating Assignment Rules

| Rule | Value |
|------|-------|
| Who Sets the Rating | Comedian. At content upload, ContentRating field is a required selector: Clean or Adult. |
| Clean Rating | Content with no profanity, no adult themes, no explicit sexual references. Appropriate for all ages. |
| Adult Rating | Content with profanity, adult humor, explicit language, mature themes. Edgy comedy, dark humor, and strong language qualify for Adult. |
| Misrating Consequence | Misrating Adult content as Clean is a Terms of Service violation. Admin can override any content rating and flag the comedian account. |
| App Store Compliance Note | Apple App Store requires: (1) Age rating declared for the app as 17+. (2) Adult content is behind a logged-in age-verified account gate. (3) Adult content is behind an explicit opt-in toggle. ComedyVault's two-gate system satisfies all three requirements. |
| Admin Override | Admin accounts can change any content's ContentRating field from the GHL backend. This is the moderation mechanism for misrated content. |

---

## 16. Analytics Dashboard Technical Spec

The Comedian Analytics Dashboard is built by Claude Code as a custom data layer on top of GHL. It aggregates data from three sources: GHL (subscriber and follower counts, earnings), Mux (play counts, completion rates), and Stripe (payout history).

### 16.1 Data Sources for Comedian Analytics Dashboard

| Metric | Source | How It Is Calculated | Display Format |
|--------|--------|----------------------|----------------|
| Total Followers | GHL — TotalFollowers custom field | Incremented by WF-006 on each follow event. Read directly from contact record. | Number with comma formatting (e.g., 1,247) |
| Total Subscribers | GHL — TotalSubscribers custom field | Incremented by WF-003 on each Premium subscription attribution. | Number with comma formatting |
| Content Play Count (per content item) | Mux analytics API + GHL PlayCount field | GHL PlayCount field updated by WF-013. Can also be pulled directly from Mux analytics API per asset. | Number per content row in content table |
| Content Completion Rate | Mux analytics API | Mux provides watch_time_minutes and duration_minutes per asset. Completion rate = watch_time / duration. | Percentage (e.g., 74%) |
| Engagement Rate (per content item) | Calculated by Claude Code | (plays + follows + saves triggered by this content) / profile_views_in_same_window. Pulled from Base44 API and GHL. | Percentage or score (e.g., 0.87) |
| Monthly Earnings | GHL — MonthlyEarnings field + Stripe | GHL MonthlyEarnings field updated by WF-012 after monthly payout. | Currency (e.g., $142.80) |
| All-Time Earnings | GHL — AllTimeEarnings field | Running total updated by WF-012. Read from GHL contact record. | Currency |
| Subscriber Growth (30-day chart) | Stripe + GHL | WF-012 logs payout events with dates. Claude Code queries historical payout records to build a 30-day subscriber trend. | Line chart — 30 data points |
| Payout History | Stripe + GHL | Query Stripe transfer records filtered by comedian's StripeConnectID. Return last 12 payouts with dates and amounts. | Table with date, amount, status |

### 16.2 Claude Code Dashboard Implementation

```
// Analytics Dashboard Flow
// Called when comedian navigates to Analytics screen

// 1. GHL page makes API call to Claude Code analytics endpoint
GET /api/analytics/comedian/{comedian_ghl_contact_id}
Authorization: Bearer {session_token}

// 2. Claude Code aggregates data:
//    a. Fetch TotalFollowers, TotalSubscribers, MonthlyEarnings, AllTimeEarnings from GHL contact
//    b. Fetch all content records for this comedian from GHL
//    c. For each content record: call Mux analytics API for play_count, completion_rate
//    d. Fetch comedian's engagement score from Base44
//    e. Fetch payout history from Stripe using StripeConnectID

// 3. Response structure:
{
  "summary": {
    "total_followers": 1247,
    "total_subscribers": 89,
    "monthly_earnings_cents": 14280,
    "all_time_earnings_cents": 87450
  },
  "content": [
    {
      "content_id": "{ghl_content_id}",
      "title": "Set title",
      "play_count": 340,
      "completion_rate": 0.74,
      "engagement_score": 0.87
    }
  ],
  "subscriber_growth_30d": [/* 30 daily datapoints */],
  "payout_history": [/* last 12 payouts */]
}
```

---

## 17. Mobile App and App Store Configuration

### 17.1 GHL White-Label Mobile App Setup

| Setting | Value |
|---------|-------|
| Navigation Path in GHL | Agency Account > Mobile App > White-Label > Self-Service Customizer |
| App Name | ComedyVault (or confirmed final brand name) |
| App Icon | 1024x1024px PNG. No transparency. No rounded corners (Apple rounds them). ComedyVault brand icon. |
| Splash Screen | Full-bleed brand image. Dimensions: 2732x2732px (covers all iOS device sizes). |
| Brand Colors | Primary: #CC0000 (red). Secondary: #1A1A1A (black). Both configured in GHL white-label customizer. |
| App Description (App Store) | A maximum-impact 170-character description for App Store search visibility. To be written in DDBA Phase 3 marketing copy session. |
| Age Rating (Apple) | 17+ — required for Adult Mode content. Set in App Store Connect during submission. |
| Content Advisory | Profanity or Crude Humor: Frequent. Sexual Content: None. Required for Adult Mode comedy content. |
| Android Content Rating | High Maturity (Google Play content rating questionnaire). Applies to: crude language, sexual references in comedy context. |
| In-App Purchases (IAP) Registration | Premium subscription ($6.99/month) must be registered as an IAP in both App Store Connect and Google Play Console before app submission. |

### 17.2 App Store Submission Checklist

| Status | Checklist Item | Platform |
|--------|---------------|---------|
| Pending | Apple Developer Program account created under ComedyVault LLC | iOS |
| Pending | App registered in App Store Connect with Bundle ID: com.comedyvault.app | iOS |
| Pending | In-App Purchase products registered: Premium $6.99/month | iOS |
| Pending | App icon uploaded: 1024x1024px PNG | iOS |
| Pending | App screenshots prepared: 6.5-inch (iPhone 14 Pro Max) minimum. 5 screenshots. | iOS |
| Pending | App Store description written and optimized for keywords | iOS |
| Pending | Privacy policy URL live on ComedyVault.com | iOS |
| Pending | Terms of Service URL live on ComedyVault.com | iOS |
| Pending | Age rating set to 17+ | iOS |
| Pending | GHL white-label app build submitted to App Store | iOS |
| Pending | Google Play Console account created under ComedyVault entity | Android |
| Pending | App registered in Google Play Console with package name: com.comedyvault.app | Android |
| Pending | In-App Purchase products registered: Premium $6.99/month | Android |
| Pending | Target API level set to Android 14 (API 34) or current requirement | Android |
| Pending | Content rating questionnaire completed — High Maturity | Android |
| Pending | Data safety form completed — lists: email, DOB, payment info, user content | Android |
| Pending | Store listing: 512x512 app icon, feature graphic (1024x500), screenshots (3 minimum) | Android |
| Pending | GHL white-label app build submitted to Google Play | Android |

---

## 18. Notification System

Every notification in ComedyVault is defined here. Notification triggers are governed by GHL automation workflows.

| WF | Notification Name | Trigger | Recipient | Delivery Method |
|----|-------------------|---------|-----------|-----------------|
| WF-001 | Welcome — Fan | Fan registration complete | Fan | Email (EM-001) |
| WF-002 | Welcome — Comedian | Comedian registration complete | Comedian | Email (EM-002) |
| WF-003 | Premium Upgrade Confirmation | Stripe payment confirmed for Premium | Fan | Email (EM-003) |
| WF-004 | Payment Failed | Stripe payment failure | Fan | Email (EM-004) |
| WF-005 | Subscription Cancelled | Fan cancels subscription | Fan | Email (EM-006) |
| WF-006 | New Follower | Fan follows comedian | Comedian | Email (EM-007) or in-app |
| WF-008 | New Content Live | Comedian uploads content (Mux processing complete) | Comedian's followers | Push notification or email (EM-009) |
| WF-009 | Complete Payout Setup | Comedian EarningsActive = No for 72 hours | Comedian | Email (EM-010) |
| WF-010 | Comedian Going Live | Daily.co room created and started | Comedian's followers | Push notification (EM-011) |
| WF-011 | Recording Available | Daily.co room ends, recording saved | Comedian | Email (EM-012) |
| WF-012 | Monthly Payout Sent | 1st of month payout processed | Comedian | Email (EM-013) |
| WF-014 | Event Reminder | 1 hour before event start | Fans marked attending | Push notification or email (EM-014) |

---

## 19. Edge Cases and Error Handling

Every failure state that is predictable must have a defined response. These are the known edge cases for the MVP. The GHL build team implements these responses before the Sprint 7 QA review.

| Edge Case | User-Facing Response | System Response |
|-----------|---------------------|-----------------|
| Fan attempts to access full content without Premium subscription | Paywall overlay appears at exactly 2-minute mark. "Subscribe to Premium to continue" with Subscribe Now button. Content pauses. Content does not continue on overlay dismiss. | GHL content gating logic checks MembershipTier field before serving full content. Free tier sees paywall. No content is accessible beyond the preview without Premium status confirmed. |
| Comedian uploads a video file and Mux processing fails | Content record is created with status Processing Failed. Comedian sees error on their dashboard: "Your upload failed to process. Try uploading again or contact support." Content is not visible to fans. | GHL workflow listens for Mux asset.errored webhook. On receipt: update content record status to Failed. Send email notification to comedian (EM-015). Do not display failed content on profile. |
| Stripe payment processing fails during fan Premium upgrade | Checkout screen shows error: "Payment could not be processed. Please check your card details and try again." Fan remains on Free tier. No charge is applied. | Stripe returns failure to GHL. WF-003 does not fire. Fan MembershipTier remains Free. No contact tag changes. |
| Fan under 18 attempts to toggle to Adult Mode | Age gate modal appears: "Adult Mode is not available for accounts under 18." Toggle returns to Kids Mode position. No change is made to account. | WF-016 fires. Attempt is logged to admin. ContentMode field is not changed. |
| Comedian goes live but Daily.co fails to initialize | Comedian sees error: "Live room could not start. Please try again. If the issue persists, contact support." Room record is not created. | GHL workflow catches Daily.co API error. WF-010 is aborted. No follower notifications are sent. Comedian is not charged. |
| Base44 discovery API is unavailable when fan loads browse page | Browse page loads with fallback sort (most recently joined comedians first). No error message shown to fan. Fan experience continues normally. | GHL discovery page detects Base44 API failure (timeout or error response). Falls back to GHL-native sort by join date. Logs error. DDBA notified via email. |
| Comedian's Stripe Express payout fails on the 1st | Comedian receives email (EM-016): "Your payout could not be processed this month. Please check your Stripe account for any issues." Balance rolls over to next month. | WF-012 catches Stripe payout failure. MonthlyEarnings is not zeroed out. Balance carries forward. Admin is notified. Comedian is emailed. |
| Fan account is created with email that already exists | Registration form shows inline error: "An account with this email already exists. Sign in or use a different email." | GHL form checks for existing contact with matching email before creating a new record. Duplicate contact is not created. |
| App loads with no network connection | Splash screen shows: "No internet connection. Please check your connection and try again." Retry button. App does not load or crash. | GHL white-label app handles network error at app shell level. No GHL API calls are attempted without confirmed connection. |

---

## 20. Security and Compliance Requirements

Security requirements are non-negotiable build requirements. They are not post-launch items. Every item below must be confirmed before the GHL build team delivers Sprint 7 QA sign-off.

| Area | Requirement | Implementation | Priority |
|------|-------------|----------------|---------|
| Authentication | Password hashing | GHL handles password hashing natively. No plaintext passwords stored. | CRITICAL |
| Authentication | Session token management | GHL manages user session tokens. Tokens expire after 24 hours of inactivity — confirm with GHL build team. | HIGH |
| Authentication | HTTPS enforcement | All ComedyVault web traffic must use HTTPS. GHL provides SSL certificate for custom domains. | CRITICAL |
| Payment Data | PCI compliance | ComedyVault never stores payment card data. All payment data is handled exclusively by Stripe. | CRITICAL |
| Payment Data | Stripe webhooks signature verification | All incoming Stripe webhook events must be verified using Stripe-Signature header and webhook signing secret. Reject any webhook that fails signature verification. | CRITICAL |
| Content Access | Mux signed tokens | All Mux playback URLs use signed tokens with 24-hour expiry. Direct Mux asset URLs are never exposed to the browser. | HIGH |
| Content Access | Age-gated content server-side check | Adult content visibility is enforced server-side via GHL contact field check on page render. Client-side JavaScript alone is not a sufficient gate. | CRITICAL |
| API Security | API key storage | Mux, Daily.co, Stripe, and Base44 API keys are stored as GHL secret environment variables or Claude Code environment variables. Never hardcoded in page templates or client-side code. | CRITICAL |
| API Security | Rate limiting on webhook endpoints | GHL webhook receivers should be configured to reject requests that do not include expected authentication headers from Mux, Daily.co, and Stripe. | HIGH |
| Privacy | Data minimization | ComedyVault collects only the data required for platform function: email, display name, date of birth, location (city/state), payment data (Stripe only), content uploads. | HIGH |
| Privacy | GDPR and CCPA readiness | Privacy policy must include: data collected, how it is used, user rights (deletion, export), and contact for privacy requests. California-based platform requires CCPA compliance. | HIGH |
| Privacy | Date of birth handling | DateOfBirth field is used for age verification only. It is stored in GHL but never displayed publicly. It must not appear in any exported data report accessible to non-admin users. | CRITICAL |
| Content Security | Terms of Service for user-generated content | Comedian uploads are user-generated content. Terms of Service must include a section on content ownership, platform license, prohibited content, and comedian responsibility for content ratings. | HIGH |
| Moderation | Admin-only content override | Content moderation actions (rating override, content removal, account suspension) are available only to Admin account type. All reports go to admin review. | HIGH |

---

## 21. Deferred Feature Log

Every feature that is NOT in the MVP is logged here. Deferred means it is planned, acknowledged, and parked for a future sprint.

> **HARD STOP:** If the GHL build team receives a request from any party to build a feature from this deferred log during the MVP build phase, stop. Contact DDBA. Do not build deferred features during the MVP sprint cycle without written DDBA authorization.

| Deferred Feature | Phase | Why Deferred | GHL Likely | Future Audit Notes |
|-----------------|-------|--------------|------------|-------------------|
| Fan Subscription Booking / Inquiry System | Phase 2 | MVP establishes the fan relationship. Booking is a conversion layer added after fan base is established. | YES | GHL calendar natively handles this. Low complexity. Target: Sprint 8. |
| Weekly Comedian Spotlight Feature | Phase 2 | Requires curation workflow and editorial process. Cannot be automated at launch without comedian content quality baseline. | YES | GHL email/SMS broadcast delivers this. Add after 30-day content quality baseline is established. |
| Brand Sponsor Portal | Phase 2 | Requires a comedian audience size floor before sponsors will pay. Cannot monetize at zero users. Add at 1,000+ active fans. | PARTIAL | Sponsorship transaction logic requires Claude Code. Audit at 500 active users. |
| Merch Integration from Comedian Profiles | Phase 2 | Requires Stripe e-commerce configuration and comedian product setup workflow. Defer to post-launch. | YES | GHL e-commerce or Printful integration. Low technical risk. Target: Sprint 9. |
| Comedian Referral Program | Phase 2 | Referral tracking requires attribution logic. Adds sprint complexity. Launch with direct recruitment first. | PARTIAL | GHL affiliate system can track referrals. Payout logic requires Claude Code. |
| Advanced Comedian Verification / Badge System | Phase 2 | Basic comedian registration is MVP. Tiered verification adds complexity without launch value. | YES | GHL custom fields support tier badges. Add after 50+ active comedians. |
| Fan Social Feed (follow-based activity feed with comments) | Phase 2 | Basic fan activity feed is in MVP. Full social feed with comments and reactions is Phase 2 after retention data confirms demand. | YES | GHL community features can power this. Audit after Day 30 retention data. |
| Collaborative Live Rooms (multi-comedian roast battles) | Phase 2 | Solo rooms are MVP. Multi-comedian rooms add technical complexity to Daily.co integration. Defer until solo rooms are stable. | PARTIAL | Daily.co supports multi-host. Requires additional room architecture. Target: Sprint 10. |
| Multi-Language Support | Phase 3 | Significant localization complexity across 37 screens. Requires professional translation. No international marketing budget at launch. | NO | Not a GHL native feature. Third-party i18n library required. Plan for Year 2. |
| App-Native Video Creation Tools | Phase 3 | Building a creator studio comparable to TikTok's editor is a separate product. Requires dedicated mobile development beyond GHL scope. | NO | Dedicated native development required. Separate product roadmap item. Year 2 or 3. |
| AI-Powered Comedy Matching | Phase 3 | Requires substantial watch history data to train on. No value at launch with small user base. | NO | Requires ML layer beyond GHL and Base44. Year 2. |
| Ad Revenue Share Program | Phase 3 | Requires a functioning ad server and minimum audience scale (industry standard: 1M+ monthly actives). | NO | Separate ad infrastructure required. Do not design for this before Year 3. |
| Brand Deal Access at 100 Subscribers | Phase 3 | Requires a verified brand marketplace that does not exist at launch. | PARTIAL | Threshold trigger is buildable in GHL. Brand marketplace requires separate infrastructure. |

---

## 22. Dev Handoff Protocol and Build Team Operating Rules

### 22.1 Roles and Responsibilities

| Party | Role | What They Are Responsible For |
|-------|------|-------------------------------|
| DDBA (Frank DeLuca) | Architecture, oversight, QA, and growth strategy | Master App Spec. Technical Spec. Feature definitions. Integration architecture. Sprint review sign-off. Gate 2 sign-off. Scope change authorization. All decisions about what to build and why. |
| GHL Build Team | Execute the build against the spec | Build every screen in the MVP Scope Document exactly as described. Configure all GHL workflows per automation library. Integrate all external systems per Technical Spec. Notify DDBA of sprint completion. Log deviations the same day they are found. |
| Abe Adewale (Client) | Platform owner and final approval authority | GHL account ownership and subscription payment. Gate 1 sign-off on Master App Spec. Apple Developer and Google Play account management. Abe does not direct the GHL build team during the build. All Abe requests during the build route through DDBA. |

> **HARD STOP:** Abe Adewale does not communicate directly with the GHL build team about build decisions during the build. All requests from Abe go through DDBA first. DDBA evaluates, approves or rejects, and relays to the GHL team in writing. This prevents scope creep and spec drift mid-build.

### 22.2 DDBA Approval Matrix

| Action | GHL Can Proceed | DDBA Approval Required |
|--------|----------------|------------------------|
| Build a screen that is in the MVP Scope Document | YES | NO |
| Configure a GHL workflow that is in the Automation Library | YES | NO |
| Create a GHL custom field from the Technical Spec field list | YES | NO |
| Integrate Mux, Daily.co, Stripe per Technical Spec instructions | YES | NO |
| Build a screen NOT in the MVP Scope Document | NO | YES — email DDBA with (1) screen description, (2) why needed, (3) what spec section it supports |
| Add a feature not in the spec to the build queue | NO | YES |
| Change or simplify a feature from how the spec describes it | NO | YES — email DDBA with (1) original spec language, (2) proposed change, (3) reason |
| Rename or restructure a GHL custom field from the canonical name | NO | YES — field name changes break workflow automation |
| Modify, disable, or delete an automation workflow after it is live | NO | YES — CRITICAL action |
| Contact Abe Adewale directly about a build decision | NO | YES — all client communication routes through DDBA |
| Change the platform from GHL to Lovable or Base44 for any feature | NO | YES — platform architecture decision |
| Move a deferred feature from the Phase 2 log into the current build | NO | YES |
| Resolve a spec ambiguity by making an independent interpretation | NO | YES — flag to DDBA with the specific question |
| Notify DDBA of sprint completion | YES | NO — GHL team responsibility |
| Flag a GHL capability gap discovered during build | YES | NO — flag immediately, action may require approval |

### 22.3 Sprint Review Protocol

| Item | Value |
|------|-------|
| Review Frequency | Weekly during active build. Every sprint must have a review before the next sprint starts. |
| DDBA Review SLA | DDBA completes sprint review within 2 business days of receiving the GHL team's sprint completion notification. |
| GHL Notification Content | Sprint number, list of screens built, list of any deviations from spec, link or access to the GHL sub-account for DDBA review. |
| Sprint Advance Authorization | GHL team does not begin the next sprint until DDBA issues written sprint advance authorization. |
| Deviation Found | DDBA logs the deviation in the Build Tracker Deviation Register (DEV-XXX). DDBA notifies GHL team in writing. GHL team resolves before next sprint advance. |
| Sprint Blocked | If a CRITICAL or HIGH deviation is found during review, the sprint is blocked. Sprint n+1 does not start until the deviation is resolved and DDBA re-reviews. |

### 22.4 Communication Protocol

| Situation | Channel | Who Sends | DDBA SLA | Required Format |
|-----------|---------|-----------|----------|-----------------|
| Sprint completion notification | Email or agreed project management tool | GHL Team Lead | 2 business days for DDBA review response | Subject: ComedyVault Sprint [N] Complete. Body: (1) screens built, (2) deviations, (3) link to GHL sub-account. |
| Capability gap discovered | Email | GHL Team Lead | Same business day acknowledgment | Subject: ComedyVault Capability Gap — [Feature Name]. Body: (1) feature, (2) specific GHL limitation, (3) workaround options if any. |
| Scope clarification question | Email | GHL Team Lead | 2 business days response | Subject: ComedyVault Spec Question — [Section/Feature]. Do not proceed until DDBA responds. |
| DDBA sprint advance authorization | Email | DDBA (Frank DeLuca) | After sprint review completion | Subject: ComedyVault Sprint [N] — APPROVED or BLOCKED. |
| Emergency build-stopping issue | Phone + follow-up email | GHL Team Lead to Frank DeLuca | Immediate phone. Email follow-up within 1 hour. | Phone call to Frank DeLuca directly. Followed by email with issue description. |

### 22.5 Build Quality Standards

The GHL build team is responsible for delivering to DDBA's standard, not GHL's default standard. ComedyVault is a consumer product. These standards apply to every screen and every workflow built.

Page load performance target: 3 seconds or under on a standard 4G mobile connection.
Content gate accuracy: 100%. No free user ever accesses post-preview content without Premium membership.
Workflow execution accuracy: All 17 automation workflows fire on their specified triggers without exception.
Data integrity: Every GHL custom field is named exactly as specified. No field is renamed, retyped, or deleted without DDBA written authorization.
Mobile-first: Every screen is designed and tested for mobile before desktop. GHL white-label app delivery requires mobile-first implementation.
No broken states: Every edge case in Section 19 has a tested, defined response. No screen ever shows a blank page, an unhandled error, or a dead-end navigation state.

---

## 23. Pricing Architecture Reference

### 23.1 Revenue Streams

| Revenue Stream | Amount | Who Pays | Platform Cut | Notes |
|----------------|--------|----------|--------------|-------|
| Fan Premium Subscription | $6.99/month | Fan to Platform | Platform keeps 20% — 80% distributed to attributed comedian | Primary recurring revenue |
| Comedian Monetization Unlock | $16.00 one-time | Comedian to Platform | 100% to platform | Commitment signal. Filters active from passive comedians. |
| Revenue Share (platform retained) | 20% of Premium revenue | N/A — retained from fan subscription | 20% to platform operations | Covers GHL, Mux, Daily.co, Base44, DDBA support, future development |
| Direct Tips | $1 to $100 | Fan to Comedian via Platform | Stripe processing fee only (2.9% + $0.30) — 100% of tip goes to comedian net of Stripe fee | No platform cut on tips. Builds goodwill with both sides. |
| Brand Sponsor Portal | TBD | Brand to Platform / Comedian | Split TBD | Phase 2. Not available at MVP. |

### 23.2 Pricing Rationale

The $6.99 price point sits at the intersection of five constraints:

1. **Conversion Psychology:** Below the $10 threshold where a consumer subscription decision stops feeling like a meaningful financial commitment.
2. **Operating Cost Coverage:** At 200 paying fans, $6.99/month generates $1,398/month gross revenue. The platform reaches operating cost coverage at approximately 500 paying fans.
3. **Comedian Payout Viability:** At $6.99/month, a comedian with 50 attributed Premium fans earns approximately $259.80/month (80% net of Stripe fees). At 100 fans: approximately $519.61/month. These are credible proof points for founding comedian recruitment.
4. **Competitive Positioning:** $6.99 undercuts Netflix ($15.49+), sits below Spotify Premium ($11.99), and is in range with Patreon creator subscriptions ($5 to $20).
5. **App Store IAP Economics:** If all Premium subscriptions are processed through Apple or Google IAP, the platform cut is 30% (or 15% under Small Business Program). At 30%: $6.99 x 0.70 = $4.89 net. The web checkout option (Stripe) produces $6.49 net.

### 23.3 Price Sensitivity Analysis

| Monthly Price | Conv. Rate Est. | Net (Stripe) | Comedian 80% | Platform 20% | Trade-off |
|---------------|----------------|-------------|-------------|-------------|----------|
| $4.99 | 5-8% | $4.55 | $3.64 | $0.91 | Higher conversion, lower payout per subscriber. Platform struggles to cover costs below 1,000 fans. |
| **$6.99 (SELECTED)** | 3-5% | $6.49 | **$5.19** | $1.30 | Balanced. Low enough to convert impulse. High enough to generate meaningful comedian income at 50-100 fans. |
| $9.99 | 2-3% | $9.41 | $7.53 | $1.88 | Better payout per fan, lower conversion. Right for an established platform. Risky at launch. |
| $12.99 | 1.5-2.5% | $12.62 | $10.10 | $2.52 | Strong unit economics, weak conversion. Only viable with high-profile comedian content. |

---

## 24. Change Control Protocol

After Gate 1 sign-off, any change to this document requires written authorization from DDBA. This is not a formality. It is a build integrity requirement. Unauthorized changes to this spec mid-build produce spec-to-build misalignment that the QA checklist will catch and require re-build to fix.

| Rule | Value |
|------|-------|
| Who can request a change | Abe Adewale (client) or the GHL build team. Changes cannot be authorized by the GHL build team alone. |
| How to request a change | Email DDBA with: (1) Section of this spec the change affects, (2) current spec language, (3) proposed new language, (4) reason for change. |
| DDBA review SLA | DDBA reviews and responds to change requests within 2 business days. |
| Change classification | Minor (cosmetic, copy, field label): DDBA approves without document revision. Major (feature, logic, flow, data field, automation): document is revised, re-versioned, and must be re-signed by Abe. |
| Change log | Every approved change is logged in the Change Log below with date, requester, change description, and DDBA authorization. |
| Unauthorized changes | If the GHL build team builds something not in this spec without prior DDBA authorization, the work is out of scope. It will be audited in Sprint review and either (a) approved and added to this spec retroactively, or (b) removed and rebuilt to spec. Out-of-scope work does not delay the sprint gate. |

### Change Log

| Date | Version | Requester | Change Description | DDBA Authorization |
|------|---------|-----------|-------------------|--------------------|
| | | | | |
| | | | | |
| | | | | |

---

## 25. Gate 1 Sign-Off

Sprint 1 does not start until both sign-off lines below are completed and this document is filed. A build that starts before this section is signed is an unauthorized build.

By signing below, both parties confirm this document is accurate, complete, and approved as the single source of truth for the ComedyVault build. I confirm I have read this entire document. I understand that the features, flows, logic, and scope defined here are locked. Changes require written authorization from DDBA.

**Client (Abe Adewale):**

Signature: _______________________________ Date: _______________

**DDBA Operator (Frank DeLuca):**

Signature: _______________________________ Date: _______________

**Sprint 1 Authorized:** YES / NO

**Authorized Start Date:** _______________

**Version of document signed:** v1.0

**Document file name:** ComedyVault_MASTER_SPEC_v1.md

---

*DDBA | DeLuca Designs | Master App Spec | ComedyVault | v1.0 | June 2026 | Gate 1 Sign-Off Required*

*Single Source of Truth for the ComedyVault Build | Changes require written DDBA authorization*

*Confidential | DeLuca Designs LLC | DDBA Ecosystem*
