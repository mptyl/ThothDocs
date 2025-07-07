graph TD
    A[Start: User Question] --> B[Initialize System State]
    B --> C[Check for Cancellation]
    C -->|Cancelled| D[Yield: Processing Interrupted]
    C -->|Not Cancelled| E[Set Vector DB]
    E --> F[Check Vector DB Initialization]
    F -->|Failed| G[Yield: Failed to Initialize Vector DB]
    F -->|Success| H[Get Tentative Schema]
    H --> I[Select SQL Generator Strategy]
    I -->|Basic or Titanic| J[Setup State with DB Manager, Schema, Question]
    J --> K[Validate Question]
    K -->|Validation Failed| L[Yield: Question Validation Failed]
    K -->|Validation Passed| M[Extract Keywords]
    M --> N[Check for Cancellation]
    N -->|Cancelled| O[Yield: Processing Interrupted]
    N -->|Not Cancelled| P[Retrieve Hints from Vector DB]
    P --> Q[Retrieve Similar SQL Examples]
    Q --> R[Retrieve Entity Based on LSH Similarity]
    R --> S[Prepare Schema String]
    S --> T[Branch Based on Generator Strategy]
    T -->|Titanic| U[Prepare Schema for Titanic]
    U --> V[Run Titanic Generation Cycle]
    V --> W[Check Generation Success]
    W -->|Success| X[Display Query Results]
    W -->|Failure| Y[End: All Models Failed]
    T -->|Basic| Z[Retrieve Context for Basic]
    Z --> AA[Prepare Schema String for Basic]
    AA --> AB[Select Columns]
    AB --> AC[Check for Cancellation]
    AC -->|Cancelled| AD[Yield: Processing Interrupted]
    AC -->|Not Cancelled| AE[Run Basic Generation Cycle]
    AE --> AF[Check Generation Success]
    AF -->|Success| X
    AF -->|Failure| AG[Fallback to Titanic]
    AG --> U
    X --> AH[Check if Explanation Requested]
    AH -->|Yes| AI[Generate Explanation]
    AI --> AJ[Yield: Formatted SQL with Explanation]
    AH -->|No| AK[Yield: Formatted SQL Only]
    Y --> AL[Yield: All AI Agents Failed]
    M -->|All Agents Failed| AM[Yield: Keyword Extraction Failed]
    D --> AN[End]
    G --> AN
    L --> AN
    O --> AN
    AD --> AN
    AJ --> AN
    AK --> AN
    AL --> AN
    AM --> AN
