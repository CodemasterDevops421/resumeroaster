# User Flow: Sign-up → Upload → Roast Results → History

```mermaid
flowchart TD
    A[Landing / Marketing Page] --> B{Sign-up Method}
    B -->|Email| C[Email Registration Form]
    B -->|Google OAuth| D[Google Consent Screen]

    C --> E{Form Validation}
    E -->|Valid| F[Create Account]
    E -->|Invalid| C

    F --> G[Send Verification Email]
    G --> H[Verify Email]
    H --> I[First Login]

    D --> J[OAuth Callback]
    J --> I

    I --> K[Dashboard - Empty State]
    K --> L[Upload Resume]
    L --> M{Client-side Validation}
    M -->|Fail| L
    M -->|Pass| N[Upload to Storage]
    N --> O[Extract Text]
    O --> P[Show Preview & Confirm]
    P --> Q{Add Job Description?}
    Q -->|Yes| R[Paste / Scrape Job Description]
    R --> S[Submit Roast Request]
    Q -->|No| S

    S --> T[Processing Queue]
    T --> U[Roast Completed]
    U --> V[Results Screen]
    V --> W[Save & Tag Roast]
    W --> X[History List]
    X -->|Select Roast| V
    X -->|Upload New| L
```

**Key Notes**
- Verification required before first dashboard access for email users.
- Client-side validation covers file type/size and job description length.
- History provides entry point for re-opening past results or starting new roast.
