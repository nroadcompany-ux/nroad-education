# DESIGN.md — Toss (토스)

> Inspired by Toss Design System (TDS) — Korea's leading fintech super-app.
> Source: tossmini-docs.toss.im · toss.im
> Clean, trustworthy, minimal. Designed for clarity at every scale.

---

## 1. Visual Theme & Atmosphere

**Mood:** Clean white canvas, high trust, zero friction. Every element earns its place.

**Design Philosophy:**
- Radical simplicity — remove until you can't remove anymore
- Content-first layout: numbers and data speak without decoration
- Whitespace is a design element, not empty space
- Micro-interactions feel smooth and intentional, never flashy
- Korean-first typography that feels native on any device
- Mobile-first but scales to admin dashboards naturally

**Personality:** Trustworthy, friendly, precise. Like a helpful bank clerk who never wastes your time.

**What Toss looks like:**
- Pure white backgrounds with barely-there grey surfaces
- Bold blue (#3182f6) used sparingly but decisively
- Typography does the heavy lifting — no decorative flourishes
- Cards with subtle shadows, generous inner padding
- Rounded corners throughout (8–16px)
- Lists and rows, not grids — information flows vertically

---

## 2. Color Palette & Roles

### Primary Brand
| Token | Hex | Role |
|-------|-----|------|
| `blue500` | `#3182f6` | Primary CTA, links, active states |
| `blue600` | `#2272eb` | Button hover |
| `blue700` | `#1b64da` | Button pressed |
| `blue50` | `#e8f3ff` | Light blue background, info badges |
| `blue100` | `#c9e2ff` | Selected row background |

### Greyscale (Core UI)
| Token | Hex | Role |
|-------|-----|------|
| `grey50` | `#f9fafb` | Page background, subtle surface |
| `grey100` | `#f2f4f6` | Card background, input background |
| `grey200` | `#e5e8eb` | Dividers, borders |
| `grey300` | `#d1d6db` | Disabled borders |
| `grey400` | `#b0b8c1` | Placeholder text |
| `grey500` | `#8b95a1` | Secondary/caption text |
| `grey600` | `#6b7684` | Tertiary text |
| `grey700` | `#4e5968` | Label text |
| `grey800` | `#333d4b` | Body text |
| `grey900` | `#191f28` | Primary text, headings |

### Semantic Colors
| Token | Hex | Role |
|-------|-----|------|
| `red500` | `#f04452` | Error, destructive, negative amount |
| `red50` | `#ffeeee` | Error background |
| `green500` | `#03b26c` | Success, positive amount, confirmed |
| `green50` | `#f0faf6` | Success background |
| `orange500` | `#fe9800` | Warning |
| `orange50` | `#fff3e0` | Warning background |
| `yellow500` | `#ffc342` | Notice, highlight |

### Backgrounds
| Token | Hex | Role |
|-------|-----|------|
| `background` | `#ffffff` | Page / modal base |
| `greyBackground` | `#f2f4f6` | Section background, list surface |
| `layeredBackground` | `#ffffff` | Card on card |

---

## 3. Typography Rules

**Font Family (Korean):** `Pretendard` → fallback `Apple SD Gothic Neo`, `Noto Sans KR`, `sans-serif`

**Font Family (Latin/Numbers):** `Pretendard` handles both well. Numbers use tabular figures for alignment.

**Font Weights Available:** Light (300) · Regular (400) · Medium (500) · Semibold (600) · Bold (700)

### Type Scale (Web / Admin)
| Token | Size | Line Height | Weight | Usage |
|-------|------|-------------|--------|-------|
| `Typography1` | 30px | 40px | Bold | Hero numbers, large display |
| `Typography2` | 26px | 35px | Bold/Semibold | Page title, modal header |
| `Typography3` | 22px | 31px | Semibold | Section heading |
| `Typography4` | 20px | 29px | Semibold | Card title, table header |
| `Typography5` | 17px | 25.5px | Medium/Regular | Body, primary content |
| `Typography6` | 15px | 22.5px | Regular | Secondary content, labels |
| `Typography7` | 13px | 19.5px | Regular | Caption, metadata, helper text |

**Typography Rules:**
- **Never** use font sizes below 13px
- Body text minimum: 15px (Typography6)
- Line height ratio: approximately 1.5× font size
- Letter spacing: `-0.3px` for headings, `0` for body
- Numbers (amounts, stats) always use `font-variant-numeric: tabular-nums`
- Don't mix more than 2 weights on a single screen

---

## 4. Component Styling

### Buttons
```
Primary Button:
  background: #3182f6 (blue500)
  color: #ffffff
  border-radius: 10px
  padding: 14px 20px
  font: 17px/500 (Typography5 Medium)
  hover: #2272eb (blue600)
  pressed: #1b64da (blue700)
  disabled: #b0b8c1 (grey400) bg, white text

Secondary Button:
  background: #f2f4f6 (grey100)
  color: #191f28 (grey900)
  border-radius: 10px
  padding: 14px 20px
  hover: #e5e8eb (grey200)

Ghost/Text Button:
  background: transparent
  color: #3182f6 (blue500)
  no border
  hover: underline or blue50 bg

Destructive:
  background: #f04452 (red500)
  color: #ffffff
```

### Cards
```
Surface: #ffffff
Border: none (use shadow instead)
Border-radius: 16px
Padding: 20px 24px
Shadow: 0 2px 8px rgba(0, 0, 0, 0.06), 0 0 1px rgba(0, 0, 0, 0.08)

List Card (데이터 목록):
  Divider: 1px solid #e5e8eb (grey200)
  Row padding: 16px 0
  No external borders on rows
```

### List Rows (핵심 컴포넌트)
```
Height: 56–72px (content-driven)
Padding: 0 20px
Background: #ffffff
Divider: 1px solid #f2f4f6 (grey100) — hairline
Leading: icon (24px) or avatar (36px)
Trailing: value text + chevron or badge
Hover: #f9fafb (grey50) background

Label (left):   15–17px / grey900 / Medium
Value (right):  15–17px / grey900 / Regular
Sublabel:       13px / grey500 / Regular
```

### Inputs / Text Fields
```
Height: 52px
Background: #f2f4f6 (grey100)
Border: none (default) / 2px solid #3182f6 (focused)
Border-radius: 10px
Padding: 0 16px
Font: 17px / grey900
Placeholder: grey400
Error state: border 2px solid #f04452, helper text in red500
```

### Badges
```
Positive/Info: blue50 bg + blue500 text
Success:       green50 bg + green500 text
Warning:       orange50 bg + orange500 text
Error:         red50 bg + red500 text
Neutral:       grey100 bg + grey700 text
Border-radius: 6px
Padding: 2px 8px
Font: 13px / Semibold
```

### Navigation / Sidebar (Admin)
```
Width: 240px (expanded) / 60px (collapsed)
Background: #ffffff
Border-right: 1px solid #e5e8eb
Active item: blue50 bg + blue500 text + 3px left border blue500
Inactive item: grey700 text
Item height: 48px
Item padding: 0 16px
Font: 15px / Medium
```

### Tables
```
Header: grey50 bg, grey700 text, 13px/Semibold, uppercase
Row: white bg, grey900 text, 17px/Regular
Row height: 56px
Hover row: grey50 bg
Divider: 1px solid grey200
Selected row: blue50 bg
Cell padding: 0 16px
```

### Toast / Alerts
```
Success: green50 bg + green700 text + green500 icon
Error:   red50 bg + red700 text + red500 icon
Info:    blue50 bg + blue700 text + blue500 icon
Border-radius: 12px
Padding: 14px 16px
Shadow: 0 4px 16px rgba(0,0,0,0.12)
```

### Skeleton / Loading
```
Base: #f2f4f6 (grey100)
Shimmer: linear-gradient(90deg, #f2f4f6 0%, #e5e8eb 50%, #f2f4f6 100%)
Animation: 1.5s ease-in-out infinite
Border-radius: matches the element shape
```

---

## 5. Layout Principles

**Spacing Scale (8px base):**
| Token | Value | Usage |
|-------|-------|-------|
| `space-1` | 4px | Icon gap, tight pairs |
| `space-2` | 8px | Inner padding small |
| `space-3` | 12px | Component internal |
| `space-4` | 16px | Card padding, list padding |
| `space-5` | 20px | Section gap |
| `space-6` | 24px | Card padding large |
| `space-8` | 32px | Section spacing |
| `space-10` | 40px | Page section gap |
| `space-12` | 48px | Hero spacing |

**Grid System (Admin Web):**
- Max content width: 1200px (centered)
- Sidebar: 240px fixed
- Main content: fluid, min 600px
- Column gap: 24px
- Row gap: 16px

**Whitespace Philosophy:**
- Content breathes — no element is cramped
- Group related items tightly, separate unrelated groups generously
- Padding inside cards: 20–24px (never under 16px)
- Page margin (mobile): 20px horizontal

---

## 6. Depth & Elevation

**Shadow Levels:**
```
Level 0 — Flat:     no shadow (default page surface)
Level 1 — Raised:   0 2px 8px rgba(0,0,0,0.06), 0 0 1px rgba(0,0,0,0.06)
Level 2 — Float:    0 4px 16px rgba(0,0,0,0.10), 0 0 2px rgba(0,0,0,0.06)
Level 3 — Modal:    0 8px 32px rgba(0,0,0,0.14), 0 0 2px rgba(0,0,0,0.08)
Level 4 — Overlay:  0 16px 48px rgba(0,0,0,0.18)
```

**Surface Hierarchy:**
1. Page — `#f2f4f6` (grey100)
2. Card — `#ffffff` + Level 1 shadow
3. Floating element (dropdown, tooltip) — `#ffffff` + Level 2 shadow
4. Modal / Dialog — `#ffffff` + Level 3 shadow + backdrop `rgba(0,0,0,0.4)`

---

## 7. Do's and Don'ts

### ✅ DO
- Use `blue500` only for primary actions — max 1–2 per screen
- Let typography carry hierarchy; avoid decorative elements
- Use grey100 for page backgrounds, white for interactive cards
- Align numbers right, labels left in tables
- Round all monetary values with `Intl.NumberFormat` comma formatting
- Use `tabular-nums` for all financial figures
- Provide immediate visual feedback on every interaction (hover, press, focus)
- Keep button text short and action-oriented (저장, 확인, 다음)

### ❌ DON'T
- Never use gradients (Toss is flat)
- Don't mix more than 2 font weights per screen section
- Don't use red for anything other than errors or negative values
- Don't use shadows heavier than Level 2 on cards
- Don't use colored backgrounds for sections — stick to grey50/grey100/white
- Don't add borders to buttons that already have a background color
- Don't truncate important data — let it wrap or scroll
- Don't use font size below 13px

---

## 8. Responsive Behavior

**Breakpoints:**
```
mobile:  < 768px   (full-width, single column, bottom nav)
tablet:  768–1024px (sidebar collapses to icon-only, 2-col possible)
desktop: > 1024px  (full sidebar, multi-column admin layout)
```

**Mobile Admin Patterns:**
- Sidebar becomes a bottom tab bar (4–5 icons max)
- Cards go full-width with 16px margin
- Tables become scrollable horizontally or collapse to list rows
- Touch targets: minimum 44×44px
- Floating action button: 56px circle, blue500, bottom-right

**Responsive Typography:**
- Headings scale down 2–4px on mobile
- Body stays at 15–17px (readability priority)

---

## 9. Agent Prompt Guide

### Quick Color Reference
```
Primary blue:    #3182f6
Text primary:    #191f28
Text secondary:  #6b7684
Text tertiary:   #8b95a1
Placeholder:     #b0b8c1
Border:          #e5e8eb
Surface light:   #f2f4f6
Page bg:         #f9fafb
White:           #ffffff
Success:         #03b26c
Error:           #f04452
Warning:         #fe9800
```

### Ready-to-Use Prompts

**Dashboard:**
> "Build an admin dashboard with a white card grid showing key metrics. Use grey50 page background, white cards with Level 1 shadow. Numbers in Typography2 Bold grey900, labels in Typography7 grey500. Blue500 for positive indicators."

**List Page:**
> "Create a data list page with full-width rows. Each row: 56px height, 20px horizontal padding, hairline grey200 divider. Label in grey900 Typography6 Medium, value right-aligned in grey700, chevron in grey400."

**Form:**
> "Design a form with grey100 input backgrounds, no input borders, blue500 2px border on focus. 52px input height, 10px border-radius. Error state in red500 with helper text below."

**Navigation:**
> "Build a left sidebar 240px wide on white background. Active item: blue50 background, blue500 3px left border, blue500 text. Inactive items: grey700 text. 48px row height, Pretendard 15px Medium."

**Data Table:**
> "Create a table with grey50 header, grey900 body text. 56px row height, grey200 dividers, blue50 selected row. Right-align all numeric columns with tabular-nums. No external table border."
