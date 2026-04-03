# GigShield  AI Powered Parametric Income Insurance for Food Delivery Partners


## Problem Statement

India's food delivery partners (Zomato, Swiggy) lose **20–30% of monthly income** during extreme weather, pollution, curfews, and natural disasters. They have **no financial safety net** when disruptions happen, they bear the full loss alone.

**GigShield** solves this with a parametric insurance platform that:
- Monitors disruptions in real time
- Automatically triggers claims (zero paperwork)
- Instantly pays out lost income to the worker's UPI account

---

## Persona: Food Delivery Partner (Zomato / Swiggy)

### Who They Are
- Age: 20–35 | City based | 2 wheeler rider
- Works 8–10 hours/day, 6 days/week
- Earns ₹500–₹1,200/day (₹3,000–₹8,400/week)
- Highly dependent on daily earnings for rent, food, EMIs
- Low digital literacy needs a simple, Regional language friendly app

### Persona Scenarios

#### Scenario 1 Ramesh, Mumbai (Monsoon Disruption)
> Ramesh has been delivering for Swiggy for 2 years. One Tuesday in July, Mumbai receives 180mm of rainfall in 6 hours. Zomato suspends operations. Ramesh cannot work for 9 hours, losing ~₹700. With GigShield, the system detects rainfall > 15mm/hr in his pincode, auto-triggers a claim, and ₹600 is credited to his UPI within 10 minutes. He never had to open the app.

#### Scenario 2 Arjun, Delhi (Pollution Shutdown)
> Delhi's AQI crosses 400 (Severe+). The local authority issues an advisory halting outdoor work for 2 days. Arjun loses ₹1,400. GigShield detects the AQI threshold breach, cross-validates with his active policy zone, and processes a 2-day payout automatically.

#### Scenario 3 Priya, Bangalore (Local Bandh)
> A sudden unplanned bandh is declared in Priya's delivery zone. She cannot access the restaurant pickup areas. GigShield's social disruption trigger (fed by a verified news/government API) flags the event and issues a zone specific payout for the affected hours.

---

## Application Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    WORKER ONBOARDING                        │
│  Sign Up → KYC (Aadhaar/DL) → Link Zomato/Swiggy ID →       │
│  Set Delivery Zone → Income Verification → Policy Issued    │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│                  AI RISK PROFILING                          │
│  Zone Risk Score + Historical Disruption Freq +             │
│  Weekly Income Estimate → Dynamic Weekly Premium            │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│              REAL-TIME DISRUPTION MONITORING                │
│  Weather API (rain, heat) + AQI API + Social Trigger API    │
│  Runs every 30 minutes against all active policy zones      │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│                PARAMETRIC TRIGGER ENGINE                    │
│  Threshold Breached? → Fraud Check → Auto Claim Created     │
│  → Payout Calculated → UPI Transfer Initiated               │
└─────────────────────┬───────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────┐
│                    DASHBOARDS                               │
│  Worker: Coverage status, payouts received, active policy   │
│  Admin: Loss ratios, risk heatmaps, fraud flags             │
└─────────────────────────────────────────────────────────────┘
```

---

## Weekly Premium Model

### How It Works
Premiums are charged **every Monday** via UPI auto debit. Coverage runs Monday 00:00 → Sunday 23:59.

### Premium Calculation Formula

```
Base Premium = ₹49/week (standard coverage)

Adjustments:
  + Zone Risk Multiplier     (0.8x – 1.4x based on flood/heat history)
  + Income Bracket Modifier  (higher earners pay slightly more for proportional coverage)
  - Loyalty Discount         (–₹2/week after 12 consecutive weeks)
  - Low-Risk Zone Discount   (–₹3/week for historically safe zones)

