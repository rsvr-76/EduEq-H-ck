# End-to-End Solution Workflow

## 🎯 User Journey: From Blank Page to Funder-Ready LFA

### **Entry Point**
User lands on platform → Sees two options:
1. **Start with a Pattern** (recommended)
2. **Start from Scratch**

---

## 📍 Step-by-Step Flow

### **Step 1: Theme Selection**
**Screen:** Landing page with theme cards

**User Action:**
- Selects theme: `FLN` (or Custom)
- If FLN → sees 10-12 pattern options

**System Action:**
- Loads theme-specific patterns
- Initializes wizard state
- Shows progress bar (0/8 steps)

**Data Stored:**
```js
{ theme: "FLN", pattern: null }
```

---

### **Step 2: Pattern Selection** (Optional)
**Screen:** Pattern library browser

**User Action:**
- Browses patterns (e.g., "Reading Comprehension Boost")
- Reads "Why this pattern works"
- Clicks "Use This Pattern"

**System Action:**
- Pre-fills outcomes, indicators, stakeholders
- Advances to Step 3
- Progress: 1/8

**Data Stored:**
```js
{
  pattern: "fln_reading_boost",
  problem: "Students in Grades 3-5 cannot read grade-level text",
  outcomes: ["Reading at grade level"],
  indicators: ["ORF score ≥ 45 wpm"]
}
```

---

### **Step 3: Problem Statement**
**Screen:** Text area with examples

**User Action:**
- Edits pre-filled problem (if pattern used)
- OR writes from scratch
- Minimum 50 characters

**System Action:**
- Validates length
- Auto-saves to localStorage
- Shows toast: "Progress saved"
- Progress: 2/8

**Validation:**
- ❌ Error if < 50 chars
- 💡 Hint: "Describe the specific gap in student learning or system functioning"

---

### **Step 4: Student Outcomes**
**Screen:** Multi-select + custom input

**User Action:**
- Selects from common outcomes (if pattern)
- OR adds custom outcomes
- Minimum 1 outcome required

**System Action:**
- Validates min count
- Progress: 3/8

**Validation:**
- ❌ Error if 0 outcomes
- 💡 Hint: "What specific change do you want to see for students?"

---

### **Step 5: Outcome Indicators**
**Screen:** Searchable indicator library

**User Action:**
- For each outcome, selects indicators
- Searches library (20-30 indicators)
- Can add custom indicators

**System Action:**
- Links indicators to outcomes
- Validates each outcome has ≥1 indicator
- Progress: 4/8

**Validation:**
- ❌ Error: "Outcome 'Reading at grade level' has no indicator"
- 💡 Hint: "Most FLN programs track this using ORF scores or ASER levels"

**Data Stored:**
```js
{
  outcomes: [
    {
      name: "Reading at grade level",
      indicators: ["ORF score ≥ 45 wpm", "ASER Reading Level 2+"]
    }
  ]
}
```

---

### **Step 6: Intervention Approach**
**Screen:** Pattern picker + description field

**User Action:**
- Selects intervention type (e.g., "Teacher coaching")
- Describes methodology

**System Action:**
- Progress: 5/8

---

### **Step 7: Stakeholder Mapping** (CORE FEATURE)
**Screen:** Hierarchical accordion view

**User Action:**
1. Expands "SCHOOL" level
2. Clicks "Add Stakeholder ▼"
3. Selects "Teacher" from dropdown
4. Types practice: "Daily 30-min FLN instruction"
5. Repeats for other levels (Cluster, Block)

**System Action:**
- Validates min 2 system levels
- Progress: 6/8

**UI Structure:**
```
┌─────────────────────────────────────┐
│ DISTRICT   [Add Stakeholder ▼]     │
│   ○ DEO          [Remove]           │
├─────────────────────────────────────┤
│ BLOCK      [Add Stakeholder ▼]     │
│   ○ BRP          [Remove]           │
│     └─ Practice: [Monthly visits__] │
├─────────────────────────────────────┤
│ SCHOOL     [Add Stakeholder ▼]     │
│   ○ Teacher      [Remove]           │
│     └─ Practice: [Daily FLN_____]   │
└─────────────────────────────────────┘
```

