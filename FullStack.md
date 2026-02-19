
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

Solution:

A. Minimal User Flow

1. User logs into the platform.
2. User connects their LinkedIn account using secure authentication.
3. User creates or updates their persona (tone, style, experience, preferences).
4. User enters a topic for LinkedIn post generation.
5. System generates multiple post drafts based on persona and topic.
6. User reviews drafts, selects one, and chooses to post immediately or schedule for later.
7. System automatically publishes the post to LinkedIn at the selected time.

B. High-Level Architecture

Frontend (React.js):
- Provides UI for LinkedIn connection, persona setup, topic input, and post review.
- Displays generated drafts and scheduling options.
- Shows post status and history.

Backend (Node.js + Express.js):
- Handles authentication and LinkedIn connection.
- Receives topic and persona data.
- Generates post drafts using AI service.
- Stores drafts and scheduling info in MongoDB.
- Publishes posts to LinkedIn using LinkedIn API.

Database (MongoDB):
- Stores user data, persona configuration, LinkedIn access tokens, post drafts, and scheduling details.

AI Service:
- Generates LinkedIn posts based on user persona and topic.

Scheduler Module:
- Runs periodically to check scheduled posts.
- Publishes posts automatically at the scheduled time.

Integration Flow:
React Frontend → Express Backend → MongoDB → AI Service → MongoDB → Scheduler → LinkedIn API

C. LinkedIn Integration and Authentication

- User connects LinkedIn account using OAuth authentication.
- LinkedIn provides an access token after successful authorization.
- Backend securely stores the access token in MongoDB.
- Backend uses this token to publish posts automatically using LinkedIn API.
- Ensures secure and authorized access to user's LinkedIn account.

D. Data Model

User Collection:
- _id
- name
- email
- password
- linkedinAccessToken
- createdAt

Persona Collection:
- _id
- userId
- tone
- style
- experience
- languagePreference
- createdAt

Post Collection:
- _id
- userId
- personaId
- topic
- content
- status (draft, approved, scheduled, published, failed)
- scheduledTime
- createdAt

E. API Endpoints

POST /api/auth/register
POST /api/auth/login

POST /api/linkedin/connect
GET /api/linkedin/status

POST /api/persona
GET /api/persona

POST /api/posts/generate
GET /api/posts

POST /api/posts/approve

POST /api/posts/schedule

POST /api/posts/publish

GET /api/posts/status

F. Scheduling and Publishing Flow

1. User selects and approves a post.
2. User chooses immediate publishing or scheduling.
3. Backend saves scheduling information in MongoDB.
4. Scheduler checks database regularly for scheduled posts.
5. When scheduled time arrives, backend publishes post using LinkedIn API.
6. Post status is updated to "published".

G. Status Lifecycle

draft → approved → scheduled → published → failed

H. Scalability and Reliability Considerations

- Backend handles multiple users and posts efficiently.
- Scheduling runs independently from main API to avoid blocking requests.
- MongoDB indexes improve performance.
- Secure token storage ensures safe LinkedIn integration.
- System maintains post history and status tracking.


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

Solution:

A. Minimal User Flow

1. User uploads a DOCX template file.
2. System analyzes the template and detects editable fields.
3. User reviews and confirms detected fields.
4. User enters field values manually or uploads Excel/CSV file for bulk generation.
5. System generates DOCX/PDF documents.
6. User downloads generated document or ZIP file for bulk generation.

B. High-Level Architecture

Frontend (React.js):
- Allows user to upload DOCX template
- Displays detected fields
- Allows manual input or Excel upload
- Provides download links

Backend (Node.js + Express.js):
- Handles template upload
- Parses DOCX file and detects fields
- Generates documents by replacing template fields
- Handles bulk document generation

Database (MongoDB):
- Stores template metadata
- Stores field definitions
- Stores bulk generation job status

Storage:
- Stores uploaded templates
- Stores generated documents
- Stores ZIP files

Processing Module:
- Parses DOCX templates
- Replaces placeholders with actual values
- Generates final DOCX or PDF files

