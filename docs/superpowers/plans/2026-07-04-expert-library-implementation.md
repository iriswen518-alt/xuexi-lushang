# 學習路上 資源庫 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a fourth 資源庫 (expert library) tab to 學習路上, enabling users to discover curated expert analyses organized by specialist and theme.

**Architecture:** 
- Flat JSON data file (`experts.json`) stores 5–6 expert profiles, each with themed link collections
- HTML tab structure reuses existing `.tab` component pattern; cards expand/collapse via JS toggles
- CSS animations (height transition, fade) keep UX smooth; responsive design via existing `@media` breakpoints

**Tech Stack:** 
- Vanilla JS (no new dependencies)
- CSS3 (flex, transitions)
- JSON data (hand-maintained)
- StatiCrypt encryption (existing build)

## Global Constraints

- Bottom-nav button colors: use `--primary`, `--active`, `--active-text`, `--muted` from existing CSS variables
- Card styling: match `.plan-block` (border-radius: 16px, shadow, padding)
- SVG icons: line-style, no emoji (matching morning-board convention)
- Mobile: responsive via `@media` queries; cards fill width with safe padding
- Data: `experts.json` committed to git; no auto-fetching for MVP

---

## File Structure

### Files to Create
- `experts.json` — Expert profiles and curated content (new, 200–400 lines)

### Files to Modify
- `index.src.html` — Add bottom-nav tab, HTML structure, CSS, JS logic (~250 lines added/modified)

### Build Output
- `index.html` — Auto-encrypted by StatiCrypt (no manual edit)

---

## Task Breakdown

### Task 1: Create experts.json with 5–6 Expert Profiles

**Files:**
- Create: `xuexi_lushang_site/experts.json`

**Interfaces:**
- Produces: `experts` array (imported by Task 2)
  - Each expert: `{ id, name, title, bio, tags[], sections[] }`
  - Each section: `{ theme, links[] }`
  - Each link: `{ title, description, source, url }`

- [ ] **Step 1: Write experts.json with 吳嘉隆 + 張明輝 + 3–4 additional experts**

Create file `/xuexi_lushang_site/experts.json`:

