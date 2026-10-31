UniTrack
Personal university attendance tracker for Fedi — L1 LIRI B G4 · TD · ISI Kef (Université de Jendouba), Semestre 1.

A dark-themed weekly timetable where every class is a card you tap to mark attendance, with Q1/Q2 fortnight rotation, live stats, and a self-updating Android APK. No account, no cloud, no server needed.

The app ships three ways
Shape
What it is
Runs on
Standalone HTML (the product)	One self-contained file (~620 KB): UI + CSS + fonts + timetable + attendance history + all logic inlined. Zero network by construction.	Any browser, even from file://
Android APK	WebView wrapper (com.fedi.unitrack) embedding the same HTML from device assets. 100% offline — the only network use is the GitHub update check.	Phone, marks persist in WebView localStorage
Server version (legacy backup)	The original Next.js app (same UI + SQLite on port 3000). Kept as a history source; no longer required by the phone.	Sandbox / dev machine

Features
13 subjects / 24 weekly sessions, each subject keeps one consistent color everywhere (cards, stats, chips)
Tap-to-cycle marking: unmarked → attended (emerald) → skipped (rose) → dismissed / prof absent (purple) → clear. Saves instantly.
Q1/Q2 quinzaine (fortnight) system: alternating sessions swap in and out automatically per week; scheduled cards carry a Q1/Q2 badge, and the header shows the current week's half
Week navigation: previous / next / back-to-today, with a rollover timer so "today" follows midnight even when the app stays open
Stats tab: overall rate, per-subject rates (with each subject's color), and per-week history — oldest first
Self-updating: online + new version on GitHub → update modal; offline → zero nagging, normal use
How stats are counted
graded = attended + skipped — dismissed sessions (prof absent / cancelled) are excluded from the denominator, so a cancelled class never hurts the rate. Unmarked sessions count for nothing.

The data
Schema (Prisma / SQLite on the server; mirrored as an embedded JSON seed + localStorage snapshot in the standalone build):
Subject — name, color key, display order
Class — dayOfWeek (0–6), HH:mm start/end, room, kind (Cours | TD | TP), teacher, frequency (weekly | q1 | q2)
AttendanceRecord — classId + date (YYYY-MM-DD, unique pair), status (attended | skipped | dismissed)
Current dataset: 13 subjects / 24 classes / 29 attendance records
Fortnight anchor: q1 = odd ISO weeks, q2 = even ISO weeks (week of Oct 19 2026 = ISO week 43 = q1)
Server backup: every attendance write is mirrored to /home/sync/unitrack-backup (server version only)
