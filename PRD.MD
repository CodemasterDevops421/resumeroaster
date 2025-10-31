# PRD.md - ResumeRoast.io Product Requirements Document

```markdown
# Product Requirements Document: ResumeRoast.io
**Version:** 1.0  
**Last Updated:** October 25, 2025  
**Owner:** Product Team  
**Status:** Ready for Development

---

## 1. EXECUTIVE SUMMARY

### 1.1 Product Vision
ResumeRoast.io is a SaaS platform that provides brutally honest, AI-powered resume analysis and ATS optimization. We help job seekers understand exactly why their resumes aren't getting interviews and provide actionable fixes.

### 1.2 Business Objectives
- **Primary Goal:** Generate $10K MRR within 6 months
- **User Acquisition:** 10,000 registered users in first 3 months
- **Conversion Rate:** 5% free-to-paid conversion
- **Revenue Streams:** 
  - One-time purchases (60% of revenue)
  - Monthly subscriptions (40% of revenue)
- **Market Position:** Premium AI resume optimization tool

### 1.3 Success Metrics
| Metric | Target | Timeframe |
|--------|--------|-----------|
| User Signups | 10,000 | 3 months |
| Paid Users | 500 | 3 months |
| MRR | $10,000 | 6 months |
| Average Roast Rating | 4.5/5 | Ongoing |
| Free-to-Paid Conversion | 5% | 3 months |
| Churn Rate | <5% | Monthly |
| NPS Score | >50 | Ongoing |

---

## 2. PROBLEM STATEMENT

### 2.1 User Problems
1. **Job seekers don't know why their resumes fail**
   - 75% of resumes are rejected by ATS before human review
   - Users receive no feedback on rejections
   - Generic advice doesn't address specific issues

2. **Existing solutions are inadequate**
   - Resume reviewers are expensive ($100-500)
   - Generic AI tools lack personality and specificity
   - No instant feedback mechanism

3. **ATS optimization is confusing**
   - Users don't understand keyword optimization
   - Formatting issues invisible to users
   - No clear before/after guidance

### 2.2 Target Audience

**Primary Persona: "Desperate Dave"**
- Age: 25-35
- Job Status: Actively job hunting (3+ months)
- Pain: Applied to 50+ jobs, 2 interviews
- Willingness to Pay: High (job = income)
- Tech Savviness: Medium
- Income: $40K-$80K

**Secondary Persona: "Career Climber Claire"**
- Age: 28-40
- Job Status: Passively looking for better role
- Pain: Not getting responses from dream companies
- Willingness to Pay: Medium
- Tech Savviness: High
- Income: $80K-$150K

**Tertiary Persona: "Fresh Graduate Frank"**
- Age: 21-24
- Job Status: Entry-level job search
- Pain: No experience, doesn't know how to present skills
- Willingness to Pay: Low-Medium
- Tech Savviness: High
- Income: $0-$40K

---

## 3. PRODUCT FEATURES

### 3.1 Core Features (MVP - Phase 1)

#### 3.1.1 User Authentication
**Priority:** P0 (Critical)  
**User Story:** As a user, I want to create an account so I can save my roasts and track progress.

**Requirements:**
- Email/password registration
- Google OAuth integration
- Email verification required
- Password reset flow
- Session management (JWT tokens)
- Account deletion capability

**Acceptance Criteria:**
- [ ] User can sign up with email in <30 seconds
- [ ] Email verification sent within 1 minute
- [ ] OAuth completes in <5 seconds
- [ ] Password reset email delivered in <2 minutes
- [ ] Sessions persist for 30 days
- [ ] GDPR-compliant data deletion

---

#### 3.1.2 Resume Upload System
**Priority:** P0 (Critical)  
**User Story:** As a user, I want to upload my resume easily so I can get it analyzed.

**Requirements:**
- Drag-and-drop interface
- File type support: PDF, DOCX, TXT
- Maximum file size: 10MB
- Client-side validation
- Virus scanning (ClamAV)
- Automatic text extraction
- Preview before submission
- Mobile-responsive upload

**Technical Specifications:**
- Cloud Storage bucket: `gs://resumeroast-uploads-prod`
- File naming: `{user_id}/{timestamp}_{original_filename}`
- Retention policy: 90 days
- Encryption: AES-256 at rest
- CDN: Google Cloud CDN

