# PDLC Mermaid Charts

## Chart 1: Standard PDLC Flow (Sequential Reality)

Shows the actual flow including feedback loops and gates that create delays.

```mermaid
flowchart TD
    A[Ideation & Discovery] --> B[Requirements & Spec]
    B --> C[Design]
    C --> D[Implementation / Build]
    D --> E[Code Review]
    E --> F{Approved?}
    F -->|No - Revisions| D
    F -->|Yes| G[QA & Testing]
    G --> H{Bugs Found?}
    H -->|Yes - Bug Fixes| D
    H -->|No| I[Staging / Pre-Prod]
    I --> J{Stakeholder Signoff?}
    J -->|No - Changes| D
    J -->|Yes| K[Production Release]
    K --> L[Post-Release Monitoring]
    L -->|Iteration| A

    style D fill:#2d8a4e,stroke:#1a5c33,color:#fff
    style E fill:#c0392b,stroke:#922b21,color:#fff
    style G fill:#c0392b,stroke:#922b21,color:#fff
    style I fill:#c0392b,stroke:#922b21,color:#fff
    style F fill:#e67e22,stroke:#d35400,color:#fff
    style H fill:#e67e22,stroke:#d35400,color:#fff
    style J fill:#e67e22,stroke:#d35400,color:#fff
```

**Legend:**
- Green = AI-accelerated (fast)
- Red = Human-gated bottlenecks (slow)
- Orange = Decision gates (wait time)

---

## Chart 2: PDLC with Time Proportions (AI-Assisted Build)

Shows where time actually goes when build is AI-accelerated.

```mermaid
gantt
    title PDLC Time Distribution - AI-Assisted Build (12 weeks)
    dateFormat YYYY-MM-DD
    axisFormat %b %d

    section Discovery
    Ideation & Discovery           :a1, 2025-01-06, 14d

    section Spec
    Requirements & Spec            :a2, after a1, 14d

    section Design
    Design                         :a3, after a2, 10d

    section Build
    Implementation (AI-Assisted)   :crit, a4, after a3, 4d

    section Review
    Code Review (Waiting)          :a5, after a4, 7d
    Code Review (Active)           :a5b, after a5, 3d
    Revisions                      :a5c, after a5b, 3d
    Re-Review (Waiting)            :a5d, after a5c, 5d

    section QA
    QA Queue Wait                  :a6, after a5d, 5d
    Active QA Testing              :a6b, after a6, 5d
    Bug Fix + Retest               :a6c, after a6b, 4d

    section Staging
    Deploy to Staging              :a7, after a6c, 1d
    Bake Time / Stakeholder Demo   :a7b, after a7, 9d

    section Release
    Production Deploy              :a8, after a7b, 2d
```

---

## Chart 3: Where Humans Wait vs. Where Humans Work

```mermaid
pie title Time Breakdown: Working vs. Waiting (12-week cycle)
    "Active Human Work" : 25
    "Waiting for Humans (queues, scheduling, availability)" : 45
    "Active AI/Automated Work" : 5
    "Process Overhead (meetings, status updates, handoffs)" : 15
    "Environment/Tooling Issues" : 10
```

---

## Chart 4: The Compression Problem

What happens when build collapses but nothing else changes.

