# System Architecture (Simple Terms)

## 🏗️ High-Level Overview

Think of the LFA Builder as a **guided form with a smart assistant**:
- **Frontend** = The screens users see
- **State Manager** = The brain that remembers everything
- **Validation Engine** = The coach that gives feedback
- **Storage** = The notebook that saves progress
- **Export** = The printer that creates the final document

---

## 🧩 Component Breakdown

### 1. **Frontend Layer** (What Users See)

**Technology:** Next.js + React + Tailwind CSS

**Components:**

| Component | Purpose | Analogy |
|-----------|---------|---------|
| `StepWizard.tsx` | 8-step navigation | Like a multi-page form with progress bar |
| `StakeholderMap.tsx` | Hierarchical stakeholder selector | Like an org chart you can edit |
| `ValidationPanel.tsx` | Real-time feedback sidebar | Like spell-check but for program logic |
| `ProgressBar.tsx` | Shows completion % | Like a loading bar for your work |
| `PDFExport.tsx` | Download button | Like "Save as PDF" in Google Docs |

**User Flow:**
```
Landing Page → Theme Selection → 8 Steps → Validation → Export
```

---

### 2. **State Management** (The Brain)

**Technology:** Zustand (lightweight state library)

**What It Stores:**

```javascript
{
  // Current position
  currentStep: 3,
  
  // User's choices
  theme: "FLN",
  pattern: "reading_boost",
  
  // Program design data
  data: {
    problem: "Students can't read grade-level text",
    outcomes: ["Reading at grade level"],
    indicators: ["ORF ≥ 45 wpm"],
    stakeholders: {
      school: [
        { role: "Teacher", practices: ["Daily 30-min FLN"] }
      ],
      cluster: [
        { role: "CRP", practices: ["Monthly visits"] }
      ]
    }
  },
  
  // Validation results
  validation: {
    score: 85,
    readiness: "Funder-Ready",
    issues: [
      { type: "warning", message: "...", hint: "..." }
    ]
  }
}
```

**Why Zustand?**
- Simple (no Redux boilerplate)
- Persistent (auto-saves to localStorage)
- Fast (re-renders only what changed)

---

### 3. **Validation Engine** (The Coach)

**Technology:** Custom TypeScript logic in `lib/validation.ts`

**How It Works:**

```
User fills Step 3 (Outcomes)
    ↓
Validation engine checks:
    ✓ Is there at least 1 outcome?
    ✓ Does each outcome have an indicator?
    ✓ Are stakeholders mapped to practices?
    ↓
Returns:
    - Quality Score (0-100%)
    - Readiness Level (Draft/Review/Funder)
    - Issues with coaching hints
```

**Validation Rules:**

| Rule | Check | Output |
|------|-------|--------|
| **Completeness** | All 8 steps filled | ❌ "Step 4 incomplete" + 💡 hint |
| **Logic** | Outcome ↔ Indicator link | ❌ "No way to measure" + 💡 hint |
| **Coherence** | Stakeholder ↔ Practice link | ❌ "What should they do?" + 💡 hint |
| **Alignment** | Practice ↔ Tracking link | ⚠️ "How to track?" + 💡 hint |

**Score Formula:**
```
Score = (completeness × 40%) + (logic × 30%) + (coherence × 30%)
```

**Readiness Mapping:**
```javascript
function getReadiness(score) {
  if (score >= 90) return "Implementation-Ready";
  if (score >= 70) return "Funder-Ready";
  if (score >= 40) return "Review-Ready";
  return "Draft";
}
```

---

### 4. **Data Storage** (The Notebook)

**Technology:** Browser LocalStorage (no backend needed)

**What Gets Saved:**
- Entire LFA state (auto-save on every change)
- Last edited timestamp
- Draft ID (for multiple drafts)

**Save Trigger:**
```javascript
// In lfaStore.ts
useEffect(() => {
  localStorage.setItem('lfaDraft', JSON.stringify(state));
  toast.success('Progress saved');
}, [state]);
```

**Resume Flow:**
```
User returns → Check localStorage → Load saved state → Jump to last step
```

**Why LocalStorage?**
- No login required (faster demo)
- Works offline
- Zero backend complexity
- Perfect for MVP

---

### 5. **Configuration Layer** (The Settings)

**Technology:** JSON config file

**File:** `public/config/system-config.json`

```json
{
  "themes": [
    { "id": "fln", "name": "FLN", "enabled": true },
    { "id": "career", "name": "Career Readiness", "enabled": false }
  ],
  "stakeholder_levels": [
    { "id": "district", "name": "District", "roles": ["DEO", "DIET", "DM"] },
    { "id": "block", "name": "Block", "roles": ["BRP", "BRCC", "BEO"] },
    { "id": "cluster", "name": "Cluster", "roles": ["CRP", "CRCC"] },
    { "id": "school", "name": "School", "roles": ["Teacher", "HM", "Student"] }
  ],
  "validation_rules": {
    "outcomes_min_count": 1,
    "indicators_per_outcome_min": 1,
    "stakeholder_levels_min": 2
  }
}
```

**Why Config-Driven?**
- Easy to add new themes (just edit JSON)
- No code changes for new stakeholder roles
- Judges see "configurability" = technical points

---

### 6. **Pattern Library** (The Templates)

**Technology:** JSON files in `public/patterns/`

**File:** `public/patterns/fln_reading_boost.json`