**Acceptance Criteria:**
- [ ] Supports PDF, DOCX, TXT formats
- [ ] Upload completes in <10 seconds for 5MB file
- [ ] Text extraction accuracy >95%
- [ ] Error messages clear for unsupported formats
- [ ] Works on mobile devices
- [ ] Shows upload progress indicator

**Edge Cases:**
- Password-protected PDFs → Show error: "Remove password protection"
- Scanned images → Use OCR with Gemini Vision
- Multi-column resumes → Parse correctly with layout analysis
- Non-English resumes → Detect language, support top 10 languages

---

#### 3.1.3 Job Description Input
**Priority:** P0 (Critical)  
**User Story:** As a user, I want to paste a job description so the AI can compare my resume against it.

**Requirements:**
- Large text area (unlimited characters)
- Paste from clipboard (Ctrl+V)
- Auto-detect job title
- URL input option (scrape job posting)
- Character count display
- Save common job descriptions

**Technical Specifications:**
- Web scraping: Puppeteer for LinkedIn, Indeed, Glassdoor
- Rate limiting: 10 scrapes per user per day
- Store in Firestore: `jobDescriptions` collection

**Acceptance Criteria:**
- [ ] Text area accepts >10,000 characters
- [ ] URL scraping works for top 5 job boards
- [ ] Scraping completes in <15 seconds
- [ ] Auto-save draft every 30 seconds
- [ ] Character count updates in real-time

---

#### 3.1.4 AI Roast Generation
**Priority:** P0 (Critical)  
**User Story:** As a user, I want to receive brutally honest feedback so I know exactly what's wrong with my resume.

**Requirements:**

**Roast Components:**
1. **Overall Score (1-10)**
   - Weighted algorithm: 40% ATS compatibility, 30% content quality, 30% formatting

2. **Brutal Roast Section**
   - 3-4 sentences
   - Tone: Honest, funny, constructive
   - Personality: Gordon Ramsay meets career coach

3. **ATS Compatibility Score (1-10)**
   - Keyword match percentage
   - Formatting issues detected
   - File type compatibility

4. **Critical Issues (5-7 bullets)**
   - Specific, actionable problems
   - Prioritized by impact

5. **Keyword Analysis**
   - Missing keywords from job description
   - Keyword density comparison
   - Synonym suggestions

6. **Clichés to Delete**
   - "Team player", "Hard worker", "Results-driven"
   - 5-10 specific instances found

7. **Power Verbs to Add**
   - 10 role-specific action verbs
   - Replace weak verbs

8. **Formatting Fixes**
   - Font consistency
   - Margin issues
   - Bullet point alignment
   - Section order optimization

9. **Quick Wins**
   - Top 3 easiest high-impact changes
   - Estimated time to fix each

**Technical Specifications:**

**Gemini API Configuration:**
```json
{
  "model": "gemini-1.5-flash",
  "temperature": 0.7,
  "top_p": 0.9,
  "top_k": 40,
  "max_output_tokens": 4096,
  "safety_settings": [
    {
      "category": "HARM_CATEGORY_HATE_SPEECH",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    }
  ]
}
```

**Prompt Template:**