```json
{
  "experts": [
    {
      "id": "wu-jialone",
      "name": "吳嘉隆",
      "title": "經濟學分析家",
      "bio": "台灣及全球經濟趨勢分析家，長期從事總體經濟研究，深度剖析重大經濟事件。",
      "tags": ["總體經濟", "股市", "投資策略"],
      "sections": [
        {
          "theme": "總體經濟",
          "links": [
            {
              "title": "2024年全球經濟展望",
              "description": "美中貿易局勢與全球利率趨勢分析",
              "source": "YouTube",
              "url": "https://example.com/wu-global-2024"
            },
            {
              "title": "台灣經濟未來挑戰",
              "description": "半導體産業與人口結構對經濟的影響",
              "source": "部落格",
              "url": "https://example.com/wu-taiwan-challenge"
            }
          ]
        },
        {
          "theme": "股市分析",
          "links": [
            {
              "title": "科技股周期與輪動",
              "description": "AI浪潮下台股科技類股的投資邏輯",
              "source": "Podcast",
              "url": "https://example.com/wu-tech-cycle"
            }
          ]
        },
        {
          "theme": "投資策略",
          "links": [
            {
              "title": "景氣循環投資法",
              "description": "根據經濟周期調整資產配置的實操指南",
              "source": "線上課程",
              "url": "https://example.com/wu-cycle-strategy"
            }
          ]
        }
      ]
    },
    {
      "id": "zhang-minghui",
      "name": "張明輝",
      "title": "會計師 / 財務顧問",
      "bio": "執業會計師，專長企業財稅規劃與家族傳承設計，已服務逾千位高淨值客戶。",
      "tags": ["企業財務", "稅務規劃", "家族信託"],
      "sections": [
        {
          "theme": "企業財務",
          "links": [
            {
              "title": "新公司法下的股權規劃",
              "description": "2023年新修法對股權結構的影響與調整建議",
              "source": "新聞",
              "url": "https://example.com/zhang-equity"
            }
          ]
        },
        {
          "theme": "稅務規劃",
          "links": [
            {
              "title": "高淨值族群的稅效優化",
              "description": "合法節稅的策略與注意事項",
              "source": "部落格",
              "url": "https://example.com/zhang-tax"
            }
          ]
        },
        {
          "theme": "家族信託",
          "links": [
            {
              "title": "信託規劃101",
              "description": "家族資產傳承的信託工具完全指南",
              "source": "線上課程",
              "url": "https://example.com/zhang-trust"
            }
          ]
        }
      ]
    },
    {
      "id": "economist-a",
      "name": "陳家琪",
      "title": "產業分析師",
      "bio": "聚焦台灣半導體與電子產業，提供前瞻性的產業研究與投資視角。",
      "tags": ["半導體", "電子產業", "供應鏈"],
      "sections": [
        {
          "theme": "半導體趨勢",
          "links": [
            {
              "title": "2024年晶片產業大預測",
              "description": "AI晶片需求與成熟製程的新機會",
              "source": "新聞",
              "url": "https://example.com/chen-chip-2024"
            }
          ]
        },
        {
          "theme": "供應鏈分析",
          "links": [
            {
              "title": "地緣政治對台灣電子業的影響",
              "description": "貿易變化與廠商應對策略",
              "source": "部落格",
              "url": "https://example.com/chen-geopolitics"
            }
          ]
        }
      ]
    },
    {
      "id": "fund-manager-b",
      "name": "王建民",
      "title": "基金經理人",
      "bio": "擁有20年投資經驗，專長全球資產配置與風險管理。",
      "tags": ["資產配置", "國際投資", "風險管理"],
      "sections": [
        {
          "theme": "資產配置",
          "links": [
            {
              "title": "2026年投資展望",
              "description": "股債配置與新興市場機會評估",
              "source": "YouTube",
              "url": "https://example.com/wang-allocation-2026"
            }
          ]
        }
      ]
    },
    {
      "id": "insurance-expert-c",
      "name": "李思穎",
      "title": "保險顧問",
      "bio": "保險專家，協助家庭與企業規劃完整的風險防護方案。",
      "tags": ["保險規劃", "風險管理", "家庭保障"],
      "sections": [
        {
          "theme": "家庭保障",
          "links": [
            {
              "title": "人生各階段的保險規劃",
              "description": "從單身到退休的完整保障藍圖",
              "source": "部落格",
              "url": "https://example.com/lee-lifecycle-insurance"
            }
          ]
        }
      ]
    }
  ]
}
```

- [ ] **Step 2: Validate JSON structure**

Run: `node -e "console.log(JSON.parse(require('fs').readFileSync('/Users/iriswen/xuexi_lushang_site/experts.json')).experts.length + ' experts loaded')"`

Expected: `5 experts loaded`

- [ ] **Step 3: Commit experts.json**

```bash
cd /Users/iriswen/xuexi_lushang_site
git add experts.json
git commit -m "data: add expert library initial dataset (5 experts)"
```

---

### Task 2: Add HTML Structure & Bottom-Nav Tab

**Files:**
- Modify: `xuexi_lushang_site/index.src.html` (bottom-nav + tab structure)

**Interfaces:**
- Consumes: `experts.json` structure from Task 1
- Produces: HTML `.tab.experts` section; bottom-nav `<button data-tab="experts">`; JS data object: `window.expertsData` (to be imported)

- [ ] **Step 1: Open index.src.html and locate the bottom-nav section**

Find the section with:
```html
<div class="bottom-nav">
  <button data-tab="market" class="active">...</button>
  <button data-tab="succession">...</button>
  <button data-tab="english">...</button>
</div>
```

- [ ] **Step 2: Add 資源庫 button to bottom-nav**

Add this button before the closing `</div>`:

