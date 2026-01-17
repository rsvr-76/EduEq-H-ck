# Code Implementation Checklist

## 🎯 Priority Ranking

### ✅ DO NOW (Critical Path - Days 1-3)

- [ ] **#2: Validation Hints** (Low effort, high impact)
  - File: `lib/validation.ts`
  - Add `hint` field to issue objects
  - File: `components/ValidationPanel.tsx`
  - Render 💡 hints alongside ❌ errors
  - **Time:** 1-2 hours
  - **Impact:** +15% perceived intelligence

- [ ] **#3: Readiness Level** (Very low effort, high impact)
  - File: `lib/validation.ts`
  - Add `getReadiness(score)` function
  - Update store schema: `validation.readiness`
  - File: `components/ValidationPanel.tsx`
  - Display readiness badge (Draft/Review/Funder)
  - **Time:** 30 mins
  - **Impact:** +20% conceptual clarity

- [ ] **#4: Simplified Stakeholder UI** (Medium effort, saves time)
  - File: `components/StakeholderMap.tsx`
  - Remove: drag-drop handlers
  - Add: dropdown button + remove buttons
  - Add: accordion collapse/expand
  - **Time:** 3-4 hours
  - **Impact:** -40% UI complexity, -50% bug risk

---

### 🟡 DO IF TIME (Polish - Day 4)

- [ ] **#1: Config Layer** (Low effort, technical points)
  - File: `public/config/system-config.json`
  - Create config schema (themes, validation rules, stakeholder levels)
  - Files: `lib/validation.ts`, `components/StepWizard.tsx`
  - Replace hardcoded constants with config imports
  - **Time:** 2 hours
  - **Impact:** +4 technical points (configurability)

- [ ] **#7: Auto-Save Toast** (Low effort, UX polish)
  - Install: `sonner` or `react-hot-toast`
  - File: `store/lfaStore.ts`
  - Add localStorage save + toast on state change
  - **Time:** 30 mins
  - **Impact:** +10% perceived reliability

- [ ] **#6: Loading States** (Very low effort, polish)
  - Files: `components/PDFExport.tsx`, `components/ValidationPanel.tsx`
  - Replace "Loading..." with step-wise messages
  - **Time:** 15 mins
  - **Impact:** +5% polish

---

### ⭐ OPTIONAL (Day 5 if ahead)

- [ ] **#5: Confetti Celebration** (Very low effort, delight)
  - Install: `canvas-confetti`
  - File: `components/ValidationPanel.tsx`
  - Trigger once when score ≥ 90%
  - **Time:** 15 mins
  - **Impact:** +10% delight factor

---

### ❌ DO NOT BUILD

- [ ] ❌ AI/LLM integration
- [ ] ❌ Real-time collaboration
- [ ] ❌ Backend APIs
- [ ] ❌ User authentication
- [ ] ❌ Multi-language support
- [ ] ❌ Analytics dashboards

---

## 📋 Git-Style Implementation Order

```bash
# Day 1-2: Core Features
git commit -m "feat: add validation hints to engine"
git commit -m "feat: add readiness level framework"
git commit -m "refactor: simplify stakeholder mapper UI"

# Day 3-4: Polish (if time permits)
git commit -m "feat: add config-driven validation rules"
git commit -m "feat: add auto-save with toast notifications"
git commit -m "polish: improve loading state messages"

# Day 5: Optional delight
git commit -m "feat: add confetti celebration for 90% score"
```

---

## 🧾 Effort vs Impact Matrix

| Feature | Effort | Impact | Priority |
|---------|--------|--------|----------|
| Validation hints | Low | High | ✅ NOW |
| Readiness level | Very Low | High | ✅ NOW |
| Simplified stakeholder UI | Medium | High (time savings) | ✅ NOW |
| Config layer | Low | Medium | 🟡 IF TIME |
| Auto-save toast | Low | Medium | 🟡 IF TIME |
| Loading states | Very Low | Low | 🟡 IF TIME |
| Confetti | Very Low | Low | ⭐ OPTIONAL |

---

## 📦 Dependencies to Install

```bash
# Critical (install Day 1)
npm install zustand lucide-react jspdf

# Polish (install Day 4 if time)
npm install sonner canvas-confetti
```

---

## 🎯 Total Complexity Budget

- **Base MVP:** 100% complexity
- **Code changes added:** +10-15% complexity
- **Time saved (simplified UI):** -40% on stakeholder mapper
- **Net complexity:** ~75-85% of original plan
- **Perceived maturity:** +40%

**Result:** More deliverable, more impressive, less risky.