```
You are ResumeRoast AI, a brutally honest resume critic with 20 years of HR experience and deep knowledge of ATS systems. Your personality is sharp, witty, and constructive - like Gordon Ramsay for resumes.

CONTEXT:
- Candidate is applying for: {job_title}
- Industry: {industry}
- Experience level: {experience_level}

RESUME TEXT:
{resume_text}

JOB DESCRIPTION:
{job_description}

ANALYSIS REQUIRED:
Provide a comprehensive roast following this EXACT JSON structure:

{
  "overall_score": <number 1-10>,
  "overall_score_reasoning": "<2 sentence explanation>",
  
  "brutal_roast": "<3-4 sentences, funny but constructive>",
  
  "ats_score": <number 1-10>,
  "ats_issues": [
    "<specific issue 1>",
    "<specific issue 2>"
  ],
  
  "critical_issues": [
    {
      "issue": "<problem>",
      "impact": "<high|medium|low>",
      "fix": "<specific solution>"
    }
  ],
  
  "keyword_analysis": {
    "missing_keywords": ["<keyword1>", "<keyword2>"],
    "keyword_density": <percentage>,
    "recommended_keywords": ["<keyword1>", "<keyword2>"]
  },
  
  "cliches_found": [
    {
      "phrase": "<cliché>",
      "location": "<where found>",
      "replacement": "<better alternative>"
    }
  ],
  
  "power_verbs": [
    {
      "verb": "<action verb>",
      "usage_example": "<how to use it>"
    }
  ],
  
  "formatting_fixes": [
    {
      "issue": "<formatting problem>",
      "fix": "<specific solution>"
    }
  ],
  
  "quick_wins": [
    {
      "change": "<specific action>",
      "impact": "<expected result>",
      "time_required": "<minutes>"
    }
  ],
  
  "before_after_example": {
    "before": "<weak resume line>",
    "after": "<improved version>",
    "explanation": "<why it's better>"
  }
}

TONE GUIDELINES:
- Be honest but not cruel
- Use humor but stay professional
- Focus on actionable feedback
- Avoid generic advice
- Reference specific content from the resume

Return ONLY valid JSON, no additional text.
```

**Processing Flow:**

1. User uploads resume + job description
1. Extract text (PDF/DOCX parsing)
1. Pre-process: Clean formatting, detect sections
1. Call Gemini API with structured prompt
1. Parse JSON response
1. Validate response structure
1. Store in Firestore
1. Return to frontend
1. Log analytics event

**Performance Requirements:**

- Response time: <15 seconds for 90th percentile
- Timeout: 30 seconds
- Retry logic: 3 attempts with exponential backoff
- Fallback: If Gemini fails, return generic analysis

**Quality Assurance:**

- Human review of 10% of roasts (random sampling)
- User feedback rating required
- Flag low-rated roasts for review
- A/B test different prompt variations

**Acceptance Criteria:**

- [ ] 95% of roasts complete in <15 seconds
- [ ] JSON parsing success rate >99%
- [ ] Average user rating >4.5/5
- [ ] Roast tone appropriate (not offensive)
- [ ] All 9 components present in response
- [ ] Mobile display renders correctly

-----

#### 3.1.5 Roast Results Display

**Priority:** P0 (Critical)  
**User Story:** As a user, I want to see my roast results in an easy-to-read format so I can take action immediately.

**Requirements:**

**UI Components:**

1. **Hero Section**

- Large score display (1-10) with color coding:
  - 8-10: Green (Excellent)
  - 6-7: Yellow (Good)
  - 4-5: Orange (Needs Work)
  - 1-3: Red (Critical)
- Animated score counter
- Share score on Twitter/LinkedIn

1. **Brutal Roast Card**

- Prominent display
- Fun icon (🔥 flame)
- Shareable quote card generation

1. **Tabbed Navigation**

- Tab 1: Critical Issues
- Tab 2: ATS Analysis
- Tab 3: Keywords
- Tab 4: Writing Improvements
- Tab 5: Quick Wins

1. **Action Items Checklist**

- Interactive checkboxes
- Progress bar showing completion
- Estimated total time to fix all issues

1. **Before/After Examples**

- Side-by-side comparison
- Highlight changes

1. **Download Options**

- PDF report (branded)
- Plain text summary
- Checklist as CSV

1. **Social Sharing**

- Twitter: Share score + roast snippet
- LinkedIn: Share improvement journey
- Copy link to roast

**Acceptance Criteria:**

- [ ] Page loads in <2 seconds
- [ ] All components render correctly on mobile
- [ ] PDF downloads in <5 seconds
- [ ] Social sharing generates correct image
- [ ] Checklist state persists across sessions

