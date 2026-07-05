# 🧠 Blackboard — Smart Classroom & Timetable Scheduler

A browser-based, constraint-aware timetable generator for schools and colleges. Add your teachers, classrooms, and sections, and Blackboard automatically builds a clash-free weekly schedule — no server, database, or installation required.

**🔗 [Open the live app](https://sathyam2228-svg.github.io/smart-classroom-scheduler/)**
*(Update this link to match your exact repo name once it's live under Settings → Pages.)*

---

## ✨ Features

- **One-click generation** — builds a full weekly timetable in seconds using a constraint-satisfaction (CSP) scheduling algorithm
- **Clash-free by design** — no teacher is double-booked, and no lab/special room is assigned to two classes at once
- **Three views** — inspect the schedule *by section*, *by teacher*, or *by room* to catch conflicts from every angle
- **Manual overrides** — click any cell to reassign a subject, with live conflict warnings as you edit
- **Special rooms** — mark subjects like Science or Computer Science as needing a lab, and the scheduler reserves it automatically
- **Configurable week** — choose 5-day or 6-day weeks, set periods per day, and place a daily break
- **Sample data included** — load a ready-made two-section example to see it working instantly
- **Autosave** — your setup persists automatically between sessions
- **Print / export to PDF** — generate a clean, printable copy of any timetable view
- **Zero dependencies** — a single HTML file with vanilla JavaScript; works fully offline

---

## 🚀 Getting Started

No installation needed.

1. Download `smart-classroom-scheduler.html` from this repository
2. Open it in any modern browser (Chrome, Safari, Edge, Firefox)
3. Click **Load sample data** to explore the app immediately, or start adding your own teachers, rooms, and sections
4. Click **Generate timetable**

That's it — the whole app runs client-side, in the file itself.

### Optional: host it live with GitHub Pages

1. Go to your repo's **Settings → Pages**
2. Under **Build and deployment**, choose **Deploy from a branch**, pick `main`, and save
3. GitHub will publish the file at a public URL you can share with anyone

---

## 🗂️ How It Works

### Data model

| Entity | Description |
|---|---|
| **Teacher** | A staff member who can be assigned to subjects |
| **Room** | A classroom or lab; labs are flagged as *special* rooms |
| **Section** | A class/batch (e.g. *Grade 9 - A*) with a home classroom |
| **Subject** | Belongs to a section; has an assigned teacher, a weekly period count, and an optional special room requirement |

### Scheduling algorithm

Each subject's weekly periods become individual **tasks** (e.g. Mathematics × 6/week → 6 separate tasks). The scheduler then repeatedly:

1. Shuffles the task order and the pool of available day/period slots
2. Places each task into the first slot where **all three constraints hold**:
   - the section is free at that slot
   - the teacher is free at that slot
   - the room (home or special) is free at that slot
3. Tracks how many tasks couldn't be placed without breaking a constraint
4. Repeats this randomized process (up to 250 attempts) and keeps the **best result** — ideally one with zero unplaced tasks

This is a classic **randomized greedy CSP (Constraint Satisfaction Problem)** approach — the same family of techniques taught in AI and Design & Analysis of Algorithms courses for timetabling problems. Any periods that remain unplaced after all attempts are flagged clearly, so they can be resolved manually with a click.

---

## 🖥️ Tech Stack

- **HTML5 / CSS3** — layout, theming, and print styles
- **Vanilla JavaScript (ES6+)** — app logic and the scheduling algorithm, no frameworks
- **Google Fonts** — Caveat, IBM Plex Sans, IBM Plex Mono
- **Client-side storage** — autosaves app state so data persists across sessions

No build tools, no npm install, no backend — the entire project is one portable file.

---

## 📁 Project Structure

```
smart-classroom-scheduler/
├── smart-classroom-scheduler.html   # the entire application
└── README.md                        # this file
```

---

## 🛣️ Possible Extensions

Ideas for taking this further as a project submission or personal enhancement:

- Export the generated timetable to Excel/CSV
- Add teacher weekly workload limits and preferred free periods
- Support drag-and-drop swapping between two filled cells
- Add student-elective handling for senior/college timetables
- Multi-week or semester-level scheduling

---

## 📄 License

This project is open for personal, academic, and educational use. Feel free to fork it, adapt it, and use it for your coursework or institution.

---

## 🙋 About

Built as a mini-project demonstrating constraint-based scheduling applied to a real, everyday problem: making a school or college timetable without conflicts.