```html
<button data-tab="experts" title="資源庫">
  <div class="ic">
    <svg class="ic-svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round">
      <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/>
      <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/>
    </svg>
  </div>
  <div style="font-size: .68rem; font-weight: 700;">資源庫</div>
</button>
```

(Icon: open book/library symbol, line-style)

- [ ] **Step 3: Locate where the existing tabs are rendered (`.tab` sections)**

Look for:
```html
<div class="tab active" data-tab-name="market">...</div>
<div class="tab" data-tab-name="succession">...</div>
<div class="tab" data-tab-name="english">...</div>
```

- [ ] **Step 4: Add .tab.experts section after the last existing tab**

Add before `</div>` closing `.wrap`:

```html
<div class="tab" data-tab-name="experts">
  <div class="wrap">
    <div class="subj-head">
      <div class="em">
        <svg class="ic-svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round">
          <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/>
          <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/>
        </svg>
      </div>
      <h2>專家資源庫</h2>
    </div>
    <p class="lead">探索認可專家的精選分析與觀點，深化你在市場、財務與投資領域的理解。</p>
    
    <div id="experts-list"></div>
  </div>
</div>
```

- [ ] **Step 5: Commit tab structure**

```bash
cd /Users/iriswen/xuexi_lushang_site
git add index.src.html
git commit -m "feat: add 資源庫 tab structure and bottom-nav button"
```

---

### Task 3: Add CSS Styling for Expert Cards & Animations

**Files:**
- Modify: `xuexi_lushang_site/index.src.html` (add `<style>` rules)

**Interfaces:**
- Consumes: existing CSS variables (`--primary`, `--card`, `--border`, etc.)
- Produces: `.expert-card`, `.expert-header`, `.expert-expanded`, `.expert-section`, `.expert-link` selectors

- [ ] **Step 1: Locate the existing `<style>` block in index.src.html**

Find the closing `</style>` tag.

- [ ] **Step 2: Add expert card CSS before `</style>`**