-----

#### 3.1.6 Payment System

**Priority:** P0 (Critical)  
**User Story:** As a user, I want to purchase roasts easily and securely so I can optimize my resume.

**Pricing Tiers:**

|Tier       |Price    |Credits  |Features                                           |
|-----------|---------|---------|---------------------------------------------------|
|**Free**   |$0       |1 roast  |Basic roast, limited analysis                      |
|**Starter**|$9.99    |5 roasts |Full roast, PDF download                           |
|**Pro**    |$19.99/mo|Unlimited|All features, priority support, comparison tracking|

**Payment Flow:**

1. User selects tier
1. Stripe Checkout redirect
1. Payment processing
1. Webhook confirms payment
1. Credits added to Firestore
1. Confirmation email sent
1. Redirect to dashboard

**Stripe Integration:**

- Payment methods: Card, Apple Pay, Google Pay
- Currency: USD (expand to EUR, GBP later)
- Tax calculation: Stripe Tax
- Invoicing: Automatic
- Refund policy: 7 days, unused credits only

**Subscription Management:**

- Auto-renewal
- Cancel anytime
- Prorated refunds
- Upgrade/downgrade flow
- Payment method update
- Failed payment handling (retry 3 times)

**Technical Specifications:**

- Stripe API version: Latest (2024+)
- Webhook endpoint: `/api/webhooks/stripe`
- Signature verification: Required
- Idempotency keys: Generated per request
- PCI compliance: Stripe handles (Level 1)

**Security:**

- No card data stored on our servers
- Stripe Elements for card input
- 3D Secure (SCA) compliance
- Fraud detection: Stripe Radar

**Acceptance Criteria:**

- [ ] Payment completes in <10 seconds
- [ ] Webhook processes in <5 seconds
- [ ] Credits appear immediately after payment
- [ ] Confirmation email sent within 1 minute
- [ ] Refund processes in <24 hours
- [ ] Subscription cancellation immediate
- [ ] Failed payment retry logic works

-----

#### 3.1.7 User Dashboard

**Priority:** P1 (High)  
**User Story:** As a user, I want to view my past roasts so I can track my resume improvements over time.

**Features:**

1. **Roast History**

- List view of all roasts
- Sort by: Date, Score, Job Title
- Filter by: Date range, Score range
- Search by job title/company

1. **Credits Display**

- Current credits remaining
- Credit usage history
- Purchase more CTA

1. **Progress Tracking**

- Score improvement graph
- Before/after comparison
- Milestone achievements

1. **Account Settings**

- Email preferences
- Password change
- Subscription management
- Delete account

1. **Quick Actions**

- Start new roast
- Re-roast with updates
- Download all reports (batch)

**Acceptance Criteria:**

- [ ] Dashboard loads in <3 seconds
- [ ] Roast history shows last 100 roasts
- [ ] Pagination works smoothly
- [ ] Graph renders correctly
- [ ] Settings save immediately

-----

### 3.2 Advanced Features (Phase 2)

#### 3.2.1 Roast Comparison

**Priority:** P2 (Medium)  
**Release:** Month 3

**User Story:** As a user, I want to compare multiple versions of my resume so I can see which performed better.

**Features:**

- Side-by-side comparison view
- Highlight score differences
- Track which changes improved score
- A/B test different resume versions

-----

#### 3.2.2 LinkedIn Profile Optimizer

**Priority:** P2 (Medium)  
**Release:** Month 4

**User Story:** As a user, I want to optimize my LinkedIn profile to match my resume improvements.

**Features:**

- Import LinkedIn profile (OAuth)
- Analyze headline, summary, experience
- Suggest keyword optimization
- Generate optimized profile copy

-----

#### 3.2.3 Cover Letter Generator

**Priority:** P2 (Medium)  
**Release:** Month 4

**User Story:** As a user, I want to generate a tailored cover letter based on my roasted resume.

**Features:**