Final Weekly Premium Range: ₹29 – ₹99/week
```

### Coverage Tiers

| Tier | Weekly Premium | Max Weekly Payout | Disruptions Covered |
|------|---------------|-------------------|---------------------|
| Basic Shield | ₹29 | ₹500 | Rain, Heat |
| Full Shield | ₹59 | ₹1,200 | Rain, Heat, AQI, Curfew |
| Pro Shield | ₹99 | ₹2,500 | All triggers + extended hours |

### What Is Covered (Income Loss ONLY)
✅ Lost delivery hours due to extreme rain (>15mm/hr)

✅ Lost income during extreme heat advisory (>43°C)

✅ Work stoppage due to severe AQI (>300)

✅ Income loss from verified curfews/bandhs in delivery zone

### What Is NOT Covered
❌ Vehicle repairs or damage

❌ Health or accident expenses

❌ Life insurance

❌ Income loss due to personal reasons

---

## Parametric Triggers

| Trigger ID | Event | Data Source | Threshold | Payout |
|-----------|-------|-------------|-----------|--------|
| T1 | Heavy Rain | OpenWeatherMap API | Rainfall > 15mm/hr | ₹75/hr lost |
| T2 | Extreme Heat | OpenWeatherMap API | Temp > 43°C for 3+ hrs | ₹60/hr lost |
| T3 | Severe Pollution | OpenAQ API | AQI > 300 | ₹70/hr lost |
| T4 | Curfew/Bandh | Mock Govt. Alert API | Zone flagged active | ₹80/hr lost |
| T5 | Flash Flood | IMD Mock API | Flood alert Level 2+ | ₹90/hr lost |

---

## AI/ML Integration Plan

### 1. Dynamic Premium Calculator (scikit learn)
- **Model**: Gradient Boosted Regressor
- **Inputs**: Zone lat/lng, historical disruption count, avg weekly income, season, worker tenure
- **Output**: Personalized weekly premium in ₹
- **Training Data**: Synthetic dataset of 10,000 worker profiles + historical weather data

### 2. Fraud Detection Engine (Rule Based + ML Hybrid)
- **GPS Validation**: Worker's last known location must be within disruption zone
- **Platform Cross Check**: If Zomato API shows active orders during claimed disruption → flag
- **Anomaly Score**: ML model flags workers with claim frequency > 2 std deviations above mean
- **Duplicate Prevention**: Claim deduplication by worker ID + event ID + date

### 3. Predictive Risk Modeling
- Weekly forecast of likely disruption events per zone
- Helps insurer pre-provision payout reserves
- Shown on Admin dashboard as "Next Week Risk Forecast"

---

## Platform Choice: Web (PWA)

**Why Web over Native Mobile?**
- Workers use low end Android phones PWA works without Play Store install
- Reduced friction for onboarding (share a link, not an APK)
- Admin dashboard works on desktop browsers for insurers
- Single codebase for both worker and admin portals

---

## Tech Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| Frontend | React.js + Tailwind CSS | Team strength; fast UI dev |
| PWA | Vite PWA Plugin | Offline support, installable |
| Backend | Node.js + Express | REST APIs, lightweight |
| Database | PostgreSQL | Relational data for policies/claims |
| Cache | Redis | Real-time trigger data caching |
| ML Service | Python + FastAPI + scikit-learn | Premium calc & fraud detection |
| Weather API | OpenWeatherMap (free tier) | Real rain/heat data |
| AQI API | OpenAQ (free) | Pollution trigger |
| Payments | Razorpay Test Mode | Simulated UPI payouts |
| Auth | JWT + OTP (mock SMS) | Simple, mobile-friendly |
| Hosting | Vercel (frontend) + Railway (backend) | Free tier deployment |

---

## Repository Structure

```
gigshield/
├── frontend/                  # React PWA
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Onboarding/    # Worker registration flow
│   │   │   ├── Dashboard/     # Worker dashboard
│   │   │   ├── Policy/        # Policy & premium view
│   │   │   ├── Claims/        # Claims history
│   │   │   └── Admin/         # Insurer dashboard
│   │   ├── components/        # Reusable UI components
│   │   └── services/          # API calls
├── backend/                   # Node.js + Express
│   ├── routes/
│   │   ├── auth.js
│   │   ├── policy.js
│   │   ├── claims.js
│   │   └── triggers.js
│   ├── models/                # DB schemas
│   └── services/
│       ├── weatherService.js  # OpenWeatherMap integration
│       └── payoutService.js   # Razorpay mock
├── ml-service/                # Python FastAPI
│   ├── premium_model.py       # Premium calculator
│   ├── fraud_detector.py      # Fraud scoring
│   └── risk_forecast.py       # Predictive analytics
└── mock-data/                 # Simulated APIs & datasets
    ├── workers.json
    ├── disruptions.json
    └── weather_history.json
```

---

## Development Plan

### Phase 1 (Mar 4–20): Foundation
- [x] Finalize architecture & README
- [ ] Set up GitHub repo & project structure
- [ ] Design UI wireframes (Figma)
- [ ] Build onboarding flow prototype (React)
- [ ] Record 2 minute concept video

### Phase 2 (Mar 21–Apr 4): Core Build
- [ ] Worker registration + KYC flow
- [ ] Premium calculator (ML model)
- [ ] Policy creation & management UI
- [ ] OpenWeatherMap + OpenAQ integration
- [ ] Claims dashboard UI
- [ ] Auto trigger engine (4 triggers)

### Phase 3 (Apr 5–17): Polish & Scale
- [ ] Fraud detection module
- [ ] Razorpay test mode payout simulation
- [ ] Worker analytics dashboard
- [ ] Admin/insurer dashboard with charts
- [ ] Final pitch deck (PDF)
- [ ] 5-minute demo video

---
## Links
- 🎥 Demo Video (Phase 1): *[To be added]*
- 🎥 https://youtube.com/shorts/B3NbgCR6wC0
- 🌐 Live Demo: https://gig-shield-mu.vercel.app/
## 👥 Team

**Team Name**: Apex Innovators
**Hackathon**: Guidewire DEVTrails 2026
