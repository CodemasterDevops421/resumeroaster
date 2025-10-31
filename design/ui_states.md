# UI States, Error Copy, and Validation Messaging

## Authentication
- **Empty State:**
  - Placeholder copy: "Enter the email you used to sign up."
- **Validation Errors:**
  - Email: "Enter a valid email address (name@example.com)."
  - Password: "Password must be at least 12 characters with a symbol."
- **Processing State:**
  - Button label changes to "Signing in…" with spinner; disable inputs.
- **Auth Failure:**
  - Toast: "Check your email or password and try again."
- **Email Verification Required:**
  - Inline alert: "Verify your email to access ResumeRoast. Resend link?"

## Resume Upload
- **Empty State:**
  - Instruction: "Drop your resume or browse to upload. Supported: PDF, DOCX, TXT up to 10MB."
- **Client-side Validation Errors:**
  - Unsupported type: "Upload PDF, DOCX, or TXT files only."
  - Oversized file: "Keep uploads under 10MB. Compress the file and try again."
  - Virus scan failed: "We blocked this file for safety. Try a clean copy."
- **Processing State:**
  - Progress bar with copy: "Uploading securely…"
  - Extraction: "Analyzing layout and text…"
- **Server Errors:**
  - Generic: "Something broke on our side. Retry in a moment."
  - Rate limit: "Too many uploads today. Try again after midnight UTC."

## Job Description Input
- **Empty State:**
  - Prompt: "Paste the job description or import a URL for best results."
- **Validation Errors:**
  - Character limit exceeded (soft cap): "Trim the description under 20,000 characters."
  - URL scrape failure: "We couldn't reach that link. Paste the text instead."
- **Processing State:**
  - Copy: "Pulling role details…"

## Roast Results
- **Loading State:**
  - Skeleton panels with copy: "Cooking up your roast…"
- **Error State:**
  - Banner: "We couldn't finish the roast. Your credits were refunded."
  - CTA: "Start another roast".
- **Partial Data:**
  - Notice: "We only parsed part of your resume. Review the sections below and adjust your file."

## History
- **Empty State (no roasts yet):**
  - Copy: "Your roast history will live here. Upload your first resume to see results."
  - CTA: "Upload resume".
- **No Search Results:**
  - Copy: "No roasts match those filters. Clear filters to try again."
- **Error Loading List:**
  - Banner: "We couldn't load your history. Refresh to try again."

## Global Notifications
- **Processing Toast:** "Roast started! We'll ping you when it's ready."
- **Completion Toast:** "Your roast is done. View results now."
- **Timeout:** "This is taking longer than expected. We'll email you when the roast finishes."