```css
  /* ===== EXPERT CARDS ===== */
  .expert-card {
    background: var(--card);
    border: 1.5px solid var(--border);
    border-radius: 16px;
    padding: 1rem 1.1rem;
    margin-bottom: 1rem;
    box-shadow: 0 2px 12px rgba(0,31,77,.05);
    cursor: pointer;
    transition: border-color .15s, box-shadow .15s;
    overflow: hidden;
  }
  .expert-card:hover {
    border-color: var(--primary);
    box-shadow: 0 4px 16px rgba(0,31,77,.12);
  }
  .expert-card.expanded {
    max-height: none;
    overflow: visible;
  }

  .expert-collapsed {
    display: block;
  }
  .expert-card.expanded .expert-collapsed {
    display: none;
  }

  .expert-header-compact {
    font-size: 1rem;
    font-weight: 800;
    color: var(--primary);
    margin-bottom: .3rem;
  }
  .expert-title-compact {
    font-size: .9rem;
    color: var(--text);
    margin-bottom: .5rem;
  }
  .expert-tags {
    display: flex;
    flex-wrap: wrap;
    gap: .4rem;
  }
  .expert-tag {
    font-size: .72rem;
    font-weight: 700;
    color: var(--primary);
    background: var(--active);
    padding: .12rem .6rem;
    border-radius: 20px;
    border: 1px solid var(--border);
  }

  /* ===== EXPANDED CONTENT ===== */
  .expert-expanded {
    display: none;
    padding-top: .8rem;
    margin-top: .8rem;
    border-top: 1px solid var(--border);
  }
  .expert-card.expanded .expert-expanded {
    display: block;
    animation: fadeIn .25s;
  }

  .expert-expanded-header {
    margin-bottom: 1rem;
  }
  .expert-expanded-name {
    font-size: 1.1rem;
    font-weight: 800;
    color: var(--primary);
    margin-bottom: .2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .expert-close-btn {
    background: none;
    border: none;
    color: var(--primary);
    font-size: 1.3rem;
    cursor: pointer;
    padding: 0;
    width: 1.5rem;
    height: 1.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .expert-bio {
    font-size: .85rem;
    color: var(--muted);
    line-height: 1.6;
    margin-bottom: .6rem;
  }

  .expert-section {
    margin-bottom: 1.2rem;
  }
  .expert-section:last-child {
    margin-bottom: 0;
  }
  .expert-section-theme {
    font-size: 1rem;
    font-weight: 800;
    color: var(--primary);
    margin-bottom: .6rem;
    padding-bottom: .4rem;
    border-bottom: 1px dashed var(--border);
  }

  .expert-link {
    display: flex;
    flex-direction: column;
    margin-bottom: .8rem;
    padding-bottom: .8rem;
    border-bottom: 1px solid #f0f0f0;
  }
  .expert-link:last-child {
    border-bottom: none;
    margin-bottom: 0;
    padding-bottom: 0;
  }

  .expert-link-title {
    font-size: .95rem;
    font-weight: 700;
    color: var(--primary);
    text-decoration: none;
    margin-bottom: .25rem;
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: .5rem;
  }
  .expert-link-title:hover {
    text-decoration: underline;
  }
  .expert-link-arrow {
    flex-shrink: 0;
    width: 1.1em;
    height: 1.1em;
    margin-top: .15rem;
  }

  .expert-link-meta {
    font-size: .8rem;
    color: var(--muted);
    margin-bottom: .3rem;
  }
  .expert-link-source {
    display: inline-block;
    background: var(--active);
    color: var(--primary);
    padding: .08rem .5rem;
    border-radius: 12px;
    font-size: .72rem;
    font-weight: 700;
    margin-right: .4rem;
  }

  .expert-link-desc {
    font-size: .82rem;
    color: var(--text);
    line-height: 1.5;
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  /* ===== MOBILE RESPONSIVE ===== */
  @media (max-width: 600px) {
    .expert-card {
      padding: .85rem .9rem;
      margin-bottom: .8rem;
    }
    .expert-link-title {
      flex-direction: column;
    }
    .expert-link-arrow {
      display: none;
    }
  }
```

- [ ] **Step 3: Commit CSS**

```bash
cd /Users/iriswen/xuexi_lushang_site
git add index.src.html
git commit -m "style: add expert card styling and animations"
```

---

### Task 4: Implement JavaScript Logic (Tab Switching, Expand/Collapse, Rendering)

**Files:**
- Modify: `xuexi_lushang_site/index.src.html` (add/update `<script>` section)

**Interfaces:**
- Consumes: `experts.json` data (loaded as `window.expertsData`)
- Produces: 
  - `window.renderExpertsList(data)` — renders expert cards
  - `window.toggleExpertCard(cardId)` — expand/collapse single card
  - Tab-switch event handler for 資源庫 tab

- [ ] **Step 1: Locate the existing `<script>` section near the end of index.src.html**

Find where tab-switching is currently implemented (look for `data-tab` button listeners).

- [ ] **Step 2: Add experts data loading at the top of the script**

Add after page load or near tab initialization:

```javascript
// Load experts data
let expertsData = [];
fetch('experts.json')
  .then(res => res.json())
  .then(data => {
    expertsData = data.experts;
  })
  .catch(err => console.error('Failed to load experts.json', err));
```

- [ ] **Step 3: Add renderExpertsList function**

