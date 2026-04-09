flowchart TD
    A[Landing Page]
    B[Sign Up / Sign In through Clerk]
    C[Email Verification and Password Recovery]
    D[Connect Meta Ads Account via OAuth]
    E[Main Dashboard]
    F[Campaign Creation Wizard]
    G[Define Campaign Objective and Audience]
    H[Set Budget and Schedule]
    I[Add or Generate Creative Assets using Upload or AI Sora]
    J[Generate Ad Copy and Headlines using GPT-4]
    K[Review Ad Variations using 3x2x2 Preview]
    L[Submit Campaign for Approval or Launch]
    M[Meta Marketing API Processing]
    N[Campaign Launched]
    O[Performance Dashboard with Metrics]
    P[AI-Driven Recommendations]
    Q[Account Settings and Management]
    R[Error and Alert Handling]
    S[AI Assistant Chat]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> L
    L --> M
    M --> N
    N --> O
    O --> P
    O --> Q
    E --> S
    S --> Q
    R -.-> E