Flow:
React Frontend → Express Backend → Template Parser → MongoDB → Document Generator → Storage → Frontend

C. Data Model

Template Collection:
- _id
- name
- fileUrl
- fields
- createdAt

TemplateField Collection:
- _id
- templateId
- fieldName
- fieldType
- required

BulkRun Collection:
- _id
- templateId
- status
- createdAt

RowResult Collection:
- _id
- bulkRunId
- status
- fileUrl
- errorMessage

Artifact Collection:
- _id
- fileUrl
- type (docx, pdf, zip)
- createdAt

D. API Endpoints

POST /api/templates/upload
GET /api/templates

GET /api/templates/:id

POST /api/templates/:id/generate

POST /api/templates/:id/bulk-generate

GET /api/jobs/:id/status

GET /api/download/:id

E. Template Field Detection

- Backend analyzes DOCX template
- Detects placeholders like {{name}}, {{date}}
- Stores field names in database
- User confirms field mapping

F. Bulk Generation Flow

1. User uploads Excel file
2. Backend reads each row
3. Replaces template fields with row data
4. Generates document for each row
5. Stores generated documents
6. Creates ZIP file for download

G. Output Format

Single generation:
- Generated DOCX or PDF file

Bulk generation:
- ZIP file containing documents
- Generation report with success/failure status

H. Scalability Considerations

- Bulk processing handled asynchronously
- Storage separated from backend
- MongoDB tracks job progress
- System handles multiple templates and users


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

Solution:

A. Minimal User Flow

1. User creates characters with image, personality, and voice settings.
2. User defines relationships between characters.
3. User provides episode story prompt and selects characters.
4. System generates script, scenes, and asset plan.
5. System generates visual assets and voiceover.
6. System combines assets and generates final video.
7. User downloads video and supporting assets.

B. High-Level Architecture

Frontend (React.js):
- Allows user to create and manage characters
- Allows user to create episodes
- Displays generated scripts and videos
- Provides download options

Backend (Node.js + Express.js):
- Handles character and episode creation
- Stores character and episode data
- Generates scripts and scenes using AI
- Manages asset generation and video rendering

Database (MongoDB):
- Stores character data
- Stores relationships
- Stores episode data
- Stores asset references

Storage:
- Stores character images
- Stores generated assets
- Stores final videos

Processing Pipeline:
- Script Generator
- Scene Generator
- Asset Generator
- Voice Generator
- Video Renderer

Flow:
React Frontend → Express Backend → MongoDB → AI Services → Asset Generator → Video Renderer → Storage → Frontend

C. Data Model

Character Collection:
- _id
- name
- personality
- imageUrl
- voiceSettings
- createdAt

Relationship Collection:
- _id
- character1Id
- character2Id
- relationshipType

Episode Collection:
- _id
- title
- prompt
- characterIds
- script
- status
- createdAt

Scene Collection:
- _id
- episodeId
- sceneNumber
- description
- dialogue

Asset Collection:
- _id
- episodeId
- type (image, audio, video)
- fileUrl

D. Pipeline Flow

1. User provides story prompt
2. Backend generates script
3. Script is divided into scenes
4. Assets are generated for each scene
5. Voiceover is generated
6. Video renderer combines assets into final video
7. Video stored and available for download

E. Consistency Strategy

- Character data stored in MongoDB
- Same character image and voice reused
- Character personality stored and reused
- Relationships stored and reused

This ensures consistency across episodes.

F. MVP Scope

MVP Features:
- Character creation
- Episode script generation
- Scene generation
- Asset generation
- Video rendering

Future Enhancements:
- Advanced video editing
- Real-time preview
- Multiple animation styles

G. API Endpoints

POST /api/characters
GET /api/characters

POST /api/episodes
GET /api/episodes

GET /api/episodes/:id

POST /api/episodes/:id/generate

GET /api/assets/:id

GET /api/download/:id

H. Scalability Considerations

- Asset generation handled asynchronously
- Storage separated from backend
- MongoDB stores asset references
- System supports multiple characters and episodes