```javascript
function renderExpertsList(experts) {
  const container = document.getElementById('experts-list');
  if (!container) return;
  
  container.innerHTML = experts.map(expert => `
    <div class="expert-card" id="expert-${expert.id}">
      <div class="expert-collapsed">
        <div class="expert-header-compact">${expert.name}</div>
        <div class="expert-title-compact">${expert.title}</div>
        <div class="expert-tags">
          ${expert.tags.map(tag => `<div class="expert-tag">#${tag}</div>`).join('')}
        </div>
      </div>
      
      <div class="expert-expanded">
        <div class="expert-expanded-header">
          <div class="expert-expanded-name">
            <span>${expert.name}</span>
            <button class="expert-close-btn" data-close="${expert.id}">✕</button>
          </div>
          <div class="expert-title-compact">${expert.title}</div>
          <div class="expert-bio">${expert.bio}</div>
        </div>
        
        ${expert.sections.map(section => `
          <div class="expert-section">
            <div class="expert-section-theme">${section.theme}</div>
            ${section.links.map(link => `
              <div class="expert-link">
                <a href="${link.url}" target="_blank" class="expert-link-title">
                  <span>${link.title}</span>
                  <svg class="expert-link-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M7 17L17 7M17 7H7M17 7V17"/>
                  </svg>
                </a>
                <div class="expert-link-meta">
                  <span class="expert-link-source">${link.source}</span>
                  ${link.description}
                </div>
              </div>
            `).join('')}
          </div>
        `).join('')}
      </div>
    </div>
  `).join('');
  
  // Add click handlers
  container.querySelectorAll('.expert-card').forEach(card => {
    const cardId = card.id.replace('expert-', '');
    card.addEventListener('click', (e) => {
      if (!e.target.closest('.expert-close-btn')) {
        toggleExpertCard(cardId);
      }
    });
  });
  
  // Add close button handlers
  container.querySelectorAll('.expert-close-btn').forEach(btn => {
    btn.addEventListener('click', (e) => {
      e.stopPropagation();
      toggleExpertCard(btn.dataset.close);
    });
  });
}

function toggleExpertCard(expertId) {
  const card = document.getElementById(`expert-${expertId}`);
  if (card) {
    card.classList.toggle('expanded');
  }
}
```

- [ ] **Step 4: Add 資源庫 tab click handler**

Find existing tab button click listeners and add:

```javascript
// Tab switching (add if not present)
document.querySelectorAll('.bottom-nav button').forEach(btn => {
  btn.addEventListener('click', () => {
    const tabName = btn.dataset.tab;
    
    // Hide all tabs
    document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
    // Remove active state from all buttons
    document.querySelectorAll('.bottom-nav button').forEach(b => b.classList.remove('active'));
    
    // Show selected tab
    const activeTab = document.querySelector(`.tab[data-tab-name="${tabName}"]`);
    if (activeTab) activeTab.classList.add('active');
    btn.classList.add('active');
    
    // Render experts list when 資源庫 tab is activated
    if (tabName === 'experts' && expertsData.length > 0) {
      renderExpertsList(expertsData);
    }
  });
});
```

- [ ] **Step 5: Initialize experts list on page load**

Add to page initialization (after DOM ready):

```javascript
document.addEventListener('DOMContentLoaded', () => {
  // Wait a moment for experts.json to load
  setTimeout(() => {
    if (expertsData.length > 0 && document.querySelector('.tab[data-tab-name="experts"]')) {
      renderExpertsList(expertsData);
    }
  }, 500);
});
```

- [ ] **Step 6: Commit JS logic**

```bash
cd /Users/iriswen/xuexi_lushang_site
git add index.src.html
git commit -m "feat: add expert card rendering and expand/collapse logic"
```

---

### Task 5: Test in Browser (Chrome/Safari Mobile Simulation)

**Files:**
- Test: `index.src.html` (via browser)

**Interfaces:**
- Consumes: rendered HTML from Tasks 2–4
- Validates: all success criteria from spec

- [ ] **Step 1: Start a local server**

```bash
cd /Users/iriswen/xuexi_lushang_site
python3 -m http.server 8000
```

Open browser to `http://localhost:8000`

- [ ] **Step 2: Test 資源庫 tab navigation**

- Click 資源庫 button in bottom nav
- Verify: 專家資源庫 heading appears
- Verify: All 5 expert cards render with name, title, tags

- [ ] **Step 3: Test card expand/collapse**

- Click on first expert card (吳嘉隆)
- Verify: Card expands, showing bio, sections, links
- Verify: Close button (✕) appears
- Click close button
- Verify: Card collapses back to compact view