```mermaid
flowchart LR
    subgraph Traditional ["Traditional PDLC (15 weeks)"]
        direction LR
        T1["Discovery<br/>2 wk"] --> T2["Spec<br/>2 wk"]
        T2 --> T3["Design<br/>2 wk"]
        T3 --> T4["Build<br/>3 wk"]
        T4 --> T5["Review<br/>1.5 wk"]
        T5 --> T6["QA<br/>2 wk"]
        T6 --> T7["Stage<br/>1.5 wk"]
        T7 --> T8["Ship<br/>0.5 wk"]
    end

    subgraph Current ["Current Reality (12 weeks)"]
        direction LR
        C1["Discovery<br/>2 wk"] --> C2["Spec<br/>2 wk"]
        C2 --> C3["Design<br/>1.5 wk"]
        C3 --> C4["Build<br/>0.5 wk"]
        C4 --> C5["Review<br/>2.5 wk"]
        C5 --> C6["QA<br/>2 wk"]
        C6 --> C7["Stage<br/>1.5 wk"]
        C7 --> C8["Ship<br/>0.5 wk"]
    end

    subgraph Target ["AI-Native PDLC (? weeks)"]
        direction LR
        F1["Discovery<br/>? wk"] --> F2["Spec<br/>? wk"]
        F2 --> F3["Design<br/>? wk"]
        F3 --> F4["Build<br/>? wk"]
        F4 --> F5["Review<br/>? wk"]
        F5 --> F6["QA<br/>? wk"]
        F6 --> F7["Stage<br/>? wk"]
        F7 --> F8["Ship<br/>? wk"]
    end

    style T4 fill:#e67e22,stroke:#d35400,color:#fff
    style C4 fill:#2d8a4e,stroke:#1a5c33,color:#fff
    style C5 fill:#c0392b,stroke:#922b21,color:#fff
    style F1 fill:#3498db,stroke:#2980b9,color:#fff
    style F2 fill:#3498db,stroke:#2980b9,color:#fff
    style F3 fill:#3498db,stroke:#2980b9,color:#fff
    style F4 fill:#3498db,stroke:#2980b9,color:#fff
    style F5 fill:#3498db,stroke:#2980b9,color:#fff
    style F6 fill:#3498db,stroke:#2980b9,color:#fff
    style F7 fill:#3498db,stroke:#2980b9,color:#fff
    style F8 fill:#3498db,stroke:#2980b9,color:#fff
```

---

## Chart 5: Sequential Gates vs. Parallel Possibilities

```mermaid
flowchart TD
    subgraph today ["Today: Sequential Gates"]
        direction LR
        S1[Build] --> S2[PR Created]
        S2 --> S3[Wait for<br/>Reviewer 1]
        S3 --> S4[Wait for<br/>Reviewer 2]
        S4 --> S5[Wait for<br/>Reviewer 3]
        S5 --> S6[Revisions]
        S6 --> S7[Re-review]
        S7 --> S8[QA Queue]
        S8 --> S9[QA Test]
        S9 --> S10[Stage Queue]
        S10 --> S11[Bake]
        S11 --> S12[Ship]
    end

    subgraph future ["Possible: Parallel + AI Gates"]
        direction LR
        P1[Build] --> P2[PR Created]
        P2 --> P3[AI Pre-Review<br/>Instant]
        P3 --> P4[AI Test Suite<br/>Runs Parallel]
        P3 --> P5[Human Review<br/>1 Reviewer]
        P4 --> P6[Auto-Stage<br/>if Green]
        P5 --> P6
        P6 --> P7[AI Monitoring<br/>Auto-Rollback]
        P7 --> P8[Ship]
    end

    style S3 fill:#c0392b,stroke:#922b21,color:#fff
    style S4 fill:#c0392b,stroke:#922b21,color:#fff
    style S5 fill:#c0392b,stroke:#922b21,color:#fff
    style S8 fill:#c0392b,stroke:#922b21,color:#fff
    style S10 fill:#c0392b,stroke:#922b21,color:#fff
    style P3 fill:#2d8a4e,stroke:#1a5c33,color:#fff
    style P4 fill:#2d8a4e,stroke:#1a5c33,color:#fff
    style P7 fill:#2d8a4e,stroke:#1a5c33,color:#fff
```

---

## How to Render These Charts

These charts use [Mermaid](https://mermaid.js.org/) syntax. They render natively in:
- GitHub markdown (README, issues, PRs)
- VS Code with Mermaid extensions
- Notion (with Mermaid block)
- [Mermaid Live Editor](https://mermaid.live/) for quick previews and PNG/SVG export

For presentations, use the live editor to export as SVG/PNG.
