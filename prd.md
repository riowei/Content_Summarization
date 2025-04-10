# Product Requirements Document (PRD): ContentDigest SaaS Application

## 1. Introduction

### 1.1 Overview
ContentDigest is a SaaS application designed to help users efficiently process and manage online content. Users can input links to articles, YouTube videos, and research papers, and the app will use AI to generate concise summaries. Additionally, it provides a relevance-based search functionality to explore stored content, enabling users to quickly find and review key insights.

### 1.2 Purpose
This PRD outlines the requirements for building an MVP of ContentDigest, focusing on core functionalities to validate the concept with a small user base (100-500 users). The goal is to deliver a functional product within a short development timeline (4 weeks) for a time-constrained engineer.

### 1.3 Target Audience
- Busy professionals and students who need quick insights from diverse content.
- Initial beta testers from tech forums and organic outreach.

## 2. Goals and Objectives

### 2.1 Goals
- Allow users to submit links to articles, YouTube videos, and research papers for summarization.
- Use AI to generate concise summaries (~150 words) of submitted content.
- Provide a basic keyword search to find relevant summaries.
- Deliver an MVP within 4 weeks for beta testing.

### 2.2 Success Metrics
- Process at least 100 links in the first month of beta.
- Achieve a user retention rate of 30% (monthly active users).
- Receive positive feedback on summary quality (via user surveys).

## 3. Features and Requirements

### 3.1 Core Features (MVP)

#### 3.1.1 Content Input
Description: Users can submit links to content for summarization.

Requirements:
- Single text field for URL input (no drag-and-drop in MVP).
- Supported content types:
  - Articles (web URLs).
  - YouTube videos (video URLs).
  - Research papers (PDF URLs or direct uploads).
- Basic validation to ensure the link/file is accessible.
- Feedback: Display "Processing..." status during summarization.

#### 3.1.2 AI-Powered Summarization
Description: AI generates summaries of submitted content.

Requirements:
- Fixed summary length: ~150 words, focusing on key points.
- Articles and research papers: Use BERT-based extractive summarization.
- YouTube videos: Fetch transcripts via YouTube API; skip videos without transcripts in MVP.
- Processing: Queued (1-5 minute delay acceptable).
- Store summaries with metadata (e.g., original link, date added).

#### 3.1.3 Relevance Search
Description: Users can search through their stored summaries.

Requirements:
- Basic keyword search across summaries and content titles.
- Display results as a list with summary snippets and links to original content.
- No filters or advanced ranking in MVP.

#### 3.1.4 User Accounts
Description: Basic user management for accessing the app.

Requirements:
- Sign-up/login with email and password.
- Dashboard showing a list of all submitted content and summaries.

### 3.2 Non-Functional Requirements
- Performance: Summarization completes within 1-5 minutes.
- Scalability: Support 100-500 users initially.
- Security: Basic data privacy (secure storage of user data).
- Language: English-only content for MVP.

## 4. User Stories
- As a user, I want to paste a URL into the app so that I can get a summary of the content without reading/watching it fully.
- As a user, I want to see a list of all my submitted content and summaries so that I can easily review them.
- As a user, I want to search for keywords in my summaries so that I can quickly find relevant information.
- As a user, I want to sign up and log in so that my content is saved securely.

## 5. Technical Requirements

### 5.1 Tech Stack
- Frontend: React.js (Create React App) with basic CSS.
- Backend: Python + Flask, SQLite for database.
- AI: Hugging Face Transformers (BERT for summarization).
- APIs:
  - YouTube Data API for video transcripts.
  - BeautifulSoup for web scraping articles.
- Hosting: Heroku or Vercel (free tier for MVP).

### 5.2 API Endpoints
- POST /submit-link: Submit a URL for summarization.
- GET /summaries: Retrieve user's summaries.
- GET /search: Search summaries by keyword.

### 5.3 Infrastructure
- Use Heroku for quick deployment.
- SQLite for lightweight database needs (user data, summaries).
- No advanced scaling in MVP (revisit after beta).

## 6. Development Timeline

| Week | Tasks |
|------|-------|
| Week 1 | Set up frontend (input form, dashboard) and backend (API endpoints). |
| Week 2 | Integrate AI summarization (BERT for text, YouTube API for videos). |
| Week 3 | Add search functionality and user authentication. |
| Week 4 | Testing, bug fixes, and deployment. |

## 7. Future Enhancements (Post-MVP)
- Add drag-and-drop for link/file submission.
- Introduce filters and advanced search (e.g., by content type, date).
- Allow customization of summary length and focus.
- Optimize summarization speed (e.g., real-time processing).
- Add multi-language support and more content types (e.g., podcasts).

## 8. Risks and Mitigations
Risk: Slow summarization delays user experience.
- Mitigation: Set clear expectations (1-5 minute delay) and optimize in future iterations.

Risk: YouTube videos without transcripts can't be processed.
- Mitigation: Skip such videos in MVP; add speech-to-text later.

Risk: Limited user adoption.
- Mitigation: Focus on a small, targeted beta group and iterate based on feedback.

## 9. Appendix

### Assumptions:
- Users are comfortable with a 1-5 minute delay for summarization.
- Most YouTube videos have transcripts available.

### Dependencies:
- YouTube Data API access.
- Hugging Face Transformers library for AI summarization.
