# 🎓 AcademiaQR — University QR Attendance & Academic Management System

> **A production-grade, anti-proxy dynamic QR code attendance and academic hierarchy management system with real-time GPS geofencing, student camera scanning, live lecturer dashboards, and 80% attendance policy compliance enforcement.**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Vite](https://img.shields.io/badge/Vite-8.x-646CFF.svg?logo=vite&logoColor=white)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3.x%2F4.x-38B2AC.svg?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Firebase](https://img.shields.io/badge/Firebase-v10_Modular-FFCA28.svg?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![Geolocation](https://img.shields.io/badge/GPS-Haversine_Geofencing-10B981.svg)](https://en.wikipedia.org/wiki/Haversine_formula)

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
5. **🏛️ Full 4-Year Academic Hierarchy & RBAC:**
   - Degree Programs ➔ Academic Years (1–4) ➔ Semesters (1 & 2) ➔ Modules ➔ Lecture Sessions ➔ Attendance Records.
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
