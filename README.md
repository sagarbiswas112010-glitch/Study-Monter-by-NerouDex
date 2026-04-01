import { useState, useEffect, useRef } from "react";

// ── DATA ────────────────────────────────────────────────────────────────────

const SUBJECTS = {
  maths: {
    id: "maths", label: "Maths", icon: "📐", color: "#00f5ff",
    bg: "rgba(0,245,255,0.08)", border: "rgba(0,245,255,0.35)",
    chapters: [
      { id: "m1",  unit: "Number Systems",       name: "Real Numbers" },
      { id: "m2",  unit: "Algebra",               name: "Polynomials" },
      { id: "m3",  unit: "Algebra",               name: "Pair of Linear Equations in Two Variables" },
      { id: "m4",  unit: "Algebra",               name: "Quadratic Equations" },
      { id: "m5",  unit: "Algebra",               name: "Arithmetic Progressions" },
      { id: "m6",  unit: "Coordinate Geometry",   name: "Lines in Two Dimensions" },
      { id: "m7",  unit: "Geometry",              name: "Triangles" },
      { id: "m8",  unit: "Geometry",              name: "Circles" },
      { id: "m9",  unit: "Geometry",              name: "Constructions" },
      { id: "m10", unit: "Trigonometry",          name: "Introduction to Trigonometry" },
      { id: "m11", unit: "Trigonometry",          name: "Trigonometric Identities" },
      { id: "m12", unit: "Trigonometry",          name: "Heights & Distances" },
      { id: "m13", unit: "Mensuration",           name: "Areas Related to Circles" },
      { id: "m14", unit: "Mensuration",           name: "Surface Areas & Volumes" },
      { id: "m15", unit: "Statistics & Prob.",    name: "Statistics" },
      { id: "m16", unit: "Statistics & Prob.",    name: "Probability" },
    ]
  },
  science: {
    id: "science", label: "Science", icon: "🔬", color: "#39ff14",
    bg: "rgba(57,255,20,0.08)", border: "rgba(57,255,20,0.35)",
    chapters: [
      { id: "s1",  unit: "Chemistry", name: "Chemical Reactions & Equations" },
      { id: "s2",  unit: "Chemistry", name: "Acids, Bases & Salts" },
      { id: "s3",  unit: "Chemistry", name: "Metals & Non-metals" },
      { id: "s4",  unit: "Chemistry", name: "Carbon & its Compounds" },
      { id: "s5",  unit: "Biology",   name: "Life Processes" },
      { id: "s6",  unit: "Biology",   name: "Control & Coordination" },
      { id: "s7",  unit: "Biology",   name: "How do Organisms Reproduce?" },
      { id: "s8",  unit: "Biology",   name: "Heredity & Evolution" },
      { id: "s9",  unit: "Biology",   name: "Our Environment" },
      { id: "s10", unit: "Physics",   name: "Light – Reflection & Refraction" },
      { id: "s11", unit: "Physics",   name: "Human Eye & Colourful World" },
      { id: "s12", unit: "Physics",   name: "Electricity" },
      { id: "s13", unit: "Physics",   name: "Magnetic Effects of Electric Current" },
    ]
  },
  sst: {
    id: "sst", label: "Social Science", icon: "🌍", color: "#ff9500",
    bg: "rgba(255,149,0,0.08)", border: "rgba(255,149,0,0.35)",
    chapters: [
      { id: "ss1",  unit: "History",          name: "Rise of Nationalism in Europe" },
      { id: "ss2",  unit: "History",          name: "Nationalism in India" },
      { id: "ss3",  unit: "History",          name: "Making of a Global World" },
      { id: "ss4",  unit: "History",          name: "Age of Industrialisation" },
      { id: "ss5",  unit: "History",          name: "Print Culture & Modern World" },
      { id: "ss6",  unit: "Geography",        name: "Resources & Development" },
      { id: "ss7",  unit: "Geography",        name: "Forest & Wildlife Resources" },
      { id: "ss8",  unit: "Geography",        name: "Water Resources" },
      { id: "ss9",  unit: "Geography",        name: "Agriculture" },
      { id: "ss10", unit: "Geography",        name: "Minerals & Energy Resources" },
      { id: "ss11", unit: "Geography",        name: "Manufacturing Industries" },
      { id: "ss12", unit: "Geography",        name: "Lifelines of National Economy" },
      { id: "ss13", unit: "Pol. Science",     name: "Power Sharing" },
      { id: "ss14", unit: "Pol. Science",     name: "Federalism" },
      { id: "ss15", unit: "Pol. Science",     name: "Gender, Religion & Caste" },
      { id: "ss16", unit: "Pol. Science",     name: "Political Parties" },
      { id: "ss17", unit: "Pol. Science",     name: "Outcomes of Democracy" },
      { id: "ss18", unit: "Economics",        name: "Development" },
      { id: "ss19", unit: "Economics",        name: "Sectors of the Indian Economy" },
      { id: "ss20", unit: "Economics",        name: "Money & Credit" },
      { id: "ss21", unit: "Economics",        name: "Globalisation & the Indian Economy" },
    ]
  },
  english: {
    id: "english", label: "English (Code 101)", icon: "📖", color: "#ff2d9a",
    bg: "rgba(255,45,154,0.08)", border: "rgba(255,45,154,0.35)",
    chapters: [
      { id: "e1",  unit: "Reading Skills",       name: "Unseen Passage – Factual" },
      { id: "e2",  unit: "Reading Skills",       name: "Unseen Passage – Discursive / Literary" },
      { id: "e3",  unit: "Writing Skills",       name: "Formal Letter (Complaint / Enquiry / Order)" },
      { id: "e4",  unit: "Writing Skills",       name: "Article / Speech Writing" },
      { id: "e5",  unit: "Writing Skills",       name: "Descriptive Paragraph" },
      { id: "e6",  unit: "Grammar",              name: "Tenses & Modals" },
      { id: "e7",  unit: "Grammar",              name: "Subject-Verb Agreement & Reported Speech" },
      { id: "e8",  unit: "Grammar",              name: "Clauses & Determiners / Gap Filling" },
      { id: "e9",  unit: "Literature – Prose",   name: "A Letter to God" },
      { id: "e10", unit: "Literature – Prose",   name: "Nelson Mandela: Long Walk to Freedom" },
      { id: "e11", unit: "Literature – Prose",   name: "Two Stories About Flying" },
      { id: "e12", unit: "Literature – Prose",   name: "From the Diary of Anne Frank" },
      { id: "e13", unit: "Literature – Prose",   name: "The Hundred Dresses I & II" },
      { id: "e14", unit: "Literature – Prose",   name: "Glimpses of India" },
      { id: "e15", unit: "Literature – Prose",   name: "Mijbil the Otter" },
      { id: "e16", unit: "Literature – Prose",   name: "Madam Rides the Bus" },
      { id: "e17", unit: "Literature – Prose",   name: "The Sermon at Benares" },
      { id: "e18", unit: "Literature – Prose",   name: "The Proposal (Play)" },
      { id: "e19", unit: "Literature – Poetry",  name: "Dust of Snow / Fire and Ice" },
      { id: "e20", unit: "Literature – Poetry",  name: "A Tiger in the Zoo / How to Tell Wild Animals" },
      { id: "e21", unit: "Literature – Poetry",  name: "The Ball Poem / Amanda / Animals" },
      { id: "e22", unit: "Literature – Poetry",  name: "The Trees / Fog / Custard Dragon / For Anne Gregory" },
    ]
  },
  ai: {
    id: "ai", label: "AI (Code 417)", icon: "🤖", color: "#bf5fff",
    bg: "rgba(191,95,255,0.08)", border: "rgba(191,95,255,0.35)",
    chapters: [
      { id: "a1",  unit: "Part A – Employability", name: "Communication Skills" },
      { id: "a2",  unit: "Part A – Employability", name: "Self-Management Skills" },
      { id: "a3",  unit: "Part A – Employability", name: "ICT Skills" },
      { id: "a4",  unit: "Part A – Employability", name: "Entrepreneurial Skills" },
      { id: "a5",  unit: "Part A – Employability", name: "Green Skills" },
      { id: "a6",  unit: "Part B – Subject Skills", name: "Revisiting AI Project Cycle & Ethics" },
      { id: "a7",  unit: "Part B – Subject Skills", name: "Introduction to Advanced Modelling" },
      { id: "a8",  unit: "Part B – Subject Skills", name: "Evaluating AI Models" },
      { id: "a9",  unit: "Part B – Subject Skills", name: "Introduction to Statistical Data" },
      { id: "a10", unit: "Part B – Subject Skills", name: "Introduction to Computer Vision" },
      { id: "a11", unit: "Part B – Subject Skills", name: "Introduction to Natural Language Processing" },
      { id: "a12", unit: "Part B – Subject Skills", name: "Advance Python for AI" },
    ]
  }
};