**Validation:**
- ❌ Error: "Stakeholder 'Teacher' has no practice change"
- 💡 Hint: "Successful programs typically include daily FLN instruction or activity-based learning"

---

### **Step 8: Tracking Plan**
**Screen:** Indicator-practice alignment matrix

**User Action:**
- Maps practices to tracking indicators
- Ensures all practices have measurement

**System Action:**
- Calculates quality score
- Determines readiness level
- Progress: 8/8 ✅

**Validation:**
- ⚠️ Warning: "Practice 'Daily FLN' has no tracking indicator"
- 💡 Hint: "Consider tracking through observation checklists or self-reported logs"

---

## 🎯 Validation & Readiness

### **Continuous Validation Panel** (Right Sidebar)

**Real-time Display:**
```
┌─────────────────────────────────┐
│ Quality Score: 85%              │
│ 🟩 Funder-Ready                 │
│                                 │
│ ✅ All steps complete           │
│ ✅ All outcomes have indicators │
│ ⚠️ 1 practice needs tracking    │
│                                 │
│ 💡 Consider adding observation  │
│    checklists for teacher       │
│    practices                    │
└─────────────────────────────────┘
```

**Score Calculation:**
```
Score = (completeness × 40%) + (logic × 30%) + (coherence × 30%)
```

**Readiness Levels:**
- 🟥 **Draft** (0-39%): "Keep building"
- 🟨 **Review-Ready** (40-69%): "Internal review"
- 🟩 **Funder-Ready** (70-89%): "External sharing"
- 🟦 **Implementation-Ready** (90-100%): "Clear path to action"

**If score ≥ 90%:**
- 🎉 Confetti animation (one-time)
- Unlocks export button

---

## 📄 Export & Download

### **Final Screen: Review & Export**

**User Action:**
- Reviews complete LFA summary
- Clicks "Export PDF"

**System Action:**
1. Shows loading: "Generating your LFA document..."
2. Creates PDF with sections:
   - Cover page
   - Problem statement
   - Theory of change
   - Stakeholder map
   - Measurement framework
   - Appendix
3. Downloads: `LFA_FLN_Reading_2026-01-17.pdf`

**PDF Contents:**
- Professional formatting
- Tables for indicators
- Visual stakeholder hierarchy
- Quality score badge
- Readiness level stamp

---

## 🔄 Save & Resume

**Auto-Save Behavior:**
- Saves to localStorage on every step change
- Shows toast: "Progress saved"
- No login required

**Resume Flow:**
- User returns → sees "Continue where you left off?"
- Loads saved state
- Jumps to last incomplete step

---

## 🎬 Demo Flow (5 Minutes)

### **Minute 1: Hook**
"Designing a program takes 4 months. Watch us do it in 4 minutes."

### **Minute 2: Pattern Selection**
- Select FLN theme
- Choose "Reading Comprehension Boost" pattern
- Show pre-filled data

### **Minute 3: Stakeholder Mapping**
- Add Teacher → "Daily 30-min FLN"
- Add CRP → "Monthly classroom visits"
- Show visual hierarchy

### **Minute 4: Validation**
- Trigger validation
- Show red flags → fix → green
- Score jumps to 92%
- 🎉 Confetti + "Implementation-Ready"

### **Minute 5: Export**
- Click export
- Download PDF live
- Open and show funder-ready document

**Closing:** "60% faster, zero consultants, funder-ready."

---

## 🧠 Key Differentiators

1. **Pattern Library** → Reduces blank-page fear
2. **Stakeholder Mapping** → System-level thinking
3. **Coaching Hints** → Supportive, not punitive
4. **Readiness Levels** → Real-world context
5. **One-Click Export** → Tangible output

**Value Proposition:**
> "From vague idea to validated LFA document in one sitting"
