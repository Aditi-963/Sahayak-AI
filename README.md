# 🇮🇳 Sahayak AI
### Your AI-Powered Guide to Ayushman Bharat Eligibility & Enrollment

**Making government healthcare benefits easier to understand, one family at a time.**

Sahayak AI is a bilingual, conversational web application that helps Indian families understand their potential eligibility for **Ayushman Bharat Pradhan Mantri Jan Arogya Yojana (AB PM-JAY)**. Instead of navigating complicated eligibility rules and paperwork, users can interact with a friendly, step-by-step assistant in English or Hindi.

The platform evaluates user responses using a deterministic, rule-based eligibility engine and generates a personalized enrollment summary with matched criteria, a document checklist, and clear next steps.

> **Our mission:** Make healthcare scheme information more accessible, understandable, and actionable for every Indian family.

---

##  Key Features

###  1. Conversational Eligibility Assistant
- Interactive, chat-style eligibility assessment.
- One question at a time, with simple explanations.
- Dynamic question branching based on previous answers.
- Special eligibility flow for senior citizens aged 70 and above.
- Transparent explanations of the criteria matched.
- Rule-based decision-making for consistent and predictable results.

###  2. Bilingual Experience
- Complete English and Hindi interfaces.
- Simple, conversational Hindi designed for accessibility.
- Language selection persists throughout the application.
- Hardcoded translations eliminate the need for external translation APIs.

###  3. PM-JAY Eligibility Assessment
The eligibility engine evaluates user-provided information against the criteria configured for the application.

**Rural household indicators**
- Kutcha housing conditions.
- No working household member aged 16–59.
- No adult male household member aged 16–59.
- A disabled household member with no able-bodied adult.
- Landless households dependent on manual labour.
- Scheduled Caste (SC) or Scheduled Tribe (ST) household status.

**Urban occupational categories**
- Ragpickers.
- Beggars.
- Domestic workers.
- Street vendors and hawkers.
- Construction workers and related trades.
- Cobblers.
- Transport workers.
- Coolies and head-load workers.
- Sanitation workers.
- Home-based artisans.
- Security guards.

**Additional eligibility pathways**
- Senior citizens aged 70 and above.
- ASHA workers.
- Anganwadi workers and helpers.

The system also collects ration card information as supporting context rather than treating it as an independent eligibility guarantee.

**Important:** These are simplified screening indicators, not a complete reproduction of official eligibility rules. The application must distinguish potential eligibility from confirmed entitlement and direct users to official verification.

###  4. Personalized Enrollment Summary
Generate a structured summary containing:

- Applicant and household details.
- Eligibility assessment result.
- Criteria that matched the user's responses.
- Applicable coverage information.
- A personalized document checklist.
- Recommended next steps for verification and enrollment.

Users can download or print their summary and share a concise summary through WhatsApp.

###  5. Document Checklist
The generated summary includes documents that may be useful during verification:

- Aadhaar cards for family members.
- Ration card, if available.
- Mobile phone linked to Aadhaar, where required.
- Bank passbook, if requested by the relevant verification process.

The application explains that the exact requirements may vary and should be confirmed with an authorized verification centre.

###  6. Privacy-Conscious MVP
- No login or account creation required.
- No paid AI APIs required.
- No direct Aadhaar or government database integration.
- Local, deterministic eligibility evaluation.
- Optional Aadhaar collection.
- Minimal data collection and clear privacy messaging.

The application should avoid retaining sensitive personal information unless necessary and should protect any information stored by the backend.

---

##  The Problem

Accessing government healthcare benefits can be difficult when families face:

- Complex eligibility criteria.
- Confusing official terminology.
- Language barriers.
- Uncertainty about which documents to carry.
- Limited awareness of verification and enrollment procedures.
- Dependence on intermediaries for basic guidance.

These obstacles can make an already complicated process even more challenging.

##  Our Solution

Sahayak AI turns eligibility screening into a guided, conversational experience.

Instead of presenting users with a long form or overwhelming them with policy documents, it:

1. Asks simple questions about the household.
2. Adapts subsequent questions according to the answers.
3. Evaluates responses using a rule-based eligibility engine.
4. Explains the assessment and its limitations.
5. Generates a personalized enrollment summary.
6. Provides a practical checklist and recommended next steps.

**The goal is not merely to provide information. It is to help users understand what to do next.**

---

##  User Journey