const ALERTS = [
  { month: 5, icon: "🚨", color: "#00f5ff",  title: "Pre-Midterm Alert!",   msg: "Pre-Midterm is in July! Finish Science Chemistry + Maths Ch 1–5 by June 30!", cta: "Open Planner" },
  { month: 7, icon: "⚡", color: "#39ff14",  title: "Post-Midterm Alert!",  msg: "Post-Midterm is in September! Complete full Maths + SST by August 31!", cta: "Open Planner" },
  { month: 9, icon: "🎯", color: "#ff9500",  title: "Deadline Alert!",      msg: "Everything done by November for Pre-boards! Revision starts December.", cta: "Check Checklist" },
  { month: 11,icon: "🏁", color: "#ff2d9a",  title: "Pre-Board Season!",    msg: "Pre-Boards in January! Treat them like real Boards. Full revision — GO!", cta: "View Progress" },
  { month: 0, icon: "🎓", color: "#bf5fff",  title: "BOARD SEASON!",        msg: "Boards are HERE. You've prepared all year. Breathe. Trust yourself. YOU GOT THIS! 💪", cta: "Stay Calm 😎" },
];

const QUOTES = [
  "You're absolutely killing it — keep going! 🔥",
  "One chapter today = one nightmare erased! 📚",
  "Future topper loading… 99% ⚡",
  "Boards won't know what hit 'em! 💪",
  "Sleep. Study. Slay. Repeat. 🌟",
  "Every tick on that checklist = pure W. ✅",
  "You're literally smarter than you think. Facts. 🧠",
  "The grind is temporary. The glory is forever. 🏆",
];