- Auto-generate from resume + job description
- Multiple tone options (formal, casual, creative)
- Incorporate roast feedback
- 3 versions per generation

-----

#### 3.2.4 Interview Prep

**Priority:** P3 (Low)  
**Release:** Month 6

**User Story:** As a user, I want interview questions based on my resume so I can prepare better.

**Features:**

- Generate likely interview questions
- Suggest STAR method answers
- Identify resume weak points interviewers might probe
- Mock interview practice mode

-----

#### 3.2.5 Team/Agency Features

**Priority:** P3 (Low)  
**Release:** Month 6

**User Story:** As a career coach, I want to roast multiple client resumes under one account.

**Features:**

- Team accounts (5-50 users)
- Bulk upload (CSV of resumes)
- White-label reports
- Admin dashboard
- Client management
- Usage analytics

**Pricing:**

- Team Plan: $99/month (50 roasts)
- Agency Plan: $299/month (200 roasts)
- Enterprise: Custom pricing

-----

### 3.3 Non-Functional Requirements

#### 3.3.1 Performance

- **Page Load:** <2 seconds (95th percentile)
- **Roast Generation:** <15 seconds (90th percentile)
- **API Response Time:** <500ms (median)
- **Uptime:** 99.9% (8.76 hours downtime/year max)
- **Concurrent Users:** Support 1,000 simultaneous roasts

#### 3.3.2 Security

- **Authentication:** JWT tokens, 30-day expiry
- **Data Encryption:**
  - In transit: TLS 1.3
  - At rest: AES-256
- **API Rate Limiting:**
  - Anonymous: 5 requests/hour
  - Free tier: 10 requests/hour
  - Paid tier: 100 requests/hour
- **File Scanning:** ClamAV for malware
- **SQL Injection:** Parameterized queries (Firestore)
- **XSS Protection:** Content Security Policy
- **CSRF:** Token-based protection
- **PII Protection:** Anonymize data for analytics

#### 3.3.3 Compliance

- **GDPR:**
  - Right to access data
  - Right to deletion (within 30 days)
  - Data portability
  - Cookie consent
- **CCPA:** California privacy rights
- **SOC 2:** Compliance target by Month 6
- **Privacy Policy:** Hosted at `/privacy`
- **Terms of Service:** Hosted at `/terms`

#### 3.3.4 Scalability

- **Horizontal Scaling:** Google Cloud Run (auto-scale)
- **Database:** Firestore (scales automatically)
- **File Storage:** Cloud Storage (unlimited)
- **CDN:** Cloud CDN for static assets
- **Caching:** Redis for session/API responses
- **Queue System:** Cloud Tasks for async jobs

#### 3.3.5 Monitoring & Observability

- **APM:** Google Cloud Monitoring
- **Error Tracking:** Sentry
- **Logging:** Cloud Logging (retention: 30 days)
- **Alerts:**
  - API error rate >5%
  - Response time >2 seconds
  - Payment failure rate >10%
  - Server uptime <99.9%
- **Analytics:**
  - Google Analytics 4
  - Custom events: roast_completed, payment_successful
  - Funnel tracking: signup → upload → roast → payment

#### 3.3.6 Accessibility

- **WCAG 2.1 Level AA Compliance**
- Screen reader compatible
- Keyboard navigation
- Color contrast ratio >4.5:1
- Alt text for all images
- ARIA labels

#### 3.3.7 Mobile Responsiveness

- **Breakpoints:**
  - Mobile: 320px - 767px
  - Tablet: 768px - 1023px
  - Desktop: 1024px+
- **Touch Targets:** Minimum 44x44px
- **Mobile Upload:** Native camera integration
- **Progressive Web App:** Installable on mobile

-----

## 4. USER FLOWS

### 4.1 First-Time User Journey