```text
           ┌──────────────────────┐
           │   Open Sahayak AI    │
           └──────────┬───────────┘
                      │
                      ▼
           ┌──────────────────────┐
           │ Select Language      │
           │ English / हिंदी      │
           └──────────┬───────────┘
                      │
                      ▼
           ┌──────────────────────┐
           │ Start Conversation   │
           │ Household Questions  │
           └──────────┬───────────┘
                      │
                      ▼
           ┌──────────────────────┐
           │ Rule-Based Eligibility│
           │ Engine (FastAPI)     │
           └──────────┬───────────┘
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      Eligible    Not Auto-    Needs
                  Eligible   Verification
          │           │           │
          └───────────┼───────────┘
                      │
                      ▼
           ┌──────────────────────┐
           │ Explain Result       │
           │ & Next Steps         │
           └──────────┬───────────┘
                      │
                      ▼
           ┌──────────────────────┐
           │ Generate Enrollment  │
           │ Summary & Checklist  │
           └──────────┬───────────┘
                      │
                      ▼
           ┌──────────────────────┐
           │ Download / Print /   │
           │ Share via WhatsApp   │
           └──────────────────────┘
```

---

##  Eligibility Engine

The eligibility engine is intentionally implemented using deterministic Python logic rather than relying on a generative AI model to make eligibility decisions.

This design provides:

- Predictable outputs for identical inputs.
- Explicit, inspectable decision rules.
- Easier testing and debugging.
- No per-request LLM API costs.
- Clear explanations of matched criteria.

### Decision Outcomes

| Status | Meaning | Recommended Action |
|---|---|---|
| `eligible` | The configured screening rules indicate a potential eligibility pathway. | Explain the matched criteria and recommend official verification. |
| `not_auto_eligible` | The supplied answers do not establish an automatic match under the configured rules. | Explain alternative verification pathways and state-specific options. |
| `uncertain` | The available information is insufficient to determine a likely outcome. | Recommend verification through an authorized channel. |

**Note:** The status describes the application's screening result, not an official government determination.

### Example API Response

```json
{
  "status": "eligible",
  "matched_criteria": [
    "senior_citizen_70_plus"
  ],
  "coverage_amount": 500000,
  "message": "Your answers indicate a potential eligibility pathway. Please verify through an official channel."
}
```

The coverage value above is an illustrative standard PM-JAY screening value. Senior-citizen coverage rules must be represented separately where applicable rather than assuming every household receives an identical additional benefit.

---

##  System Architecture

Sahayak AI uses a decoupled frontend and backend architecture.

```text
┌──────────────────────────────────────┐
│           React Frontend             │
│                                      │
│  Landing Page                        │
│  Conversational Assessment            │
│  Eligibility Results                 │
│  Enrollment Summary                  │
│  English / Hindi Interface           │
└──────────────────┬───────────────────┘
                   │ REST API
                   ▼
┌──────────────────────────────────────┐
│          FastAPI Backend             │
│                                      │
│  POST /api/check-eligibility          │
│  POST /api/generate-summary           │
│                                      │
│  Rule-Based Eligibility Engine        │
│  Summary Generation                  │
│  Request Validation                  │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│             Database                 │
│                                      │
│  Applicants                          │
│  Assessment Responses                │
│  Eligibility Results                 │
│  Optional Eligibility Rules          │
└──────────────────────────────────────┘
```

The frontend manages the conversation and presentation, while the FastAPI backend remains the authoritative source for eligibility calculations.

---

##  Technology Stack

### Frontend
- **React** — component-based user interface.
- **TypeScript** — type-safe application development.
- **Tailwind CSS** — responsive styling and design system.
- **Fetch API** — communication with the backend REST endpoints.

### Backend
- **Python** — eligibility and summary logic.
- **FastAPI** — REST API framework.
- **Pydantic** — request and response validation.
- **Uvicorn** — ASGI server.

### Database
- **SQLite** — lightweight local development and demonstrations.
- **PostgreSQL** — production-oriented database option.
- **SQLAlchemy** — database models and persistence.

### Document Generation
- Browser-based print layout for printable summaries.
- PDF export using a suitable PDF-generation library or browser print-to-PDF.
- WhatsApp sharing through a prefilled `wa.me` link.

### Infrastructure
- Frontend: Vercel or Netlify.
- Backend: a Python-compatible hosting platform.
- Database: SQLite for local demonstrations or managed PostgreSQL for deployment.

---

##  Project Structure

A suggested repository structure:

```text
sahayak-ai/
│
├── frontend/
│   ├── public/
│   │
│   ├── src/
│   │   ├── components/
│   │   │   ├── ChatMessage.tsx
│   │   │   ├── QuestionCard.tsx
│   │   │   ├── ProgressIndicator.tsx
│   │   │   ├── LanguageToggle.tsx
│   │   │   └── DocumentChecklist.tsx
│   │   │
│   │   ├── pages/
│   │   │   ├── LandingPage.tsx
│   │   │   ├── EligibilityPage.tsx
│   │   │   ├── ResultPage.tsx
│   │   │   └── SummaryPage.tsx
│   │   │
│   │   ├── i18n/
│   │   │   └── translations.ts
│   │   │
│   │   ├── services/
│   │   │   └── api.ts
│   │   │
│   │   ├── types/
│   │   │   └── eligibility.ts
│   │   │
│   │   ├── App.tsx
│   │   └── main.tsx
│   │
│   ├── package.json
│   └── tailwind.config.ts
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── eligibility.py
│   │   ├── summary.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   └── database.py
│   │
│   ├── tests/
│   │   └── test_eligibility.py
│   │
│   ├── requirements.txt
│   └── .env.example
│
├── .gitignore
└── README.md
```

*This is the proposed structure; individual files should be created as implementation progresses.*

---

##  Getting Started

### Prerequisites

Install the following:

- Node.js and npm.
- Python 3.10 or later.
- Git.

PostgreSQL is optional when using SQLite for local development.