const WIREFRAME_SCREENS = [
  {
    id: "splash", icon: "🚀", label: "Splash & Onboarding",
    ascii: [
      "┌──────────────────────────────┐",
      "│                              │",
      "│    📱  STUDY BUDDY 10        │",
      "│   'Ace boards. Stress-free.' │",
      "│                              │",
      "│   [▓▓▓▓▓▓▓▓░░░] Loading...  │",
      "│                              │",
      "│   Enter Name: ____________   │",
      "│   School:     ____________   │",
      "│   Batch:      [ 2026–27 ✓ ] │",
      "│                              │",
      "│      [ GET STARTED  → ]      │",
      "│                              │",
      "└──────────────────────────────┘",
    ],
    note: "Syllabus auto-loads for all 5 subjects on first launch.",
  },
  {
    id: "home", icon: "🏠", label: "Home Dashboard",
    ascii: [
      "┌──────────────────────────────┐",
      "│ 👋 Hey there!  📅 Apr 2026   │",
      "│ 🎓 Boards in: ~330 days      │",
      "├──────────────────────────────┤",
      "│ 🚨 ALERT ─────────────────  │",
      "│  Pre-Midterm in July!        │",
      "│  'Finish Science by June!'   │",
      "│  [ VIEW PLAN ]               │",
      "├──────────────────────────────┤",
      "│ 📊 OVERALL  ████████░░  52%  │",
      "│ 📐 Maths    ████████░░  50%  │",
      "│ 🔬 Science  ██████░░░░  46%  │",
      "│ 🌍 SST      █████░░░░░  38%  │",
      "│ 📖 English  ███████░░░  55%  │",
      "│ 🤖 AI       ████████░░  58%  │",
      "├──────────────────────────────┤",
      "│ 💬 'You're killing it! 🔥'   │",
      "└──────────────────────────────┘",
    ],
    note: "Countdown + live alerts + per-subject bars + daily quote.",
  },
  {
    id: "subject", icon: "📋", label: "Subject Detail",
    ascii: [
      "┌──────────────────────────────┐",
      "│ ← 📐  MATHS                  │",
      "│ Progress: ████████░░  50%    │",
      "│ 8 / 16 chapters done         │",
      "├──────────────────────────────┤",
      "│ ALGEBRA                      │",
      "│ ✅ Polynomials               │",
      "│ ✅ Pair of Linear Equations  │",
      "│ 🔲 Quadratic Equations       │",
      "│ 🔲 Arithmetic Progressions   │",
      "│                              │",
      "│ GEOMETRY                     │",
      "│ ✅ Triangles                 │",
      "│ 🔲 Circles                   │",
      "│ 🔲 Constructions             │",
      "├──────────────────────────────┤",
      "│  [ ▶ Add YouTube Lecture ]   │",
      "└──────────────────────────────┘",
    ],
    note: "Tap any chapter to toggle done/pending. Units auto-group.",
  },
  {
    id: "planner", icon: "📅", label: "Daily Planner",
    ascii: [
      "┌──────────────────────────────┐",
      "│ 📅  DAILY PLANNER            │",
      "├──────────────────────────────┤",
      "│ Subject:  [ Maths ▼ ]        │",
      "│ Chapters left: 8             │",
      "│ Target date: [ Jun 30 📅 ]   │",
      "│ Days left: 60                │",
      "│                              │",
      "│ ✨ PACE SUGGESTION           │",
      "│ ┌────────────────────────┐   │",
      "│ │ 8 chapters · 60 days   │   │",
      "│ │ → Do 1 every 7–8 days  │   │",
      "│ │ → Today: Quad Equations│   │",
      "│ └────────────────────────┘   │",
      "│                              │",
      "│ [ 📌 ADD TO SCHEDULE ]       │",
      "└──────────────────────────────┘",
    ],
    note: "Input target date → app auto-calculates pace per day.",
  },
  {
    id: "youtube", icon: "▶️", label: "YouTube Freak",
    ascii: [
      "┌──────────────────────────────┐",
      "│ ▶️  YOUTUBE FREAK            │",
      "├──────────────────────────────┤",
      "│ Paste YouTube link:          │",
      "│ [ youtube.com/watch?v=___ ]  │",
      "│            [ FETCH ↗ ]       │",
      "│                              │",
      "│ ✅ Fetched via YT API v3     │",
      "│ ─────────────────────────── │",
      "│ 📹 Video: Quadratic Eq.      │",
      "│ Duration:      1h 12m        │",
      "│ + Notes buffer: 3h 00m       │",
      "│ ─────────────────────────── │",
      "│ TOTAL SLOT:    4h 12m        │",
      "│                              │",
      "│ Slot into: [ Today ▼ ]       │",
      "│ [ ✅ ADD TO SCHEDULE ]       │",
      "└──────────────────────────────┘",
    ],
    note: "YT API fetches video duration. App adds 3h buffer automatically.",
  },
  {
    id: "alerts", icon: "🔔", label: "Alerts & Reminders",
    ascii: [
      "┌──────────────────────────────┐",
      "│ 🔔  ALERTS & REMINDERS       │",
      "├──────────────────────────────┤",
      "│ ✅ Pre-Midterm (July)        │",
      "│  'Finish Sci + Maths Ch 1–5  │",
      "│   by June 30!'               │",
      "│                              │",
      "│ 📅 Post-Midterm (September)  │",
      "│  'Complete Maths + SST       │",
      "│   by August 31!'             │",
      "│                              │",
      "│ 📅 Pre-Board Deadline (Nov)  │",
      "│  'Everything done by Nov!    │",
      "│   Revision starts December!' │",
      "│                              │",
      "│ 📅 Pre-Board Exam (Jan 2027) │",
      "│ 🎓 BOARDS (Feb–Mar 2027)     │",
      "└──────────────────────────────┘",
    ],
    note: "Smart pop-ups fire based on current month. Can't be ignored!",
  },
];