```
1. Land on homepage
   ↓
2. Click "Roast My Resume" CTA
   ↓
3. Sign up (email or Google OAuth)
   ↓
4. Email verification
   ↓
5. Upload resume
   ↓
6. Paste job description
   ↓
7. Click "Roast It" button
   ↓
8. Loading screen (10-15 seconds)
   ↓
9. View roast results
   ↓
10. Download PDF report
   ↓
11. Prompted to upgrade (used free roast)
   ↓
12. Purchase 5 roasts for $9.99
   ↓
13. Confirmation email
   ↓
14. Dashboard with credits
```

**Drop-off Points to Optimize:**

- Step 3: 30% abandon at signup → Reduce friction
- Step 5: 20% abandon at upload → Improve UX
- Step 11: 85% don’t convert → Better upsell copy

-----

### 4.2 Returning User Journey (Paid)

```
1. Login
   ↓
2. Dashboard (view past roasts)
   ↓
3. Click "New Roast"
   ↓
4. Upload updated resume
   ↓
5. Select previous job description or new one
   ↓
6. Generate roast
   ↓
7. Compare with previous roast
   ↓
8. Download updated report
```

-----

### 4.3 Payment Flow

```
1. User clicks "Buy 5 Roasts"
   ↓
2. Redirect to Stripe Checkout
   ↓
3. Enter payment details
   ↓
4. Complete payment
   ↓
5. Stripe webhook triggers
   ↓
6. Credits added to Firestore
   ↓
7. Redirect to dashboard
   ↓
8. Confirmation email sent
   ↓
9. User can immediately use credits
```

-----

## 5. DATA MODEL

### 5.1 Firestore Collections

#### `users` Collection

```json
{
  "userId": "auto-generated-id",
  "email": "user@example.com",
  "displayName": "John Doe",
  "photoURL": "https://...",
  "provider": "google.com", // or "password"
  "emailVerified": true,
  "createdAt": "2025-01-15T10:30:00Z",
  "lastLoginAt": "2025-10-25T14:20:00Z",
  "tier": "free", // free, starter, pro
  "credits": 0,
  "subscriptionId": "sub_123", // Stripe subscription ID
  "stripeCustomerId": "cus_123",
  "preferences": {
    "emailNotifications": true,
    "marketingEmails": false
  },
  "metadata": {
    "totalRoasts": 5,
    "averageScore": 7.2,
    "firstRoastDate": "2025-01-15T11:00:00Z"
  }
}
```

#### `roasts` Collection

```json
{
  "roastId": "auto-generated-id",
  "userId": "user-id-ref",
  "createdAt": "2025-10-25T15:00:00Z",
  "status": "completed", // pending, processing, completed, failed
  
  "input": {
    "resumeUrl": "gs://bucket/path/to/resume.pdf",
    "resumeText": "extracted text...",
    "jobDescription": "job description text...",
    "jobTitle": "Software Engineer",
    "company": "Google",
    "industry": "Technology"
  },
  
  "result": {
    "overallScore": 7,
    "overallScoreReasoning": "...",
    "brutalRoast": "...",
    "atsScore": 8,
    "atsIssues": [...],
    "criticalIssues": [...],
    "keywordAnalysis": {...},
    "clichesFound": [...],
    "powerVerbs": [...],
    "formattingFixes": [...],
    "quickWins": [...],
    "beforeAfterExample": {...}
  },
  
  "metadata": {
    "processingTimeMs": 12500,
    "geminiModel": "gemini-1.5-flash",
    "promptVersion": "v1.2",
    "creditsUsed": 1
  },
  
  "userFeedback": {
    "rating": 5, // 1-5 stars
    "comment": "This was incredibly helpful!",
    "submittedAt": "2025-10-25T15:10:00Z"
  }
}
```

#### `payments` Collection

```json
{
  "paymentId": "auto-generated-id",
  "userId": "user-id-ref",
  "stripePaymentId": "pi_123",
  "type": "one_time", // one_time, subscription
  "amount": 999, // cents
  "currency": "usd",
  "status": "succeeded", // pending, succeeded, failed, refunded
  "creditsAdded": 5,
  "createdAt": "2025-10-25T14:00:00Z",
  "metadata": {
    "plan": "starter",
    "promoCode": null
  }
}
```

#### `subscriptions` Collection

