
# **Full-Stack Developer**

**Evaluation Criteria**

* Clarity + practicality of architecture
* Clean data model + API design
* Async job + storage thinking (uploads, outputs, retries)
* Security basics (auth, access control, safe downloads)
* Cost + scalability tradeoffs (MVP → v1)
* Ability to handle ambiguity + user review flows

---

## **Problem 1:** **Video-to-Notes Platform (Architecture Proposal)**

We have long videos (3–4 hours, 200MB+). We need an automated “summary package” per video: **Summary.md + highlights (timestamps) + screenshots/clips references**, organized per video. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

**Task (No code)**

Create a concise architecture proposal for an MVP.

**Your Solution must include**

* Minimal user flow (3–5 steps)
* High-level architecture diagram (UI, API, DB, worker/queue, storage, AI)
* Job lifecycle (queued → processing → success/failed) + progress reporting
* Data model (tables/entities only)
* API list (8–12 endpoints)

**Your Solution for problem 1:**

You need to put your solution here.

Solution:
A. Minimal User Flow

1. User logs into the platform using frontend (React.js).
2. User uploads a video file.
3. Backend receives the video and stores it in storage.
4. Backend processes the video and generates summary, highlights, clips, and screenshots.
5. User views and downloads the generated summary and assets.

B. High-Level Architecture
Frontend (React.js):
- Allows user to upload video
- Shows processing status
- Displays summary and highlights
- Provides download links for clips and screenshots
  
Backend (Node.js + Express.js):
- Handles video upload API
- Stores video metadata in MongoDB
- Processes video and generates summary
- Manages summary and asset retrieval

Database (MongoDB):
- Stores user data
- Stores video information
- Stores summary and highlights data

Storage:
- Stores uploaded videos
- Stores generated clips and screenshots
- Stores Summary.md file

Processing Module:
- Extracts video metadata (duration, filename)
- Generates summary using AI API
- Creates clips and screenshots based on timestamps

Flow:
React Frontend → Express API → MongoDB + Storage → Processing Module → MongoDB → Frontend

C. Job Lifecycle

1. User uploads video → status = uploaded
2. System starts processing → status = processing
3. Summary and assets generated → status = completed
4. If error occurs → status = failed

Frontend checks status using API and updates UI.

D. Output Folder Structure
For each video, system generates:

output/
  videoId/
    Summary.md
    clips/
    screenshots/

Summary.md contains:
- Video metadata
- Summary
- Highlights with timestamps
- Links to clips and screenshots

E. Data Model

User Collection:
- _id
- name
- email
- password

Video Collection:
- _id
- userId
- fileName
- fileUrl
- status
- createdAt

Summary Collection:
- _id
- videoId
- summaryText
- highlights
- createdAt

Asset Collection:
- _id
- videoId
- clipUrls
- screenshotUrls

F. API Endpoints

POST /api/auth/register
POST /api/auth/login

POST /api/videos/upload
GET /api/videos

GET /api/videos/:id
GET /api/videos/:id/status

GET /api/videos/:id/summary

GET /api/videos/:id/assets

DELETE /api/videos/:id

G. Scalability Considerations

- Video processing runs separately from API to avoid blocking requests
- MongoDB indexes improve performance
- Storage is separated for better scalability
- Backend can handle multiple users and videos

## **Problem 2:** **LinkedIn Automation Platform (Architecture + Prompt Spec)**

User connects LinkedIn, defines persona/tone, provides topics. System generates **3 post drafts**, user approves, schedules, and posts automatically. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Task (No code)**

Provide:

1. **Architecture proposal** for MVP (auth, scheduling, approvals, posting).
2. How to store prompts provide by GenAI team

**Your Solution must include**

* Minimal flow: connect → persona → generate → approve → schedule → post
* Architecture blocks + key integrations (LinkedIn, LLM, scheduler)
* Data model (User, Persona, Draft, Schedule, PostLog)
* API list (8–12 endpoints)

**Your Solution for problem 2:**

You need to put your solution here.

---

## **Problem 3:** **DOCX Template → Bulk DOCX/PDF Generator Architecture**

Users upload Word templates, system detects editable fields, supports **single fill** and **bulk generation via CSV/Sheet**, exports DOCX/PDF, provides ZIP + report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Task (No code)**

Provide MVP architecture + LLM prompt spec for:

* Template field detection
* Field schema generation (types, required, validations)

**Your Solution must include**

* Flow: upload template → field review → single generate → bulk generate
* Architecture blocks (template parser, worker, storage, export service)
* Data model (Template, TemplateField, BulkRun, RowResult, Artifact)
* Bulk report format (success/fail + reason)

**Your Solution for problem 3:**

You need to put your solution here.

---

## **Problem 4:** **Character-Based Video Series Generator (Architecture Proposal)**

User defines characters once (image + personality + relationships). For each episode, user provides a short story/situation. System outputs an “episode package” (script, scenes, assets list, voiceover plan) and optionally a final video.  [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Task (No code)**

Create a small architecture proposal for MVP.

**Your Solution must include**

* Data model for Character, Relationship, Episode, Scene, Asset
* Pipeline flow: story → scene breakdown → dialogues → asset plan → render plan
* Consistency strategy (character memory, style guide, asset reuse)
* MVP scope vs v1 scope (what you would ship first)

**Your Solution for problem 4:**

You need to put your solution here.