const USER_FLOW = [
  { step: 1, icon: "📱", label: "Open App", sub: "Splash → check for month-based exam alert" },
  { step: 2, icon: "🚨", label: "See Alert", sub: "Pop-up: 'Pre-Midterm in July — finish Science by June!'" },
  { step: 3, icon: "📊", label: "Dashboard", sub: "Boards countdown + subject progress bars + quote" },
  { step: 4, icon: "📐", label: "Pick Subject", sub: "Tap Maths → see chapter checklist grouped by unit" },
  { step: 5, icon: "✅", label: "Tick Chapters", sub: "Mark done/pending → progress bar updates live" },
  { step: 6, icon: "▶️", label: "Paste YouTube", sub: "YouTube Freak: paste link → API fetches duration" },
  { step: 7, icon: "⏱️", label: "Time Slotted", sub: "+3h buffer added → scheduled into daily planner" },
  { step: 8, icon: "📅", label: "Planner Updated", sub: "Pace suggestion refreshes: 'Do Ch 4 today!'" },
];

// ── HELPERS ─────────────────────────────────────────────────────────────────

function pct(done, total) { return total ? Math.round((done / total) * 100) : 0; }

function groupByUnit(chapters) {
  const m = {};
  chapters.forEach(c => {
    if (!m[c.unit]) m[c.unit] = [];
    m[c.unit].push(c);
  });
  return m;
}

function daysTillBoards() {
  const target = new Date("2027-02-15");
  const now = new Date();
  return Math.max(0, Math.ceil((target - now) / 86400000));
}

