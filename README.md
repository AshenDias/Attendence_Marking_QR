# 🎓 AcademiaQR — University QR Attendance & Academic Management System

> **A production-grade, anti-proxy dynamic QR code attendance and academic hierarchy management system with real-time GPS geofencing, student camera scanning, live lecturer dashboards, and 80% attendance policy compliance enforcement.**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Vite](https://img.shields.io/badge/Vite-8.x-646CFF.svg?logo=vite&logoColor=white)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3.x%2F4.x-38B2AC.svg?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Firebase](https://img.shields.io/badge/Firebase-v10_Modular-FFCA28.svg?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![Geolocation](https://img.shields.io/badge/GPS-Haversine_Geofencing-10B981.svg)](https://en.wikipedia.org/wiki/Haversine_formula)

---

## 📌 Repository Short Description
*For GitHub repository "About" section:*
> **Enterprise University Attendance System featuring anti-proxy 5-second dynamic QR codes, client-side Haversine GPS geofencing, in-browser camera scanning (`html5-qrcode`), real-time lecturer dashboard, 80% exam eligibility monitors, and full 4-year academic hierarchy.**

---

## 🌟 Key Highlights & Anti-Cheating Architecture

Traditional university attendance systems suffer from screenshot sharing ("proxy attendance"), where an absent student receives a photo of the QR code from a peer in class. **AcademiaQR solves this through dual-layer physical presence verification:**

1. **⏱️ Dynamic 5-Second QR Token Refresh (Anti-Screenshot):**
   - The projected QR code automatically regenerates every 5 seconds with a unique cryptographic nonce and short-lived timestamp payload.
   - Any screenshot forwarded via messaging apps expires before an off-campus student can scan it.
2. **📍 GPS Geofencing via Haversine Great-Circle Algorithm:**
   - When a student scans the code, their device acquires live GPS coordinates (`navigator.geolocation`) and calculates the exact great-circle distance to the lecturer's classroom coordinates.
   - Attendance is strictly blocked if the student is outside the defined perimeter (e.g., >50 meters).
3. **⚠️ 80% Mandatory Attendance Risk Monitor:**
   - Real-time percentage tracking per module.
   - Color-coded badges and risk warnings for students falling below the mandatory 80% final exam eligibility threshold.
4. **📊 Live Lecturer Console & Manual Overrides:**
   - Real-time roster updating as students check in, displaying verified GPS distances (e.g. `12.4m`).
   - Manual override options for students with dead phone batteries or camera defects.
   - Instant one-click CSV spreadsheet export.
5. **📄 Official PDF Attendance & Academic Transcript Generator:**
   - Generate audit-ready, high-resolution printable PDF attendance transcripts.
   - Includes official university letterhead, statutory Academic Regulation §4.12 citation, cohort exam clearance classifications (`CLEARED`, `CONDITIONAL WARNING`, `BARRED`), Course Lecturer and Head of Department (HOD) endorsement signatures, and official registrar seal.
6. **🏛️ Full 4-Year Academic Hierarchy & RBAC:**
   - Degree Programs $\to$ Academic Years (1–4) $\to$ Semesters (1 & 2) $\to$ Modules $\to$ Lecture Sessions $\to$ Attendance Records.
   - Role-Based Access Control for **Students**, **Lecturers**, and **Administrators**.

---

## 📐 Mathematical Formulation: Haversine Algorithm

The application computes great-circle distance between two geographic coordinates on Earth using spherical trigonometry:

$$\Delta\varphi = \varphi_2 - \varphi_1$$

$$\Delta\lambda = \lambda_2 - \lambda_1$$

$$a = \sin^2\left(\frac{\Delta\varphi}{2}\right) + \cos(\varphi_1)\cdot\cos(\varphi_2)\cdot\sin^2\left(\frac{\Delta\lambda}{2}\right)$$

$$c = 2 \cdot \operatorname{atan2}\left(\sqrt{a}, \sqrt{1 - a}\right)$$

$$d = R \cdot c$$

*where $R \approx 6,371,000\text{ meters}$ (Earth's mean radius).*

```javascript
function getDistanceInMeters(lat1, lon1, lat2, lon2) {
  const R = 6371e3; // Earth radius in meters
  const φ1 = lat1 * Math.PI / 180;
  const φ2 = lat2 * Math.PI / 180;
  const Δφ = (lat2 - lat1) * Math.PI / 180;
  const Δλ = (lon2 - lon1) * Math.PI / 180;
  const a = Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
            Math.cos(φ1) * Math.cos(φ2) *
            Math.sin(Δλ / 2) * Math.sin(Δλ / 2);
  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
  return R * c; // Great-circle distance in meters
}
```

---

## 👥 User Roles & Features

### 👨‍🎓 1. Student Portal
- **Enrolled Modules View:** Filtered by current Academic Year and Semester.
- **Attendance Percentage Tracking:** Color-coded progress bars with warnings:
  - 🟢 **≥ 80%:** Good Standing (Eligible for Final Exams).
  - 🟡 **75% – 79%:** Warning Zone (Action required).
  - 🔴 **< 75%:** Critical Risk (Exam eligibility suspended pending appeal).
- **In-Browser Camera Scanner:** Native camera access using `Html5Qrcode` with targeting reticle and animated laser scanline.
- **GPS Proximity Status Indicator:** Displays live coordinates, distance to lecture hall, and in-range/out-of-range status.
- **Testing Simulator:** Quick toggles for *Inside Classroom (8.4m)*, *Outside Campus (185m)*, or *Device GPS* for development and grading.

### 👨‍🏫 2. Lecturer Console
- **Session Dispatcher:** Select Year, Semester, Module, Topic, Classroom Hall presets, and Geofence Radius (30m, 50m, 100m, 250m).
- **Auto-GPS Geotagging:** "Auto-Detect Current GPS Coordinates" locks the lecturer's physical position as the classroom origin.
- **Dynamic 5-Second QR Projector:** High-contrast projected QR display with countdown timer, progress ring, and auto-generated nonce.
- **Live Streamed Attendee Roster:** Real-time updates showing student ID, full name, timestamp, and verified GPS distance.
- **Manual Check-In Override:** Record attendance for students with device malfunctions with custom notes.
- **CSV Data Export:** Generates formatted `.csv` reports for university administration.

### 🏛️ 3. University Admin & Academic Registry
- **Curriculum Hierarchy:** 4-Year degree structure across Semesters 1 and 2.
- **Module Management:** Add new accredited modules with course code, credit values, year, semester, and assigned faculty lecturer.
- **User Directory & RBAC:** View and assign roles (`student`, `lecturer`, `admin`) to system users.
- **Campus Analytics:** Total enrolled students, active lecture sessions, total modules, and campus-wide average attendance rate.

---

## 🛠️ Technology Stack

| Component | Technology | Description |
|---|---|---|
| **Frontend Framework** | HTML5 / Single-Page Application (SPA) | Fully responsive glassmorphism UI |
| **Styling & Icons** | Tailwind CSS + Lucide Icons | Modern frosted-glass dark/light theme |
| **Camera QR Scanner** | `html5-qrcode` (v2.3+) | High-performance HTML5 canvas/video camera scanner |
| **QR Generation** | `qrcodejs` (v1.0) | Client-side dynamic QR canvas renderer |
| **Location Services** | W3C Geolocation API | High-accuracy hardware latitude and longitude |
| **Persistence & Real-time** | Firebase Firestore (Modular v10) + `BroadcastChannel` | Real-time multi-tab cross-device sync |
| **Audio Feedback** | Web Audio API | Synthesized frequency chimes for success/error events |

---

## 🚀 Quick Start Guide

### Prerequisites
- Node.js (v18 or higher recommended)
- Modern web browser with camera and location permissions enabled (Chrome, Safari, Edge, Firefox)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/academia-qr-attendance.git
   cd academia-qr-attendance
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Run the local development server:**
   ```bash
   npm run dev
   ```

4. **Open in browser:**
   ```
   http://localhost:3000
   ```

---

## 🧪 Multi-Tab Live Testing Guide

You can test real-time attendance synchronization without physical mobile devices:

1. Open `http://localhost:3000` in **Tab A** and switch the persona to **Dr. Evelyn Reed (Lecturer)**.
2. In Tab A, ensure the session is active (Dynamic QR code is refreshing every 5 seconds).
3. Open `http://localhost:3000` in **Tab B** (or in an Incognito window) and select **Alex Rivera (Student)**.
4. In Tab B, click **"One-Click Test Scan Live Session"**.
5. Switch back to **Tab A**: Notice Alex Rivera appears in the live attendee roster in real time with GPS distance recorded!

---

## 🗄️ Firebase Configuration

AcademiaQR includes resilient multi-tab synchronization out of the box via `BroadcastChannel` and `localStorage`. To connect your own live Google Cloud / Firebase Firestore database:

1. Open the app and click the user avatar in the top-right corner.
2. Select **"Firebase Settings & Sync"**.
3. Paste your web project configuration object:
   ```json
   {
     "apiKey": "AIzaSyYourActualApiKeyHere",
     "authDomain": "your-project.firebaseapp.com",
     "projectId": "your-project",
     "storageBucket": "your-project.appspot.com",
     "messagingSenderId": "123456789012",
     "appId": "1:123456789012:web:abcdef123456"
   }
   ```
4. Click **Save Config**.

---

## 🎭 Demo Personas

The system includes pre-configured personas accessible from the top-right profile menu:

| Persona | Role | Academic Program / Details |
|---|---|---|
| **Alex Rivera** | Student | Year 2 Sem 1 • B.Sc. Software Engineering (Good Standing: 88.5%) |
| **Maya Chen** | Student | Year 2 Sem 1 • B.Sc. Software Engineering (⚠️ Low Attendance: 71.4%) |
| **Dr. Evelyn Reed** | Lecturer | Senior Lecturer • Dept. of Software Engineering |
| **Prof. Marcus Vance** | Lecturer | Professor • Dept. of Computer Science & AI |
| **Prof. Sarah Jenkins**| Admin | Dean / Academic Registrar |

---

## 📄 License

This project is licensed under the **Apache License 2.0** — see the [LICENSE](LICENSE) file for details.
