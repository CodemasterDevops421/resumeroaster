# Product Requirements Document: ResumeRoast.io
**Version:** 1.1 (Draft)
**Last Updated:** October 25, 2025
**Owner:** Product Team
**Status:** Draft — circulate for stakeholder sign-off before implementation.

---

## 1. Executive Summary
ResumeRoast.io delivers brutally honest, AI-powered resume critiques aligned to a pasted job description. The MVP focuses on delivering one high-quality roast per resume/job pairing with persistent history so users can iteratively improve. Advanced concepts (OCR, scraping, multi-language, payments, mobile apps) are deferred to a future phase.

---

## 2. Problem Statement
1. **Lack of actionable feedback:** Job seekers rarely understand why resumes fail ATS filters or recruiters.
2. **Slow, expensive alternatives:** Human resume reviews take days and cost $100+.
3. **Need for repeatable improvement:** Users want a historical trail of past roasts to track progress.

Primary persona: early-career tech or business professional actively applying to roles in English-speaking markets.

---

## 3. MVP Scope
### 3.1 User Journey Overview
1. Sign in (email + password or Google OAuth).
2. Upload a single resume file.
3. Paste a job description manually.
4. Request a roast and wait for completion.
5. Review roast details and access past roasts in history.

### 3.2 Functional Requirements
#### 3.2.1 User Authentication (Sign-In)
- **Flows:**
  - Email/password registration with verification email.
  - Email/password login.
  - Google OAuth login.
  - Password reset via email link.
- **Security:**
  - JWT sessions expiring after 30 days with refresh token rotation.
  - Rate-limit auth endpoints (5 attempts/minute per IP).
  - Store passwords with Argon2id.
- **Acceptance Criteria:**
  - [ ] New users can register and verify within 5 minutes.
  - [ ] Login completes in <3 seconds p95.
  - [ ] Password reset email sent within 2 minutes.
  - [ ] OAuth callback succeeds for Google accounts.

#### 3.2.2 Resume Upload
- **Scope:** Single file upload per roast request.
- **Supported formats:** PDF, DOCX, TXT.
- **Limits:** ≤5 MB, English-language only.
- **Validation & Storage:**
  - Client-side and server-side MIME/type checking.
  - Virus scan before processing (ClamAV or equivalent).
  - Store original file in secure object storage (AES-256 at rest, 30-day retention).
- **Acceptance Criteria:**
  - [ ] Upload completes in <8 seconds p95 for 5 MB.
  - [ ] Unsupported formats rejected with actionable error.
  - [ ] Duplicate uploads allowed (versioning handled in history).

#### 3.2.3 Job Description Input (Manual Paste)
- **Scope:** Free-form text area supporting up to 15,000 characters.
- **Features:**
  - Plain text paste only (no URL scraping).
  - Character count with warning at 14,000 characters.
  - Auto-save draft per session in local storage.
- **Acceptance Criteria:**
  - [ ] Users can paste full JD without truncation.
  - [ ] Character count updates in real time (<100ms latency client-side).

#### 3.2.4 Roast Generation
- **Flow:**
  - User submits resume file + pasted JD.
  - System queues roast request and processes asynchronously (target completion <30 seconds p95).
- **Output:**
  - Overall score (1–10).
  - Brutal roast paragraph (3–4 sentences, English).
  - ATS compatibility notes (formatting + keyword gaps).
  - Top 5 critical issues with actionable guidance.
  - Suggested keywords to add (up to 10).
- **System Requirements:**
  - Use Gemini 1.5 Flash (or latest approved model) via secure API key.
  - Store generated roast JSON linked to user account.
  - Expose webhook-safe status polling endpoint (`GET /roasts/{id}`).
- **Acceptance Criteria:**
  - [ ] Roast response stored and retrievable within 30 seconds p95.
  - [ ] Users receive clear status feedback (processing → complete or error).
  - [ ] Errors (invalid file, processing failure) surface within 5 seconds with retry guidance.

#### 3.2.5 Roast History View
- **Scope:** Paginated list of a user’s roasts with ability to open detail view.
- **Features:**
  - Display resume filename, job title (if parsed or provided), request timestamp, status, and overall score.
  - Detail view shows full roast output and allows downloading original resume.
  - Sorting by newest-first; basic search by job title keyword.
- **Acceptance Criteria:**
  - [ ] History loads in <2 seconds p95 for first page (20 items).
  - [ ] Detail view renders full roast without manual refresh.
  - [ ] Access restricted to authenticated user who created roast.

---

## 4. Non-Goals
- OCR for scanned resumes.
- Resume parsing for multi-column or non-English formats.
- Automatic job scraping or URL ingestion.
- Payment processing, subscriptions, or credit systems.
- Mobile apps (iOS/Android) or dedicated tablet UI.
- Team/multi-user workspaces or collaboration features.

---

## 5. MVP Success Metrics
| Metric | Target | Measurement Window |
|--------|--------|--------------------|
| Time-to-first-roast | ≤10 minutes from sign-up | First week post-launch |
| Roast completion rate | ≥90% of submitted requests | Rolling 30 days |
| Average roast turnaround | ≤30 seconds p95 | Rolling 7 days |
| DAU-to-signup ratio | ≥25% | First month |
| CSAT on roast quality | ≥4.2/5 | First month |

---

## 6. Dependencies & Integrations
- **Gemini API:** Primary LLM for roast generation; requires service account credentials and quota management.
- **Object Storage:** Google Cloud Storage bucket `gs://resumeroast-uploads-prod` (IAM-scoped to app service account).
- **Email Service:** Transactional email provider (SendGrid or SES) for verification/reset flows.
- **Auth Provider:** Google OAuth client credentials.
- **Virus Scanning:** ClamAV service or managed equivalent (must support scanning up to 5 MB files with <5s SLA).
- **Monitoring:** Centralized logging/metrics stack (Stackdriver, Datadog, or equivalent) to track success metrics.

Dependencies must be provisioned and secrets managed via secure vault prior to QA.

---

## 7. Risks & Mitigations
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Gemini API latency spikes | Medium | High | Implement async queue + retry with exponential backoff; monitor SLA. |
| Resume text extraction failures | Low | Medium | Use proven libraries (textract/docx2txt); log failures and allow user resubmission. |
| Auth abuse (credential stuffing) | Medium | Medium | Enforce rate limits, CAPTCHA after repeated failures, monitor anomalies. |
| Storage cost overruns | Low | Medium | Apply 30-day lifecycle policy and gzip text extracts. |
| Compliance issues | Low | High | Encrypt data at rest/in transit, document data retention, run privacy review before launch. |

---

## 8. Implementation & Approvals
- Draft must be circulated to Product, Engineering, and Compliance leads for sign-off before development starts.
- Post-approval, freeze MVP scope; changes require change-control review.
- Target MVP launch: within 8 weeks of approval.

---

## Appendix A: Future Phase Opportunities
1. OCR for scanned resumes and image-to-text conversion.
2. Automated JD ingestion via scraping and integrations with job boards.
3. Multi-language resume and JD support (top 10 global languages).
4. Native mobile applications (iOS and Android).
5. Subscription billing, credit packages, and enterprise plans.
6. Collaboration features (shared workspaces, recruiter dashboards).
7. Advanced analytics (trend charts, improvement tracking over time).
