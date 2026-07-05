# 學習路上 × 資源庫 (Expert Resource Library) — Design Spec

**Date:** 2026-07-04  
**Status:** Design approved, ready for implementation planning  
**Owner:** Iris Wen

---

## Overview

Add a fourth bottom-navigation tab called **資源庫** to the 學習路上 learning platform, featuring curated expert analyses and public content organized by expert and theme. This creates a "marketplace of expertise" where learners can discover high-quality insights from recognized specialists (e.g., 吳嘉隆, 張明輝).

---

## Goals

1. **Credibility & Discovery** — Surface expert-curated content alongside learner-generated progress (quizzes, essays)
2. **Low-friction curation** — Manually curate links to expert public content (articles, videos, podcasts) rather than create original interviews
3. **Scalable structure** — Launch MVP with 5–6 core experts; add more incrementally without redesign
4. **Consistent UX** — Match existing 學習路上 navigation, card styling, and color theme

---

## Design Details

### Navigation

**Bottom Navigation Tabs (unchanged count, +1 new):**
```
| 市場 | 傳承 | 英文 | 資源庫 |
```

- New tab: **資源庫** (Resource Library / Expert Hub)
- Label font: same as existing tabs (bold, serif CJK)
- Icon: SVG line-style matching morning-board convention (not emoji)
- Active state: same aqua/gold styling as other tabs

---

### 1. List View (Collapsed Cards)

**Screen:** When 資源庫 tab is selected, show a scrollable list of expert cards.

**Card anatomy (collapsed):**
```
┌─────────────────────────────────┐
│ 吳嘉隆                        ▼  │  ← expert name, expand icon
│ 經濟學分析家                     │  ← one-line focus
│ #總體經濟 #股市 #投資策略         │  ← expertise tags (chips)
└─────────────────────────────────┘
```

**Card styling:**
- Background: white (`--card`)
- Border: 1.5px solid `--border`
- Border-radius: 16px (match existing plan-block)
- Padding: 1rem 1.1rem
- Shadow: light (0 2px 12px rgba(0,31,77,.05))
- Margin-bottom: 1rem

**Text hierarchy:**
- Expert name: 1rem, bold (`font-weight: 800`), `--primary` (aqua)
- Focus: 0.9rem, regular, `--text`
- Tags: chips (0.76rem, `--primary` bg `--active`, white border)

**Interaction:**
- Tap/click to expand; tap again or click X to collapse
- Smooth height/opacity transition (0.25s fade + slide)

---

### 2. Expanded View (Full Content)

**When card is expanded, it grows to show:**

```
┌─────────────────────────────────┐
│ 吳嘉隆                        ✕  │  ← close button
│ 經濟學分析家 | #總體經濟 #股市  │
│ 台灣及全球經濟趨勢分析家。      │  ← bio (1–2 sentences)
├─────────────────────────────────┤
│ 總體經濟                        │  ← section theme
│ • 2024年全球經濟展望        →  │  ← link + source tag
│   YouTube | 15分鐘視頻
│ • 台灣Q2 GDP預估分析        →  │
│   部落格 | 最新發表
├─────────────────────────────────┤
│ 股市分析                        │
│ • AI浪潮下的台股機會        →  │
│   Podcast | 50分鐘
│ ...
└─────────────────────────────────┘
```

**Expanded card anatomy:**

1. **Header**
   - Expert name (larger, bold aqua)
   - Close icon (X, top-right)
   - Focus + tags (compact)
   - Bio (0.9rem, `--muted`, line-height 1.6)
   - Separator line (light border)

2. **Sections** (repeating by theme)
   - Section heading (e.g., "總體經濟", bold `--primary`, 1rem)
   - Link list:
     ```
     • [Link Title]                        →
       [Source Badge] | [Brief description]
     ```
     - Title: clickable, `--primary` underline on hover
     - Source badge: small chip (YouTube / 部落格 / Podcast / 新聞等)
     - Description: 0.85rem, `--muted`
     - Arrow icon on right for "external link" cue

3. **Styling notes**
   - Section padding: 0.8rem top, borders between sections
   - Max width per card: 100% (responsive)
   - On mobile, card fills viewport less padding at edges
   - No nested boxes; content flows directly in card

---

### 3. Data Structure (JSON)

**File:** `/xuexi_lushang_site/experts.json`

```json
{
  "experts": [
    {
      "id": "wu-jialone",
      "name": "吳嘉隆",
      "title": "經濟學分析家",
      "bio": "台灣及全球經濟趨勢分析家，長期從事總體經濟研究。",
      "tags": ["總體經濟", "股市", "投資策略"],
      "sections": [
        {
          "theme": "總體經濟",
          "links": [
            {
              "title": "2024年全球經濟展望",
              "description": "15分鐘視頻，分析美中貿易及利率趨勢",
              "source": "YouTube",
              "url": "https://..."
            },
            {
              "title": "台灣Q2 GDP預估分析",
              "description": "最新發表，聚焦半導體與出口動能",
              "source": "部落格",
              "url": "https://..."
            }
          ]
        },
        {
          "theme": "股市分析",
          "links": [
            {
              "title": "AI浪潮下的台股機會",
              "description": "50分鐘Podcast，台股如何受惠AI趨勢",
              "source": "Podcast",
              "url": "https://..."
            }
          ]
        }
      ]
    },
    {
      "id": "zhang-minghui",
      "name": "張明輝",
      "title": "會計師 / 財務顧問",
      "bio": "...",
      "tags": ["企業財務", "稅務規劃", "家族信託"],
      "sections": [...]
    }
    // 4–5 more experts (MVP: 5–6 total)
  ]
}
```

**Notes:**
- Minimal structure: id, name, title, bio, tags, sections with theme + links
- Each link: title, description, source, url
- Hand-maintained; no auto-fetching required for MVP
- Commit to git with project

---

### 4. Integration Points

**File changes:**
- `index.src.html`: Add 資源庫 tab to bottom-nav; add new `.tab.experts` section
- `index.src.html` (JS): Add tab-switch logic for 資源庫; expand/collapse handlers
- **New file** `experts.json`: Expert data (see above)
- Optional: Separate `css` or inline `<style>` for expanded card animations

**Build process:**
- Encrypt final `index.html` via StatiCrypt (no change to build script)
- `experts.json` is read-only for users (embedded in HTML on build)

---

### 5. MVP Scope

**Launch with:**
- 5–6 hand-selected experts (吳嘉隆, 張明輝, + 3–4 others you identify)
- 3–4 theme sections per expert
- 3–5 curated links per section
- Manual JSON maintenance

**Future phases (out of scope):**
- Search / filter by expertise or theme
- "Follow" or "save" links (user persistence)
- Dynamic content fetch from expert APIs or feeds
- Admin panel for link management

---

### 6. Success Criteria

✓ 資源庫 tab appears in bottom nav alongside 市場/傳承/英文  
✓ Cards render with collapsed state (name + title + tags visible)  
✓ Clicking card expands to show curated content by theme  
✓ Links open in new tab without breaking navigation  
✓ Mobile responsive (card scales, no overflow)  
✓ Styling consistent with existing site (colors, fonts, spacing)  
✓ `experts.json` maintained & version-controlled  
✓ Easy to add new experts incrementally  

---

## Open Questions / Decisions

**None** — design is complete and approved. Ready for implementation planning.

---

## Next Steps

1. Invoke **writing-plans** skill to create detailed implementation plan
2. Implement HTML structure, JS expand/collapse, styling
3. Populate initial `experts.json` with 5–6 core experts
4. Test on mobile; verify external links load correctly
5. StatiCrypt encrypt and commit

