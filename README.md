# 🏥 CuraNet

> **An intelligent, decentralized hospital network and emergency response ecosystem connecting patients, healthcare facilities, ambulances, and clinical resources in real time.**

---

## 📌 Overview

**CuraNet** is a modern healthcare operations platform designed to eliminate hospital bottlenecks, accelerate emergency response times, and balance patient loads across regional healthcare networks. By combining real-time telematics, smart queue algorithms, AI clinical triage, and resource tracking, CuraNet bridges the gap between emergency services, outpatient care, and inter-facility coordination.

---

## ✨ Key Features (A to Z)

| Feature | Description |
| :--- | :--- |
| **🚑 Live Emergency & Ambulance Tracking** | Instant SOS dispatch with live GPS route rendering, dynamic ETA calculation, and automated nearest-hospital routing via WebSockets. |
| **🎟️ Smart OPD & Dynamic Token System** | Real-time queue management, predictive wait times, and priority triage reducing outpatient congestion. |
| **🛌 Bed & Ward Availability Matrix** | Live monitoring and automated bed allocation across General, ICU, HDU, and Emergency wards. |
| **🤖 AI Symptom Checker & Disease Diagnosis** | Flask ML microservice with fuzzy symptom matching (RapidFuzz) and pre-trained classification models for clinical guidance and precautions. |
| **🌐 Hospital Network & Inter-Facility Transfers** | Centralized network for inter-hospital patient transfers and cross-facility resource sharing during peak surges. |
| **🩸 Smart Blood Bank Management** | Unit-level inventory tracking by blood group, cross-hospital exchange, and emergency donor requests. |
| **📦 Supply Inventory & Resource Decay** | Predictive pharmaceutical tracking with consumption decay modeling and low-stock alerting. |
| **🦠 Disease Outbreak Detection** | Spatial and temporal epidemic cluster tracking using statistical anomalies to identify localized health threats early. |
| **👥 Role-Based Portals** | Tailored workspaces for **Patients**, **Doctors**, **Nurses**, **Hospital Administrators**, and **Super Admins**. |
| **🗺️ City Heatmap & Hospital Traffic** | Geospatial analytics visualizing crowd density, average wait times, and hospital occupancy citywide. |

---

## 🏗️ Architecture & Tech Stack

```
                     ┌───────────────────────────────────┐
                     │     Frontend (Vite + React TS)    │
                     │  Tailwind CSS • shadcn/ui • Maps  │
                     └───────────────┬───────────────────┘
                                     │
           ┌─────────────────────────┼─────────────────────────┐
           ▼                         ▼                         ▼
┌───────────────────────┐ ┌───────────────────────┐ ┌───────────────────────┐
│     Node.js API       │ │    Python AI Engine   │ │  Supabase / Firebase  │
│ Express 5 • Socket.io │ │  Flask • Scikit-Learn │ │ Auth • Realtime DB    │
│    MongoDB Atlas      │ │   RapidFuzz • Pandas  │ │ PostgreSQL Schemas    │
└───────────────────────┘ └───────────────────────┘ └───────────────────────┘
```

- **Frontend**: React 18, TypeScript, Vite, Tailwind CSS, shadcn/ui, Lucide Icons, TanStack React Query, Socket.IO Client.
- **Backend**: Node.js, Express.js 5, Socket.IO (WebSockets), Mongoose, CORS, Dotenv.
- **AI / ML Service**: Python 3.9+, Flask, Scikit-learn (`disease_model.pkl`), RapidFuzz, Joblib, Pandas, NumPy.
- **Databases**:
  - **MongoDB Atlas**: Hospital systems, ambulance fleets, and operational records.
  - **Supabase (PostgreSQL)**: Ambulance detection, outbreak tracking, and resource decay tables.
  - **Firebase**: Realtime Database & authentication services.

---

## 📁 Repository Structure