### 1. Clone the Repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd sahayak-ai
```

Replace `<YOUR_REPOSITORY_URL>` with your Git repository URL.

### 2. Start the Backend

Navigate to the backend directory:

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

Activate it on macOS or Linux:

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the FastAPI server:

```bash
uvicorn app.main:app --reload
```

The API will be available at:

```text
http://localhost:8000
```

Interactive API documentation:

```text
http://localhost:8000/docs
```

### 3. Start the Frontend

Open a second terminal:

```bash
cd frontend
npm install
```

Configure the backend URL in the frontend environment file:

```env
VITE_API_BASE_URL=http://localhost:8000
```

Save this as `.env.local` in the frontend directory.

Start the development server:

```bash
npm run dev
```

Open the local address printed by Vite in your terminal.

### 4. Test the Application

Verify the following:

- The landing page loads correctly.
- English and Hindi translations work across all screens.
- Question branching follows the configured decision rules.
- Eligibility results display the correct matched criteria.
- The enrollment summary contains the submitted details.
- Download, print, and WhatsApp sharing actions work.
- The backend validates incomplete or invalid requests.

---

## 🔌 API Reference

### `POST /api/check-eligibility`

Evaluates the submitted household responses against the configured screening rules.

**Example request**

```json
{
  "senior_citizen_70_plus": false,
  "rural_or_urban": "rural",
  "rural_criteria": [
    "landless_manual_labour"
  ],
  "asha_anganwadi_worker": false,
  "ration_card_type": "BPL"
}
```

**Example response**

```json
{
  "status": "eligible",
  "matched_criteria": [
    "landless_manual_labour"
  ],
  "coverage_amount": 500000
}
```

The response schema is illustrative; the final implementation should define and validate all supported fields and outcomes.

### `POST /api/generate-summary`

Creates structured enrollment-summary data using the applicant's details and eligibility assessment.

**Example request**

```json
{
  "applicant": {
    "full_name": "Example Applicant",
    "state": "Maharashtra",
    "family_size": 4,
    "mobile_number": "0000000000",
    "aadhaar_number": null
  },
  "answers": {
    "senior_citizen_70_plus": false,
    "rural_or_urban": "rural",
    "ration_card_type": "BPL"
  },
  "eligibility_result": {
    "status": "eligible",
    "matched_criteria": [
      "landless_manual_labour"
    ],
    "coverage_amount": 500000
  }
}
```

**Expected output**

The endpoint returns structured data containing applicant details, the assessment result, matched criteria, a document checklist, and recommended next steps.

The frontend uses this response to render a printable or downloadable summary.

In production, the backend should recompute or validate the eligibility result rather than blindly trusting a result submitted by the client.

---

##  Database Design

### `applicants`

| Field | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `full_name` | TEXT | Applicant's name |
| `state` | TEXT | Indian state or union territory |
| `family_size` | INTEGER | Number of household members |
| `mobile_number` | TEXT | Contact number |
| `aadhaar_number` | TEXT, nullable | Optional Aadhaar number |
| `rural_or_urban` | TEXT | Household location category |
| `ration_card_type` | TEXT, nullable | Reported ration card category |
| `answers_json` | JSON / JSONB | Raw assessment responses |
| `eligibility_status` | TEXT | Screening result |
| `matched_criteria` | JSON / JSONB | Criteria identified by the engine |
| `created_at` | TIMESTAMP | Record creation time |

### `eligibility_rules` (Optional)

| Field | Type | Description |
|---|---|---|
| `id` | INTEGER | Primary key |
| `rule_code` | TEXT | Unique rule identifier |
| `category` | TEXT | Rural, urban, or automatic |
| `description_en` | TEXT | English explanation |
| `description_hi` | TEXT | Hindi explanation |

For the MVP, hardcoded rules are sufficient. A data-driven rules table can be introduced when rule versioning, regional variations, and maintainability become priorities.

**Privacy recommendation:** The MVP can generate a summary without persisting the applicant's personal details. If persistence is enabled, collect only necessary data, restrict access, define retention periods, and protect sensitive fields.

---

##  Design Philosophy

Sahayak AI is designed for people who need clarity, not complexity.

The interface follows these principles:

- **Human-first:** Friendly language and supportive messages.
- **Mobile-first:** Designed for smartphones and smaller screens.
- **Accessible:** Large buttons, readable typography, and clear visual hierarchy.
- **Bilingual:** English and simple Hindi throughout the experience.
- **Transparent:** Users can understand why a screening result was returned.
- **Trustworthy:** Uncertain outcomes are explained honestly rather than presented as guaranteed eligibility.

### Visual Direction

| Element | Design Choice |
|---|---|
| Primary colors | Soft blue and green |
| Background | Light, warm neutrals |
| Cards | Rounded corners with subtle shadows |
| Typography | Clean, readable sans-serif fonts |
| Eligible state | Calm green |
| Verification state | Soft amber |
| Not automatically eligible | Neutral, non-alarming colors |
| Layout | Responsive, conversational cards |

---

##  Accuracy, Safety & Trust

Healthcare access is an important public-service issue. Incorrect eligibility claims can create false expectations or cause people to miss legitimate benefits.

Sahayak AI therefore follows these principles:

1. **No guaranteed eligibility claims:** Results describe a preliminary assessment, not an official approval.
2. **Transparent decisions:** Matched criteria are displayed to the user.
3. **Honest uncertainty:** Missing or ambiguous information leads to a verification recommendation.
4. **Official verification:** Users are directed to authorized government channels.
5. **No fabricated integrations:** The MVP does not claim to access government databases or verify Aadhaar.
6. **Privacy by design:** Aadhaar is optional, and unnecessary personal data should not be retained.

The eligibility rules must be validated against current official PM-JAY guidance before the tool is used for real-world screening. In particular, senior-citizen benefits, rural and urban eligibility criteria, worker categories, and state-specific schemes should not be treated as interchangeable rules.

### Official Resources

- **PM-JAY official website:** https://pmjay.gov.in/
- **National Health Authority:** https://nha.gov.in/

Sahayak AI is an independent informational tool and is not affiliated with or endorsed by the Government of India, the National Health Authority, or any government department.

---

##  Roadmap

### Phase 1 — MVP
- [ ] Responsive landing page.
- [ ] English and Hindi language toggle.
- [ ] Conversational eligibility flow.
- [ ] Rule-based FastAPI eligibility engine.
- [ ] Eligible, not-auto-eligible, and uncertain result states.
- [ ] Enrollment summary generation.
- [ ] Printable and downloadable summary.
- [ ] WhatsApp sharing link.

### Phase 2 — Reliability & Accessibility
- [ ] Automated unit tests for every decision branch.
- [ ] Validation against current official eligibility guidance.
- [ ] Improved accessibility and multilingual usability.
- [ ] More detailed explanations of eligibility outcomes.
- [ ] Clear distinction between standard PM-JAY benefits and applicable senior-citizen benefits.
- [ ] Better handling of incomplete and contradictory answers.

### Phase 3 — Expanded Assistance
- [ ] State-specific scheme guidance.
- [ ] Verified information about nearby authorized service centres.
- [ ] Versioned eligibility rules.
- [ ] Optional secure applicant record management.
- [ ] Additional Indian language support.

Future features will depend on reliable information sources, privacy safeguards, and appropriate validation.

---

##  Expected Impact

Sahayak AI aims to reduce the effort required to understand healthcare eligibility and prepare for enrollment verification.

By combining conversational guidance, deterministic screening, bilingual communication, and personalized paperwork preparation, the platform can make the process more approachable for families who might otherwise struggle with complex government information.

**From confusion to clarity. From eligibility questions to actionable next steps.**

---

##  Team

**Project:** Sahayak AI

**Category:** AI for Social Good · Healthcare Accessibility · GovTech

**Built with:** React, TypeScript, Tailwind CSS, Python, FastAPI, and SQLite/PostgreSQL.

*Add your team members, GitHub profile links, and contributor credits here.*

---

##  License

This project can be released under the MIT License. Add a `LICENSE` file to the repository if you choose this license.

---

<p align="center">
  <b>Sahayak AI — Healthcare benefits, made easier to understand.</b>
  <br>
  Built with  for a more accessible and informed India.
</p>
