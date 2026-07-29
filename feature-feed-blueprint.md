# Feature Feed — Engineering Scope / Blueprint

Scope: **Feature Feed only.** (Tickets and Ask AI are out of scope for this phase.)
Location: Engagement tab (`NEW`) in the Harness product left nav.

---

## Consumer View

* All posts sorted in reverse chronological order
* Default filter: current quarter

### 1. Feed Header
1. Title — "Feature Feed"
2. Subtitle — "latest releases & improvements"
3. Collapse / expand feed toggle

### 2. Filters
1. Time period — Quarter / date range (default: current quarter)
2. Module — CI, CD, FF, CCM, STO, Chaos, SEI, IDP, Code Repo, etc.
3. Sub Module — GitOps, Deployment, FF, CV, Approvals, OPA, etc.
4. Category — Performance, UX, Quality, Usability
5. "Clear all" — resets all filters
6. Result count — "Showing X to Y of N features"

### 3. Paginated List of Posts
1. Post Tag — New / Improved
2. Post Title — single liner
3. Post Description — 2 liner
4. Date + timeago — top right of post
5. Module Tag — CI, CD, etc.
6. Sub Module Tag — GitOps, Deployment, FF, CV, Approvals, OPA, etc.
7. Category Tag — Performance, UX, Quality, Usability
8. Media (optional, per post)
   1. Thumbnail (image)
   2. Video preview card (YouTube link)
9. CTAs (conditional — render only when data present)
   1. Enable Feature (FF) — if flag key attached
   2. Go to Docs — if docs link attached
   3. Watch Video — if video link attached (opens player)
   4. Open Image — if image attached (opens full screen)

### 4. CTA — Enable Feature (FF)
1. Visible only when post has an FF flag key
2. Click → confirmation dialog
   1. Feature name
   2. Flag key (e.g. `CI_CACHE_INTELLIGENCE`)
   3. Account (auto-filled, current account)
   4. Environment (default: Production)
   5. Target rollout (default: ON / 100%)
   6. Cancel / Enable Flag
3. On confirm
   1. Call FF service to set flag ON for account
   2. Button → "✓ FF Enabled" state (non-clickable)
   3. Success toast
4. States — default / enabling (loading) / enabled / error
5. Permissions — hide/disable if user lacks FF edit rights

### 5. CTA — Go to Docs
1. Opens docs link in new tab

### 6. CTA — Watch Video
1. Opens video player (modal / lightbox)
2. Fallback to poster thumbnail + external link if embed blocked

### 7. CTA — Open Image
1. Opens image full screen (lightbox)
2. Close on ✕ / Esc / backdrop click

### 8. Pagination
1. "Load more" (or numbered pages)
2. Result count updates
3. States — loading / empty ("No features match filters") / error / end of list

---

## Admin View

Two candidate approaches (pick one for phase 1):

### Approach 1 — Post via Pipeline
1. Post authored as pipeline step / config (YAML)
2. Publishing gated by pipeline approval
3. Version-controlled in Git
4. Auto-publish on merge / release

### Approach 2 — Separate Admin Dashboard
1. Create / Edit / Delete post UI
2. Draft / Publish / Schedule
3. Preview before publish
4. Role-gated access (admin only)

### Post Fields (create / edit — both approaches)
1. Tag — New / Improved
2. Title — single liner
3. Description — 2 liner
4. Publish date (default now; schedulable)
5. Module (required)
6. Sub Module (optional, multi)
7. Category (optional, multi)
8. Docs link (optional)
9. Video link (optional)
10. Image upload (optional)
11. FF flag key (optional — enables "Enable FF" CTA)
12. Targeting — all accounts / specific accounts / by module entitlement

### Post Lifecycle
1. Draft → Scheduled → Published → Archived
2. Edit published post
3. Unpublish / archive

---

## Data Model (Post)

| Field | Type | Notes |
|---|---|---|
| id | string | unique |
| tag | enum | New / Improved |
| title | string | single liner |
| description | string | 2 liner |
| publishedAt | datetime | drives sort + timeago |
| module | enum | CI, CD, FF, ... |
| subModules | enum[] | GitOps, Deployment, ... |
| categories | enum[] | Performance, UX, ... |
| docsUrl | string? | enables Docs CTA |
| videoUrl | string? | enables Video CTA |
| imageUrl | string? | enables Image CTA |
| ffFlagKey | string? | enables Enable-FF CTA |
| targeting | object | all / accounts / entitlement |
| status | enum | draft / scheduled / published / archived |

---

## Cross-cutting

1. Access — feed visible to all users in account; admin authoring role-gated
2. Entitlement — posts respect module entitlement (hide features user can't access) — TBD
3. Responsive — feed usable on narrow viewports
4. Analytics / events (for adoption metrics)
   1. post_impression
   2. post_expand
   3. docs_click
   4. video_play
   5. image_open
   6. ff_enable_click / ff_enable_confirm / ff_enable_success
   7. filter_apply
5. Empty / loading / error states for feed + each CTA

---

## Out of Scope (this phase)
1. Tickets (raise / track / JIRA sync)
2. Ask AI integration
3. Comments / reactions on posts
4. Quarter Timeline