- [ ] **Step 4: Test link opening**

- Expand an expert card
- Click on any link title
- Verify: Link opens in new tab (target="_blank" works)
- Return to original tab

- [ ] **Step 5: Test mobile responsiveness**

- Open DevTools (F12)
- Toggle device toolbar (Cmd+Shift+M)
- Select iPhone 12 or similar
- Verify: Cards scale properly, no horizontal overflow
- Verify: Tags wrap on multiple lines
- Tap to expand/collapse works on touch

- [ ] **Step 6: Test tab switching**

- Click 市場, 傳承, 英文 tabs to verify they still work
- Click 資源庫 tab again
- Verify: Cards re-render correctly

- [ ] **Step 7: Test styling consistency**

- Verify colors match existing site (`--primary` aqua, gold accents)
- Verify fonts match (PingFang TC, sans-serif)
- Verify spacing/padding consistent with plan-block style
- Verify shadows are subtle (light, not heavy)

---

### Task 6: Build & Encrypt with StatiCrypt, Final Commit

**Files:**
- Modify: `index.html` (StatiCrypt encrypted output)

**Interfaces:**
- Consumes: `index.src.html` (with all JS/CSS/HTML from Tasks 2–4)
- Produces: `index.html` (encrypted)

- [ ] **Step 1: Check if build script exists**

```bash
ls -la /Users/iriswen/xuexi_lushang_site/ | grep -E "build|encrypt|script"
```

If no script exists, look for `.staticrypt.json`:

```bash
cat /Users/iriswen/xuexi_lushang_site/.staticrypt.json
```

- [ ] **Step 2: Run StatiCrypt build**

If using staticrypt CLI:

```bash
cd /Users/iriswen/xuexi_lushang_site
staticrypt index.src.html -p $(cat ~/.xuexi_lushang_pw 2>/dev/null || echo "password") -o index.html
```

(Adjust password source as needed; check `.staticrypt.json` for config)

Or if using a build script:

```bash
cd /Users/iriswen/xuexi_lushang_site
./build.sh  # or npm run build, or similar
```

- [ ] **Step 3: Verify index.html was generated and is encrypted**

```bash
head -20 /Users/iriswen/xuexi_lushang_site/index.html | grep -i "staticrypt\|encrypted"
```

Expected: Should see references to StatiCrypt or encryption (not plain HTML)

- [ ] **Step 4: Test encrypted version locally**

Open browser to `http://localhost:8000/index.html` (encrypted version)
- Enter password
- Verify: Site loads and 資源庫 tab works as before

- [ ] **Step 5: Commit all changes**

```bash
cd /Users/iriswen/xuexi_lushang_site
git add index.html experts.json
git commit -m "build: encrypt with StatiCrypt; add expert library feature

- Add 資源庫 tab with 5 expert profiles
- Implement expand/collapse card interactions
- Curate initial expert data (吳嘉隆, 張明輝, 陳家琪, 王建民, 李思穎)
- Style consistent with existing 學習路上 design
- Responsive on mobile"
```

- [ ] **Step 6: Verify git log**

```bash
cd /Users/iriswen/xuexi_lushang_site
git log --oneline -5
```

Expected: Latest commit shows expert library feature merged

---

## Summary of Changes

| File | Type | Description |
|------|------|-------------|
| `experts.json` | Create | 5–6 expert profiles with curated content |
| `index.src.html` | Modify | Add 資源庫 tab, HTML structure, CSS, JS logic (~300 lines) |
| `index.html` | Regenerate | Encrypted via StatiCrypt |

**Success Criteria Checklist:**
- ✓ 資源庫 tab appears in bottom nav
- ✓ Cards render (name, title, tags)
- ✓ Click to expand → shows sections & links
- ✓ Links open in new tab
- ✓ Mobile responsive
- ✓ Styling matches site theme
- ✓ `experts.json` versioned
- ✓ All committed to git