```json
{
  "subscriptionId": "auto-generated-id",
  "userId": "user-id-ref",
  "stripeSubscriptionId": "sub_123",
  "status": "active", // active, canceled, past_due
  "plan": "pro",
  "amount": 1999, // cents
  "billingCycle": "monthly",
  "currentPeriodStart": "2025-10-01T00:00:00Z",
  "currentPeriodEnd": "2025-11-01T00:00:00Z",
  "cancelAtPeriodEnd": false,
  "createdAt": "2025-10-01T12:00:00Z",
  "canceledAt": null
}
```

#### `analytics` Collection (aggregated daily)

```json
{
  "date": "2025-10-25",
  "metrics": {
    "totalRoasts": 150,
    "newUsers": 50,
    "paidConversions": 3,
    "revenue": 2997, // cents
    "averageRoastScore": 6.8,
    "averageProcessingTimeMs": 13200
  }
}
```

-----

### 5.2 Cloud Storage Structure

```
gs://resumeroast-uploads-prod/
├── users/
│   └── {userId}/
│       └── resumes/
│           ├── {timestamp}_original.pdf
│           ├── {timestamp}_original.txt (extracted)
│           └── {timestamp}_report.pdf
└── temp/
    └── {session-id}/ (cleaned up after 24h)
```

**Lifecycle Policies:**

- User resumes: Retain 90 days
- Temp files: Delete after 24 hours
- Reports: Retain 1 year

-----

## 6. API SPECIFICATIONS

### 6.1 REST API Endpoints

**Base URL:** `https://api.resumeroast.io/v1`

#### Authentication

All authenticated endpoints require:

```
Authorization: Bearer {JWT_TOKEN}
```

-----

#### `POST /auth/signup`

Create new user account.

**Request:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "displayName": "John Doe"
}
```

**Response (201):**

```json
{
  "userId": "abc123",
  "email": "user@example.com",
  "token": "jwt_token_here",
  "expiresIn": 2592000,
  "emailVerificationSent": true
}
```

**Errors:**

- `400`: Email already exists
- `400`: Invalid email format
- `400`: Password too weak

-----

#### `POST /auth/login`

User login.

**Request:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response (200):**

```json
{
  "userId": "abc123",
  "email": "user@example.com",
  "token": "jwt_token_here",
  "expiresIn": 2592000,
  "user": {
    "displayName": "John Doe",
    "tier": "pro",
    "credits": 15
  }
}
```

-----

#### `POST /auth/google`

OAuth login with Google.

**Request:**

```json
{
  "idToken": "google_id_token"
}
```

**Response (200):** Same as `/auth/login`

-----

#### `POST /roasts`

Create new roast.

**Request (multipart/form-data):**

```
resume: <file>
jobDescription: <text>
jobTitle: <text> (optional)
company: <text> (optional)
```

**Response (201):**

```json
{
  "roastId": "roast_123",
  "status": "processing",
  "estimatedCompletionTime": 15,
  "pollUrl": "/roasts/roast_123"
}
```

**Errors:**

- `401`: Unauthorized
- `402`: Insufficient credits
- `400`: Invalid file format
- `413`: File too large (>10MB)
- `429`: Rate limit exceeded

-----

#### `GET /roasts/{roastId}`

Get roast details.

**Response (200):**

```json
{
  "roastId": "roast_123",
  "status": "completed",
  "createdAt": "2025-10-25T15:00:00Z",
  "input": {
    "jobTitle": "Software Engineer",
    "company": "Google"
  },
  "result": {
    "overallScore": 7,
    "brutalRoast": "Your resume reads like a robot wrote it...",
    "atsScore": 8,
    "criticalIssues": [...]
    // ... full roast data
  }
}
```

**Errors:**

- `404`: Roast not found
- `403`: Not authorized to view this roast

-----

#### `GET /roasts`

List user’s roasts.

**Query Parameters:**

- `page`: Page number (default: 1)
- `limit`: Results per page (default: 20, max: 100)
- `sortBy`: `date` or `score` (default: `