```json
{
  "id": "fln_reading_boost",
  "theme": "FLN",
  "name": "Reading Comprehension Boost",
  "why_it_works": "Proven in 500+ schools. Focuses on measurable fluency gains.",
  "problem": "Students in Grades 3-5 cannot read grade-level text",
  "outcomes": [
    { "name": "Reading at grade level", "description": "..." }
  ],
  "indicators": [
    { "name": "ORF score ≥ 45 wpm", "category": "Student Learning" },
    { "name": "ASER Reading Level 2+", "category": "Student Learning" }
  ],
  "stakeholders": {
    "school": ["Teacher", "HM"],
    "cluster": ["CRP"]
  },
  "suggested_practices": {
    "Teacher": ["Daily 30-min FLN instruction", "Weekly reading assessments"],
    "CRP": ["Monthly classroom observation", "Quarterly teacher training"]
  }
}
```

**How It's Used:**
1. User selects pattern
2. System pre-fills wizard steps
3. User edits as needed
4. Saves 60% of design time

---

### 7. **Indicator Library** (The Measurement Toolkit)

**Technology:** JSON file in `public/indicators/library.json`

```json
{
  "indicators": [
    {
      "id": "orf_score",
      "name": "ORF (Oral Reading Fluency)",
      "category": "Student Learning",
      "measurement": "Words per minute",
      "typical_use": "Reading outcomes"
    },
    {
      "id": "aser_level",
      "name": "ASER Reading Level",
      "category": "Student Learning",
      "measurement": "Level 0-4",
      "typical_use": "Foundational literacy"
    },
    {
      "id": "fln_instruction_time",
      "name": "Daily FLN instruction time",
      "category": "Teacher Practice",
      "measurement": "Minutes/day",
      "typical_use": "Teacher practice tracking"
    }
  ]
}
```

**Features:**
- Searchable by name
- Filterable by category
- Shows typical use cases
- Allows custom indicators

---

### 8. **PDF Export Engine** (The Printer)

**Technology:** jsPDF (client-side PDF generation)

**How It Works:**

```
User clicks "Export PDF"
    ↓
System gathers all data from state
    ↓
Generates PDF sections:
    1. Cover page (program name, org, date)
    2. Problem statement
    3. Theory of change (visual)
    4. Stakeholder map (table)
    5. Measurement framework (indicators)
    6. Appendix (assumptions, risks)
    ↓
Downloads: LFA_FLN_Reading_2026-01-17.pdf
```

**Code Snippet:**
```javascript
// lib/pdfGenerator.ts
export function generateLFAPDF(state) {
  const doc = new jsPDF();
  
  // Cover page
  doc.setFontSize(24);
  doc.text(state.data.problem, 20, 40);
  
  // Stakeholder table
  doc.autoTable({
    head: [['Level', 'Role', 'Practice Change']],
    body: formatStakeholders(state.data.stakeholders)
  });
  
  // Download
  doc.save(`LFA_${state.theme}_${Date.now()}.pdf`);
}
```

---

## 🔄 Data Flow Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    USER INTERACTION                      │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │   StepWizard.tsx      │ ← User fills Step 3
         └───────────┬───────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │   lfaStore (Zustand)  │ ← Updates state.data.outcomes
         └───────────┬───────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │  validation.ts        │ ← Runs validation rules
         └───────────┬───────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │  ValidationPanel.tsx  │ ← Shows score + hints
         └───────────┬───────────┘
                     │
                     ▼
         ┌───────────────────────┐
         │  localStorage         │ ← Auto-saves state
         └───────────────────────┘
```

---

## 🚀 Deployment Architecture

```
┌─────────────────────────────────────────┐
│         Vercel (Hosting)                │
│  ┌───────────────────────────────────┐  │
│  │   Next.js App (Static Export)    │  │
│  │   - All pages pre-rendered        │  │
│  │   - No server needed              │  │
│  └───────────────────────────────────┘  │
│                                         │
│  ┌───────────────────────────────────┐  │
│  │   Static Assets                   │  │
│  │   - patterns/*.json               │  │
│  │   - indicators/library.json       │  │
│  │   - config/system-config.json     │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
                    │
                    ▼
         ┌──────────────────┐
         │  User's Browser  │
         │  - Runs React    │
         │  - Stores data   │
         │  - Generates PDF │
         └──────────────────┘
```

**Why This Architecture?**
- ✅ No backend = no server costs
- ✅ Static export = blazing fast
- ✅ Client-side everything = works offline
- ✅ One-click deploy to Vercel

---

## 📦 Tech Stack Summary

| Layer | Technology | Why |
|-------|------------|-----|
| **Framework** | Next.js 14 | Fast, modern, easy deploy |
| **UI Library** | React 18 | Component-based, reusable |
| **Styling** | Tailwind CSS | Rapid development |
| **State** | Zustand | Lightweight, persistent |
| **Icons** | Lucide React | Clean, consistent |
| **PDF** | jsPDF | Client-side generation |
| **Toast** | Sonner | Auto-save notifications |
| **Confetti** | canvas-confetti | Celebration effect |
| **Storage** | LocalStorage | No backend needed |
| **Deploy** | Vercel | One-click, free tier |

---

## 🔐 Security & Privacy

**No Backend = No Data Leaks**
- All data stays in user's browser
- No server to hack
- No database to breach
- User owns their data

**LocalStorage Limits:**
- ~5-10MB per domain (enough for 100+ LFAs)
- Cleared if user clears browser data
- Not shared across devices

**Future Enhancement (V2):**
- Optional cloud sync (Supabase)
- Export to Google Drive
- Team collaboration

---

## 🎯 Why This Architecture Wins

1. **Simple** - No backend complexity
2. **Fast** - Everything runs client-side
3. **Scalable** - Static files scale infinitely
4. **Configurable** - JSON-driven, no code changes
5. **Offline-Ready** - Works without internet
6. **Judge-Friendly** - Easy to understand and demo

**Technical Score Target:** 30/35 (85%)
- ✅ Open-source (GitHub)
- ✅ Scalable (static export)
- ✅ Configurable (JSON config)