```plaintext
curanet/
├── backend/
│   ├── models/                # Mongoose schemas (Ambulance, Hospital, etc.)
│   ├── routes/                # Express REST routes (emergency, inventory, outbreak, city)
│   ├── services/              # Business logic & socket management
│   ├── utils/                 # Helper utilities and geo-calculations
│   ├── seedAmbulances.js      # Seed script for test ambulance fleets
│   ├── server.js              # Express + Socket.IO entry point (Port 5000)
│   └── package.json           # Backend dependencies
├── frontend/
│   ├── public/                # Static assets
│   ├── src/
│   │   ├── components/        # Reusable UI & shadcn components
│   │   ├── contexts/          # Auth & state providers
│   │   ├── hooks/             # Custom React hooks (emergency, queue, etc.)
│   │   ├── pages/             # Page components (OPD, Beds, CommandCenter, Portals)
│   │   ├── App.tsx            # Application routing & layout
│   │   └── main.tsx           # React DOM root
│   ├── app.py                 # Python Flask ML Symptom Checker & Diagnosis API
│   ├── disease_model.pkl      # Pre-trained ML model for disease classification
│   ├── symptoms_list.pkl      # Vocabulary list for symptom extraction
│   ├── desc_dict.pkl          # Disease descriptions dictionary
│   ├── prec_dict.pkl          # Clinical precautions dictionary
│   └── package.json           # Frontend dependencies
├── supabase/
│   ├── ambulance_detection_schema.sql  # Schema for ambulance telemetry
│   ├── disease_outbreak_schema.sql     # Schema for epidemic surveillance
│   └── resource_decay_schema.sql       # Schema for supply decay modeling
└── README.md                  # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

Ensure you have installed:
- **Node.js** (v18.0 or higher) & **npm**
- **Python** (v3.9 or higher) & **pip**
- **MongoDB Atlas** account (or local MongoDB)
- **Supabase** or **Firebase** account (optional for extended cloud services)

---

### 1. Backend Setup (Node.js & Socket.IO)

```bash
# Navigate to the backend folder
cd backend

# Install dependencies
npm install

# (Optional) Seed initial ambulance data into MongoDB Atlas
node seedAmbulances.js

# Start the server (runs on http://localhost:5000)
node server.js
```

---

### 2. Frontend Setup (React + Vite)

In a new terminal window:

```bash
# Navigate to the frontend folder
cd frontend

# Install dependencies
npm install

# Create your .env file
cp .env.example .env

# Start the Vite development server
npm run dev
```

The frontend will run at `http://localhost:8080` (or the port specified by Vite).

---

### 3. AI Symptom Checker Service (Python Flask)

In a new terminal window:

```bash
# Navigate to the frontend folder where app.py and .pkl files reside
cd frontend

# Install Python requirements
pip install flask flask-cors joblib numpy pandas rapidfuzz

# Start the AI Diagnostic Service
python app.py
```

The AI microservice listens on `http://localhost:5000/chat` for fuzzy symptom identification and recommendations.

---

## 🗄️ Database Schemas (Supabase / PostgreSQL)

If you are using Supabase, execute the SQL migration scripts located in the `supabase/` directory in your Supabase SQL Editor:
1. `ambulance_detection_schema.sql` — Tracks real-time ambulance coordinates, status, and emergency response IDs.
2. `disease_outbreak_schema.sql` — Aggregates illness reports for spatial anomaly detection.
3. `resource_decay_schema.sql` — Manages stock depreciation rates and expiry forecasting.

---

## 👥 User Roles & Access

| Role | Access Scope |
| :--- | :--- |
| **Patient** | Self-service symptom checker, queue token tracking, nearest hospital locator, and emergency SOS trigger. |
| **Doctor** | Consultation queue, digital prescriptions, patient clinical notes, and triage disposition. |
| **Nurse** | Patient intake, vitals registration, ward bed assignment, and triage staging. |
| **Hospital Staff** | Inventory management, blood bank log, bed capacity updates, and internal admissions. |
| **Admin** | Hospital-wide Command Center, capacity surge controls, resilience analytics, and network transfers. |

---

## 🛡️ License

This project is licensed under the **ISC License**.