// ── COMPONENTS ───────────────────────────────────────────────────────────────

function NeonBar({ pct: p, color, height = 8 }) {
  return (
    <div style={{ background: "rgba(255,255,255,0.07)", borderRadius: 99, height, overflow: "hidden", width: "100%" }}>
      <div style={{
        height: "100%", borderRadius: 99, width: `${p}%`,
        background: `linear-gradient(90deg, ${color}99, ${color})`,
        boxShadow: `0 0 8px ${color}`,
        transition: "width 0.6s cubic-bezier(.4,0,.2,1)",
      }} />
    </div>
  );
}

function SubjectCard({ subj, done, total, onClick, active }) {
  const p = pct(done, total);
  return (
    <div onClick={onClick} style={{
      background: active ? subj.bg : "rgba(255,255,255,0.03)",
      border: `1px solid ${active ? subj.color : "rgba(255,255,255,0.10)"}`,
      borderRadius: 14, padding: "12px 16px", cursor: "pointer",
      boxShadow: active ? `0 0 18px ${subj.color}44` : "none",
      transition: "all 0.25s",
    }}>
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 8 }}>
        <span style={{ fontFamily: "'Rajdhani', sans-serif", fontWeight: 700, fontSize: 15, color: active ? subj.color : "#ccc" }}>
          {subj.icon} {subj.label}
        </span>
        <span style={{ fontFamily: "'Orbitron', sans-serif", fontSize: 12, color: subj.color, fontWeight: 700 }}>
          {p}%
        </span>
      </div>
      <NeonBar pct={p} color={subj.color} />
      <div style={{ marginTop: 6, fontSize: 11, color: "#888", fontFamily: "'Rajdhani', sans-serif" }}>
        {done}/{total} chapters done
      </div>
    </div>
  );
}

function AlertBanner({ alert, onClose }) {
  return (
    <div style={{
      border: `1px solid ${alert.color}66`, borderRadius: 14,
      background: `linear-gradient(135deg, ${alert.color}15, rgba(0,0,0,0.4))`,
      padding: "14px 16px", position: "relative",
      boxShadow: `0 0 20px ${alert.color}33`,
      animation: "pulseGlow 2s infinite",
    }}>
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
        <div>
          <div style={{ fontFamily: "'Orbitron', sans-serif", fontWeight: 900, fontSize: 13, color: alert.color, marginBottom: 4 }}>
            {alert.icon} {alert.title}
          </div>
          <div style={{ fontFamily: "'Rajdhani', sans-serif", fontSize: 14, color: "#ddd", lineHeight: 1.5 }}>
            {alert.msg}
          </div>
        </div>
        <button onClick={onClose} style={{ background: "none", border: "none", color: "#888", cursor: "pointer", fontSize: 18, marginLeft: 12, lineHeight: 1, padding: 0 }}>×</button>
      </div>
    </div>
  );
}

// ── MAIN SCREEN COMPONENTS ───────────────────────────────────────────────────

function HomeScreen({ checked, setCurrentTab, alertDismissed, setAlertDismissed, activeAlert, quote }) {
  const days = daysTillBoards();
  const subjectList = Object.values(SUBJECTS);
  const totalAll = subjectList.reduce((a, s) => a + s.chapters.length, 0);
  const doneAll  = subjectList.reduce((a, s) => a + s.chapters.filter(c => checked[c.id]).length, 0);
  const overallP = pct(doneAll, totalAll);

  return (
    <div style={{ padding: "0 0 20px" }}>
      {/* Hero */}
      <div style={{
        background: "linear-gradient(135deg, rgba(0,245,255,0.12), rgba(191,95,255,0.12))",
        border: "1px solid rgba(0,245,255,0.2)", borderRadius: 16, padding: "18px 20px", marginBottom: 16,
      }}>
        <div style={{ fontFamily: "'Orbitron', sans-serif", fontSize: 11, color: "#888", letterSpacing: 2, textTransform: "uppercase" }}>CBSE 2026–27 · Boards 2027</div>
        <div style={{ fontFamily: "'Orbitron', sans-serif", fontSize: 22, fontWeight: 900, color: "#fff", marginTop: 6, lineHeight: 1.2 }}>
          📱 STUDY<br/>BUDDY 10
        </div>
        <div style={{ marginTop: 12, display: "flex", alignItems: "center", gap: 10 }}>
          <div style={{ textAlign: "center", background: "rgba(0,245,255,0.12)", borderRadius: 10, padding: "8px 14px", border: "1px solid rgba(0,245,255,0.25)" }}>
      
