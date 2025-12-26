# Product Requirements Document (PRD)
# YouTube Smart Learning Intent Detection

**Version:** 1.0  
**Last Updated:** December 26, 2025  
**Author:** Swapan Kumar Soren  
**Status:** Ready for Development

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Solution Overview](#3-solution-overview)
4. [User Personas](#4-user-personas)
5. [User Stories & Acceptance Criteria](#5-user-stories--acceptance-criteria)
6. [Functional Requirements](#6-functional-requirements)
7. [Technical Specifications](#7-technical-specifications)
8. [UI/UX Specifications](#8-uiux-specifications)
9. [API Specifications](#9-api-specifications)
10. [Data Models](#10-data-models)
11. [AI/ML Specifications](#11-aiml-specifications)
12. [Analytics & Metrics](#12-analytics--metrics)
13. [Edge Cases & Error Handling](#13-edge-cases--error-handling)
14. [Security & Privacy](#14-security--privacy)
15. [Accessibility Requirements](#15-accessibility-requirements)
16. [Internationalization](#16-internationalization)
17. [Testing Requirements](#17-testing-requirements)
18. [Launch Plan](#18-launch-plan)
19. [Future Roadmap](#19-future-roadmap)
20. [Appendix](#20-appendix)

---

## 1. Executive Summary

### 1.1 Product Vision

Build an AI-powered intent detection system that automatically identifies when YouTube users are searching for educational/learning content and optimizes search results to prioritize long-form tutorials over Shorts, without requiring any manual user action.

### 1.2 Business Objective

Reduce tutorial search abandonment rate by 30% within 6 months by automatically serving relevant long-form educational content to users with learning intent.

### 1.3 Key Value Proposition

| Stakeholder | Value |
|-------------|-------|
| **Users** | Find tutorials faster without manual filtering; reduced frustration |
| **YouTube** | Reduced platform abandonment; increased session duration for learners |
| **Creators** | Better visibility for educational content creators |
| **Advertisers** | Higher-value impressions on educational content (longer watch time) |

### 1.4 Success Criteria

- **Primary:** 30% reduction in tutorial search abandonment rate
- **Secondary:** Time-to-first-click reduced from ~45s to <30s for educational queries
- **Tertiary:** Tutorial video CTR increased from ~4% to >6%

---

## 2. Problem Statement

### 2.1 Current State

When users search for educational content on YouTube (e.g., "Python tutorial for beginners"), the search results are dominated by Shorts (videos <60 seconds) that appear prominently due to high engagement metrics but fail to deliver substantive educational value.

### 2.2 User Pain Points

| Pain Point | Severity (1-10) | Frequency | Evidence |
|------------|-----------------|-----------|----------|
| Shorts dominate tutorial searches | 9 | Every search | 100% of users reported seeing Shorts first |
| Manual filtering required | 8 | Every search | Users must click "Filters" → "Long" |
| Platform abandonment | 8 | 33% of sessions | Users leave for Google/other platforms |
| Time wasted scrolling | 7 | Every search | ~45 seconds to find relevant content |
| Frustration with results | 8 | Frequent | 8/10 average frustration rating |

### 2.3 Business Impact

- **Lost Users:** 33% of learners abandon YouTube for Google search
- **Lost Watch Time:** Educational content drives 3x longer sessions than Shorts
- **Lost Revenue:** Educational viewers have 2x higher ad engagement
- **Competitive Risk:** Competitors (Coursera, Udemy) capture displaced learners

### 2.4 Root Cause Analysis

```
Root Cause: YouTube's ranking algorithm optimizes for engagement (views, likes, shares)
            rather than user intent satisfaction.

Contributing Factors:
├── Shorts have higher CTR (thumbnails are eye-catching)
├── Shorts have higher completion rate (they're short)
├── Algorithm doesn't distinguish learning intent from entertainment intent
└── No automatic content-type filtering based on query semantics
```

---

## 3. Solution Overview

### 3.1 Proposed Solution

Implement an AI-powered **Learning Intent Detection System** that:

1. **Analyzes** search queries in real-time using NLP
2. **Detects** educational/learning intent with confidence scoring
3. **Optimizes** search results by deprioritizing Shorts when learning intent is detected
4. **Provides** user control via "Show Shorts" escape hatch

### 3.2 Solution Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        USER SEARCH FLOW                              │
└─────────────────────────────────────────────────────────────────────┘

[User Query] → [Intent Detection Engine] → [Result Optimizer] → [Display Results]
     │                   │                        │                    │
     │                   ▼                        ▼                    ▼
     │         ┌─────────────────┐      ┌─────────────────┐   ┌──────────────┐
     │         │ NLP Analysis    │      │ If Learning:    │   │ Show AI      │
     │         │ - Keyword Match │      │ - Boost 20+ min │   │ Indicator    │
     │         │ - Semantic Parse│      │ - Demote Shorts │   │ + Override   │
     │         │ - Context Check │      │ Else:           │   │ Option       │
     │         │ - Confidence    │      │ - Standard Rank │   │              │
     │         └─────────────────┘      └─────────────────┘   └──────────────┘
     │                   │
     │                   ▼
     │         ┌─────────────────┐
     │         │ Output:         │
     │         │ - Intent Type   │
     │         │ - Confidence %  │
     │         │ - Detected Keys │
     │         └─────────────────┘
```

### 3.3 Key Features

| Feature | Priority | Description |
|---------|----------|-------------|
| Intent Detection | P0 | NLP-based learning intent classification |
| Result Optimization | P0 | Rerank results based on detected intent |
| AI Indicator | P1 | Show users when AI optimization is active |
| User Override | P1 | "Show Shorts" link for user control |
| Feedback Loop | P2 | Learn from user behavior to improve detection |

### 3.4 Out of Scope (v1)

- Personalized learning paths
- Course recommendations
- Integration with external learning platforms
- Video chapter navigation
- Learning progress tracking

---

## 4. User Personas

### 4.1 Primary Persona: The Self-Learner

```yaml
Name: Neha Sharma
Age: 28
Location: Bengaluru, India
Occupation: Marketing Analyst

Demographics:
  - College-educated professional
  - Tech-savvy but not a developer
  - Uses YouTube 2-3 hours/day
  - Primary device: Mobile (60%), Desktop (40%)

Goals:
  - Learn Python for data analysis
  - Upskill in Excel advanced features
  - Prepare for career transition to data analytics

Behaviors:
  - Searches for tutorials during lunch break and evenings
  - Prefers 20-60 minute comprehensive tutorials
  - Takes notes while watching
  - Bookmarks videos for later reference

Pain Points:
  - "When I search for tutorials, I get Shorts first"
  - "I have to scroll a lot to find real tutorials"
  - "Sometimes I just leave YouTube and search Google instead"
  - "60-second videos can't teach me anything useful"

Quote:
  "I want to learn, not be entertained. Show me the real tutorials."

Frustration Level: 8/10
Platform Abandonment: Yes (to Google, Coursera)
```

### 4.2 Secondary Persona: The Student

```yaml
Name: Arjun Patel
Age: 21
Location: Mumbai, India
Occupation: Engineering Student

Demographics:
  - Undergraduate student
  - Heavy YouTube user (4+ hours/day)
  - Mixed usage: entertainment + learning
  - Primary device: Mobile (80%)

Goals:
  - Understand complex engineering concepts
  - Prepare for competitive exams
  - Learn programming languages

Behaviors:
  - Searches for specific topics before exams
  - Watches at 1.5x-2x speed
  - Prefers channels with structured playlists
  - Shares useful videos with classmates

Pain Points:
  - "I need in-depth explanations, not 60-second summaries"
  - "The algorithm thinks I want Shorts because I watch them for fun"
  - "Switching between learning mode and entertainment mode is hard"

Quote:
  "YouTube knows I watch Shorts for fun, but can't it tell when I'm studying?"

Frustration Level: 7/10
Platform Abandonment: Sometimes (to Khan Academy)
```

### 4.3 Tertiary Persona: The Professional Upskiller

```yaml
Name: Rajesh Kumar
Age: 35
Location: Delhi, India
Occupation: IT Manager

Demographics:
  - Mid-career professional
  - Limited time for learning
  - Desktop-primary user
  - Prefers evening learning sessions

Goals:
  - Learn cloud computing (AWS, Azure)
  - Stay updated with industry trends
  - Prepare for certifications

Behaviors:
  - Searches for specific technical topics
  - Watches full courses over weeks
  - Values quality over quantity
  - Willing to pay for premium content

Pain Points:
  - "My time is limited; I can't waste it scrolling past irrelevant content"
  - "I need structured learning, not viral clips"

Quote:
  "Give me the best tutorial, not the most popular Short."

Frustration Level: 9/10
Platform Abandonment: Yes (to Udemy, LinkedIn Learning)
```

---

## 5. User Stories & Acceptance Criteria

### 5.1 Epic: Learning Intent Detection

#### User Story 1: Automatic Intent Detection

```gherkin
AS A user searching for educational content
I WANT YouTube to automatically detect my learning intent
SO THAT I don't have to manually filter results

ACCEPTANCE CRITERIA:
- GIVEN I search for "Python tutorial for beginners"
- WHEN the search results load
- THEN the system detects learning intent with >85% confidence
- AND tutorials (videos >10 minutes) appear in top 5 results
- AND Shorts are moved below the fold

GIVEN I search for "funny cat videos"
- WHEN the search results load
- THEN the system does NOT detect learning intent
- AND standard ranking is applied (no intervention)
```

#### User Story 2: AI Transparency Indicator

```gherkin
AS A user with detected learning intent
I WANT to see an indicator that AI has optimized my results
SO THAT I understand why I'm seeing tutorials first

ACCEPTANCE CRITERIA:
- GIVEN learning intent is detected with >85% confidence
- WHEN search results display
- THEN a subtle indicator appears: "🎓 Learning mode • Showing tutorials first"
- AND the indicator is positioned below the search bar
- AND the indicator includes a "Show Shorts" link

VISUAL SPEC:
┌─────────────────────────────────────────────────┐
│ 🎓 Learning mode • Showing tutorials first      │
│                                    [Show Shorts]│
└─────────────────────────────────────────────────┘
Background: #E8F0FE (light blue)
Text: #1967D2 (blue)
Border-radius: 8px
Padding: 8px 16px
```

#### User Story 3: User Override Control

```gherkin
AS A user who wants to see Shorts despite learning intent
I WANT to easily override the AI optimization
SO THAT I maintain control over my search experience

ACCEPTANCE CRITERIA:
- GIVEN learning mode is active
- WHEN I click "Show Shorts"
- THEN Shorts are restored to their original positions
- AND the indicator changes to "Showing all results"
- AND my preference is remembered for this session only (not permanently)

- GIVEN I override to show Shorts
- WHEN I perform a new search
- THEN learning mode detection runs fresh (no carryover)
```

#### User Story 4: Seamless Non-Learning Search

```gherkin
AS A user searching for entertainment content
I WANT the experience to remain unchanged
SO THAT AI doesn't interfere when I'm not learning

ACCEPTANCE CRITERIA:
- GIVEN I search for "trending music videos"
- WHEN intent analysis completes
- THEN confidence for learning intent is <85%
- AND no AI indicator is shown
- AND results appear in standard ranking order
- AND Shorts appear in their normal positions
```

### 5.2 Epic: Result Optimization

#### User Story 5: Tutorial Prioritization

```gherkin
AS A user with detected learning intent
I WANT tutorials to be prioritized in my results
SO THAT I find relevant educational content faster

ACCEPTANCE CRITERIA:
- GIVEN learning intent detected
- WHEN results are displayed
- THEN videos >10 minutes receive +50 ranking boost
- AND videos >30 minutes receive +100 ranking boost
- AND Shorts (<60 seconds) receive -200 ranking penalty
- AND original relevance score is still factored in
```

#### User Story 6: Quality Signals

```gherkin
AS A user viewing optimized results
I WANT to see quality indicators on tutorials
SO THAT I can quickly identify the best content

ACCEPTANCE CRITERIA:
- GIVEN learning mode is active
- WHEN viewing search results
- THEN top-rated tutorials show "⭐ Top Rated" badge
- AND videos with chapters show "📑 Has Chapters" indicator
- AND duration is prominently displayed

VISUAL SPEC:
┌─────────────────────────────────────────────────────┐
│ [THUMBNAIL]  Python Tutorial for Beginners          │
│   45:22      ⭐ Top Rated • 📑 Has Chapters         │
│              Programming with Mosh • 8.5M views     │
└─────────────────────────────────────────────────────┘
```

---

## 6. Functional Requirements

### 6.1 Core Functions

| ID | Requirement | Priority | Dependencies |
|----|-------------|----------|--------------|
| FR-001 | System SHALL analyze search queries for learning intent | P0 | NLP Model |
| FR-002 | System SHALL classify intent with confidence score (0-100%) | P0 | FR-001 |
| FR-003 | System SHALL trigger optimization when confidence >85% | P0 | FR-002 |
| FR-004 | System SHALL deprioritize Shorts for learning queries | P0 | FR-003 |
| FR-005 | System SHALL boost tutorials (>10min) for learning queries | P0 | FR-003 |
| FR-006 | System SHALL display AI indicator when learning mode active | P1 | FR-003 |
| FR-007 | System SHALL provide "Show Shorts" override option | P1 | FR-006 |
| FR-008 | System SHALL process queries in <50ms | P0 | Infrastructure |
| FR-009 | System SHALL log all detections for analysis | P2 | Analytics |
| FR-010 | System SHALL support multiple languages | P2 | i18n |

### 6.2 Business Rules

```yaml
BR-001 Learning Intent Threshold:
  - Confidence >= 85%: Activate learning mode
  - Confidence 70-84%: Log but don't activate (for training data)
  - Confidence < 70%: No action

BR-002 Ranking Adjustments:
  - Videos > 30 minutes: +100 score
  - Videos 10-30 minutes: +50 score
  - Videos 3-10 minutes: No change
  - Videos 1-3 minutes: -50 score
  - Shorts (<60 seconds): -200 score

BR-003 Override Behavior:
  - Override applies to current search only
  - New search resets to default detection
  - Override is not stored in user preferences

BR-004 Fallback Behavior:
  - If NLP service unavailable: Use standard ranking
  - If confidence cannot be calculated: Use standard ranking
  - Graceful degradation required
```

### 6.3 Constraints

| Constraint | Description | Rationale |
|------------|-------------|-----------|
| Latency | Query analysis must complete in <50ms | User experience |
| Accuracy | False positive rate must be <10% | User trust |
| Coverage | Must support top 20 languages | Global user base |
| Availability | 99.9% uptime for detection service | Business critical |

---

## 7. Technical Specifications

### 7.1 System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              SYSTEM ARCHITECTURE                             │
└─────────────────────────────────────────────────────────────────────────────┘

                                    ┌─────────────┐
                                    │   Client    │
                                    │  (Web/App)  │
                                    └──────┬──────┘
                                           │
                                           ▼
                              ┌────────────────────────┐
                              │      API Gateway       │
                              │   (Rate Limiting,      │
                              │    Authentication)     │
                              └───────────┬────────────┘
                                          │
                    ┌─────────────────────┼─────────────────────┐
                    │                     │                     │
                    ▼                     ▼                     ▼
          ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
          │  Search Service │   │ Intent Detection │   │ Ranking Service │
          │                 │◄──│     Service      │──►│                 │
          │ - Query parsing │   │                 │   │ - Score calc    │
          │ - Result fetch  │   │ - NLP analysis  │   │ - Reranking     │
          │                 │   │ - Confidence    │   │ - Filtering     │
          └────────┬────────┘   │ - Caching       │   └────────┬────────┘
                   │            └────────┬────────┘            │
                   │                     │                     │
                   ▼                     ▼                     ▼
          ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
          │  Video Index    │   │   ML Model      │   │  Feature Store  │
          │  (Elasticsearch)│   │   (TensorFlow)  │   │    (Redis)      │
          └─────────────────┘   └─────────────────┘   └─────────────────┘
                                        │
                                        ▼
                              ┌─────────────────┐
                              │ Analytics Store │
                              │  (BigQuery)     │
                              └─────────────────┘
```

### 7.2 Technology Stack

| Component | Technology | Justification |
|-----------|------------|---------------|
| Frontend | React 18 + TypeScript | Industry standard, type safety |
| Mobile | React Native | Cross-platform consistency |
| API Layer | Node.js + Express | Fast I/O, JavaScript ecosystem |
| ML Service | Python + FastAPI | ML library support, async |
| ML Model | TensorFlow / PyTorch | Production-grade ML |
| Cache | Redis | Low-latency caching |
| Database | PostgreSQL | Relational data, ACID |
| Search | Elasticsearch | Full-text search |
| Analytics | BigQuery | Large-scale analytics |
| CDN | CloudFlare | Global edge caching |
| Monitoring | Datadog | APM + logging |

### 7.3 Component Specifications

#### 7.3.1 Intent Detection Service

```yaml
Service Name: intent-detection-service
Language: Python 3.11
Framework: FastAPI
Port: 8080

Endpoints:
  POST /v1/analyze:
    Input:
      - query: string (required)
      - language: string (optional, default: "en")
      - user_context: object (optional)
    Output:
      - intent_type: "learning" | "entertainment" | "unknown"
      - confidence: float (0.0 - 1.0)
      - detected_signals: string[]
      - processing_time_ms: int
    Latency Target: p99 < 50ms
    
  GET /v1/health:
    Output:
      - status: "healthy" | "degraded" | "unhealthy"
      - model_version: string
      - uptime_seconds: int

Dependencies:
  - tensorflow-serving: ML model inference
  - redis: Query caching
  - prometheus: Metrics export

Scaling:
  - Horizontal: Auto-scale 2-20 pods based on CPU
  - Caching: Cache query results for 1 hour
```

#### 7.3.2 Ranking Service

```yaml
Service Name: ranking-service
Language: Go 1.21
Port: 8081

Endpoints:
  POST /v1/rerank:
    Input:
      - results: VideoResult[] (required)
      - intent: IntentAnalysis (required)
      - limit: int (optional, default: 20)
    Output:
      - results: RankedVideoResult[]
      - optimization_applied: boolean
      - ranking_factors: object

Ranking Algorithm:
  final_score = base_relevance_score 
                + intent_adjustment 
                + duration_boost 
                + quality_signals

  intent_adjustment:
    if intent.type == "learning" AND intent.confidence >= 0.85:
      if video.duration >= 30min: +100
      elif video.duration >= 10min: +50
      elif video.duration < 1min: -200
    else:
      0

Latency Target: p99 < 30ms
```

### 7.4 Infrastructure Requirements

```yaml
Production Environment:
  Compute:
    - Intent Service: 4 pods, 2 vCPU, 4GB RAM each
    - Ranking Service: 4 pods, 2 vCPU, 2GB RAM each
    - ML Model Server: 2 pods, 4 vCPU, 8GB RAM, GPU optional
  
  Storage:
    - Redis Cluster: 3 nodes, 16GB each
    - PostgreSQL: 2 nodes (primary + replica), 100GB SSD
  
  Network:
    - Internal: Service mesh (Istio)
    - External: Load balancer with SSL termination
    - CDN: Global edge caching for static assets

  Monitoring:
    - APM: Datadog
    - Logging: ELK Stack
    - Alerting: PagerDuty integration
```

---

## 8. UI/UX Specifications

### 8.1 Design System

```css
/* Color Palette */
:root {
  /* Primary - YouTube Red */
  --color-primary: #FF0000;
  --color-primary-dark: #CC0000;
  
  /* AI Blue - Learning Mode */
  --color-ai: #1A73E8;
  --color-ai-light: #E8F0FE;
  --color-ai-dark: #1557B0;
  
  /* Neutral */
  --color-background: #0F0F0F;
  --color-surface: #1A1A1A;
  --color-surface-elevated: #282828;
  --color-border: #303030;
  
  /* Text */
  --color-text-primary: #FFFFFF;
  --color-text-secondary: #AAAAAA;
  --color-text-disabled: #717171;
  
  /* Semantic */
  --color-success: #1E8E3E;
  --color-warning: #F9AB00;
  --color-error: #D93025;
  
  /* Typography */
  --font-family: 'Roboto', Arial, sans-serif;
  --font-size-xs: 10px;
  --font-size-sm: 12px;
  --font-size-md: 14px;
  --font-size-lg: 16px;
  --font-size-xl: 18px;
  --font-size-2xl: 24px;
  
  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 12px;
  --spacing-lg: 16px;
  --spacing-xl: 24px;
  --spacing-2xl: 32px;
  
  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;
  
  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.3);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.3);
  --shadow-lg: 0 10px 15px rgba(0,0,0,0.3);
}
```

### 8.2 Component Specifications

#### 8.2.1 Learning Mode Indicator

```
┌─────────────────────────────────────────────────────────────────┐
│                     LEARNING MODE INDICATOR                      │
└─────────────────────────────────────────────────────────────────┘

DESKTOP:
┌──────────────────────────────────────────────────────────────────────┐
│ 🎓 Learning mode • Showing tutorials first              [Show Shorts]│
└──────────────────────────────────────────────────────────────────────┘
Width: 100% of search results container
Height: 40px
Background: var(--color-ai-light)
Border: 1px solid var(--color-ai)
Border-radius: var(--radius-md)
Margin: var(--spacing-md) 0
Padding: var(--spacing-sm) var(--spacing-lg)

Icon: 🎓 (graduation cap emoji)
Text: "Learning mode • Showing tutorials first"
Font: var(--font-size-md), color: var(--color-ai-dark)
Link: "Show Shorts" - right-aligned, underline on hover


MOBILE:
┌─────────────────────────────────────────┐
│ 🎓 Learning mode                        │
│    Showing tutorials first [Show Shorts]│
└─────────────────────────────────────────┘
Width: 100% - 32px (16px margin each side)
Height: auto (min 48px)
Two-line layout for smaller screens
Touch target: 48x48px minimum for "Show Shorts"


STATES:
- Default: Blue background, as shown above
- Hover (Show Shorts): Underline text, cursor pointer
- After Override: 
  ┌──────────────────────────────────────────────────────┐
  │ Showing all results                    [Learning mode]│
  └──────────────────────────────────────────────────────┘
  Background: var(--color-surface-elevated)
  Border: 1px solid var(--color-border)
```

#### 8.2.2 Video Result Card (Learning Mode)

```
┌─────────────────────────────────────────────────────────────────┐
│                    VIDEO RESULT CARD - TUTORIAL                  │
└─────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│ ┌──────────────────┐                                                   │
│ │                  │  Python Tutorial for Beginners - Full Course     │
│ │    THUMBNAIL     │  ⭐ Top Rated • 📑 Has Chapters                   │
│ │                  │                                                   │
│ │      45:22       │  Programming with Mosh                           │
│ └──────────────────┘  8.5M views • 2 years ago                        │
└────────────────────────────────────────────────────────────────────────┘

THUMBNAIL:
- Width: 168px (desktop), 160px (mobile)
- Aspect ratio: 16:9
- Border-radius: var(--radius-md)
- Duration badge: Bottom-right, background rgba(0,0,0,0.8)

TITLE:
- Font: var(--font-size-lg), font-weight: 500
- Color: var(--color-text-primary)
- Max lines: 2, overflow: ellipsis

BADGES (Learning Mode Only):
- "⭐ Top Rated" - shown for videos with >90% positive ratings
- "📑 Has Chapters" - shown if video has chapter markers
- Background: transparent
- Font: var(--font-size-sm)
- Color: var(--color-success)

CHANNEL:
- Font: var(--font-size-sm)
- Color: var(--color-text-secondary)

METADATA:
- Font: var(--font-size-sm)
- Color: var(--color-text-secondary)
- Format: "{views} • {time_ago}"


HIGHLIGHTED STATE (First Tutorial):
┌────────────────────────────────────────────────────────────────────────┐
│ ┌──────────────────┐                                                   │
│ │                  │  Python Tutorial for Beginners - Full Course     │
│ │    THUMBNAIL     │  ⭐ Top Rated • 📑 Has Chapters                   │
│ │   [GREEN BG]     │                                                   │
│ │      45:22       │  Programming with Mosh                           │
│ └──────────────────┘  8.5M views • 2 years ago                        │
└────────────────────────────────────────────────────────────────────────┘
Border: 2px solid var(--color-success)
Background: rgba(30, 142, 62, 0.1)
```

#### 8.2.3 Short Video Card (Demoted State)

```
┌─────────────────────────────────────────────────────────────────┐
│                    VIDEO RESULT CARD - SHORT (DEMOTED)           │
└─────────────────────────────────────────────────────────────────┘

When learning mode is active, Shorts appear lower and with reduced prominence:

┌────────────────────────────────────────────────────────────────────────┐
│ ┌──────────────────┐                                                   │
│ │                  │  Python in 60 Seconds! 🔥                        │
│ │    THUMBNAIL     │                                                   │
│ │   [RED BADGE]    │  CodeQuick                                       │
│ │     SHORTS       │  1.2M views • 3 days ago                         │
│ └──────────────────┘                                                   │
└────────────────────────────────────────────────────────────────────────┘

SHORTS BADGE:
- Position: Bottom-left of thumbnail
- Background: var(--color-primary)
- Text: "SHORTS"
- Font: var(--font-size-xs), font-weight: 700
- Padding: 2px 6px
- Border-radius: var(--radius-sm)

VISUAL DISTINCTION:
- Opacity: 0.8 (slightly faded compared to tutorials)
- No quality badges shown
```

### 8.3 Interaction Flows

#### 8.3.1 Search Flow with Intent Detection

```
┌─────────────────────────────────────────────────────────────────┐
│                     SEARCH INTERACTION FLOW                      │
└─────────────────────────────────────────────────────────────────┘

1. USER TYPES QUERY
   ┌─────────────────────────────────────────┐
   │ 🔍 Python tutorial for beginners        │
   └─────────────────────────────────────────┘
   
2. USER PRESSES ENTER / CLICKS SEARCH
   - Query sent to Search API
   - Intent Detection runs in parallel (~30ms)
   
3. LOADING STATE (if > 100ms)
   ┌─────────────────────────────────────────┐
   │ ⟳ Loading results...                    │
   └─────────────────────────────────────────┘

4. RESULTS DISPLAY (Learning Intent Detected)
   ┌─────────────────────────────────────────────────────────────┐
   │ 🎓 Learning mode • Showing tutorials first    [Show Shorts] │
   ├─────────────────────────────────────────────────────────────┤
   │ [Tutorial 1 - Highlighted]                                  │
   │ [Tutorial 2]                                                │
   │ [Tutorial 3]                                                │
   │ [Tutorial 4]                                                │
   │ ... more tutorials ...                                      │
   │ [Short 1 - Demoted]                                         │
   │ [Short 2 - Demoted]                                         │
   └─────────────────────────────────────────────────────────────┘

5. USER CLICKS "Show Shorts"
   - Animation: Indicator slides up, changes color
   - Results: Shorts animate back to original positions
   - Indicator: Changes to "Showing all results [Learning mode]"

6. USER PERFORMS NEW SEARCH
   - Reset to step 1
   - Previous override does not carry over
```

### 8.4 Responsive Breakpoints

```css
/* Mobile First Approach */

/* Small Mobile */
@media (max-width: 359px) {
  .learning-indicator { 
    flex-direction: column;
    align-items: flex-start;
  }
  .video-card { 
    flex-direction: column;
  }
  .thumbnail { 
    width: 100%;
  }
}

/* Mobile */
@media (min-width: 360px) and (max-width: 599px) {
  .learning-indicator { 
    padding: 8px 12px;
  }
  .thumbnail { 
    width: 160px;
  }
}

/* Tablet */
@media (min-width: 600px) and (max-width: 1023px) {
  .results-grid { 
    grid-template-columns: repeat(2, 1fr);
  }
  .thumbnail { 
    width: 168px;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .results-grid { 
    grid-template-columns: repeat(3, 1fr);
  }
  .thumbnail { 
    width: 200px;
  }
}

/* Large Desktop */
@media (min-width: 1440px) {
  .results-grid { 
    grid-template-columns: repeat(4, 1fr);
  }
}
```

---

## 9. API Specifications

### 9.1 Intent Detection API

```yaml
openapi: 3.0.0
info:
  title: Learning Intent Detection API
  version: 1.0.0
  description: API for detecting learning intent in search queries

servers:
  - url: https://api.youtube.internal/intent/v1
    description: Production

paths:
  /analyze:
    post:
      summary: Analyze query for learning intent
      operationId: analyzeIntent
      tags:
        - Intent Detection
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/IntentRequest'
      responses:
        '200':
          description: Successful analysis
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/IntentResponse'
        '400':
          description: Invalid request
        '429':
          description: Rate limit exceeded
        '500':
          description: Internal server error

components:
  schemas:
    IntentRequest:
      type: object
      required:
        - query
      properties:
        query:
          type: string
          minLength: 1
          maxLength: 500
          description: The search query to analyze
          example: "Python tutorial for beginners"
        language:
          type: string
          default: "en"
          description: ISO 639-1 language code
          example: "en"
        user_context:
          type: object
          properties:
            recent_searches:
              type: array
              items:
                type: string
              maxItems: 10
            watch_history_categories:
              type: array
              items:
                type: string
              maxItems: 5

    IntentResponse:
      type: object
      properties:
        intent_type:
          type: string
          enum: [learning, entertainment, navigation, unknown]
          description: Detected intent category
          example: "learning"
        confidence:
          type: number
          format: float
          minimum: 0.0
          maximum: 1.0
          description: Confidence score for the detected intent
          example: 0.94
        detected_signals:
          type: array
          items:
            type: string
          description: Keywords/patterns that triggered detection
          example: ["tutorial", "beginners"]
        should_optimize:
          type: boolean
          description: Whether to apply result optimization
          example: true
        processing_time_ms:
          type: integer
          description: Time taken to process the request
          example: 23
        model_version:
          type: string
          description: Version of the ML model used
          example: "v2.3.1"
```

### 9.2 Search Results API Extension

```yaml
# Extension to existing Search API
paths:
  /search:
    get:
      summary: Search videos with optional intent optimization
      parameters:
        - name: q
          in: query
          required: true
          schema:
            type: string
        - name: intent_optimization
          in: query
          required: false
          schema:
            type: boolean
            default: true
          description: Whether to apply intent-based optimization
        - name: override_learning_mode
          in: query
          required: false
          schema:
            type: boolean
            default: false
          description: Override to show Shorts despite learning intent
      responses:
        '200':
          content:
            application/json:
              schema:
                type: object
                properties:
                  results:
                    type: array
                    items:
                      $ref: '#/components/schemas/VideoResult'
                  intent_analysis:
                    $ref: '#/components/schemas/IntentResponse'
                  optimization_applied:
                    type: boolean
                  total_results:
                    type: integer

    VideoResult:
      type: object
      properties:
        video_id:
          type: string
        title:
          type: string
        description:
          type: string
        channel_name:
          type: string
        channel_id:
          type: string
        duration_seconds:
          type: integer
        view_count:
          type: integer
        published_at:
          type: string
          format: date-time
        thumbnail_url:
          type: string
        is_short:
          type: boolean
        has_chapters:
          type: boolean
        quality_badges:
          type: array
          items:
            type: string
            enum: [top_rated, has_chapters, verified_educator]
        ranking_score:
          type: number
          description: Final ranking score after optimization
        original_position:
          type: integer
          description: Position before optimization
```

### 9.3 Analytics Events API

```yaml
paths:
  /events:
    post:
      summary: Log analytics events
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AnalyticsEvent'

components:
  schemas:
    AnalyticsEvent:
      type: object
      required:
        - event_type
        - timestamp
      properties:
        event_type:
          type: string
          enum:
            - learning_intent_detected
            - learning_mode_activated
            - user_override_shorts
            - user_override_learning
            - tutorial_clicked
            - short_clicked
            - search_abandoned
        timestamp:
          type: string
          format: date-time
        session_id:
          type: string
        user_id:
          type: string
        query:
          type: string
        intent_confidence:
          type: number
        video_id:
          type: string
        video_position:
          type: integer
        time_to_click_ms:
          type: integer
```

---

## 10. Data Models

### 10.1 Database Schema

```sql
-- Intent Detection Logs
CREATE TABLE intent_detection_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    query TEXT NOT NULL,
    query_hash VARCHAR(64) NOT NULL,
    language VARCHAR(10) DEFAULT 'en',
    intent_type VARCHAR(20) NOT NULL,
    confidence DECIMAL(5,4) NOT NULL,
    detected_signals TEXT[],
    processing_time_ms INTEGER,
    model_version VARCHAR(20),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_query_hash (query_hash),
    INDEX idx_created_at (created_at),
    INDEX idx_intent_type (intent_type)
);

-- User Interactions
CREATE TABLE user_interactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id UUID NOT NULL,
    user_id VARCHAR(64),
    event_type VARCHAR(50) NOT NULL,
    query TEXT,
    intent_confidence DECIMAL(5,4),
    video_id VARCHAR(20),
    video_position INTEGER,
    is_short BOOLEAN,
    time_to_click_ms INTEGER,
    override_type VARCHAR(20),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_session_id (session_id),
    INDEX idx_user_id (user_id),
    INDEX idx_event_type (event_type),
    INDEX idx_created_at (created_at)
);

-- Model Performance Metrics
CREATE TABLE model_metrics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    model_version VARCHAR(20) NOT NULL,
    date DATE NOT NULL,
    total_predictions INTEGER,
    learning_predictions INTEGER,
    override_rate DECIMAL(5,4),
    avg_confidence DECIMAL(5,4),
    avg_processing_time_ms DECIMAL(10,2),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    UNIQUE (model_version, date)
);

-- A/B Test Results
CREATE TABLE ab_test_results (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    experiment_id VARCHAR(50) NOT NULL,
    variant VARCHAR(20) NOT NULL,
    date DATE NOT NULL,
    sessions INTEGER,
    searches INTEGER,
    abandonment_rate DECIMAL(5,4),
    avg_time_to_click_ms DECIMAL(10,2),
    tutorial_ctr DECIMAL(5,4),
    short_ctr DECIMAL(5,4),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    UNIQUE (experiment_id, variant, date)
);
```

### 10.2 Cache Schema (Redis)

```yaml
# Query Intent Cache
Key Pattern: intent:{query_hash}
Value: JSON string
TTL: 3600 seconds (1 hour)
Example:
  Key: intent:a1b2c3d4e5f6
  Value: {
    "intent_type": "learning",
    "confidence": 0.94,
    "detected_signals": ["tutorial", "beginners"],
    "model_version": "v2.3.1"
  }

# Session Override Cache
Key Pattern: override:{session_id}
Value: "true" | "false"
TTL: 1800 seconds (30 minutes)
Example:
  Key: override:sess_12345
  Value: "true"

# Feature Flags
Key Pattern: feature:{flag_name}
Value: JSON string with targeting rules
TTL: 300 seconds (5 minutes)
Example:
  Key: feature:learning_intent_v2
  Value: {
    "enabled": true,
    "rollout_percentage": 50,
    "excluded_regions": ["CN"]
  }
```

### 10.3 Analytics Data Model (BigQuery)

```sql
-- Daily Aggregated Metrics
CREATE TABLE analytics.daily_metrics (
    date DATE NOT NULL,
    region STRING,
    language STRING,
    
    -- Search Metrics
    total_searches INT64,
    learning_searches INT64,
    entertainment_searches INT64,
    
    -- Intent Detection Metrics
    learning_detections INT64,
    avg_confidence FLOAT64,
    false_positive_rate FLOAT64,
    
    -- User Behavior Metrics
    learning_mode_overrides INT64,
    override_rate FLOAT64,
    
    -- Engagement Metrics
    tutorial_clicks INT64,
    tutorial_ctr FLOAT64,
    short_clicks INT64,
    short_ctr FLOAT64,
    avg_time_to_click_ms FLOAT64,
    
    -- Abandonment Metrics
    search_abandonments INT64,
    abandonment_rate FLOAT64,
    
    -- Session Metrics
    avg_session_duration_seconds FLOAT64,
    avg_videos_watched FLOAT64
)
PARTITION BY date
CLUSTER BY region, language;

-- Funnel Analysis View
CREATE VIEW analytics.search_funnel AS
SELECT
    date,
    region,
    -- Funnel stages
    total_searches,
    learning_searches,
    learning_searches / total_searches AS learning_search_rate,
    tutorial_clicks,
    tutorial_clicks / learning_searches AS tutorial_conversion_rate,
    search_abandonments,
    abandonment_rate
FROM analytics.daily_metrics;
```

---

## 11. AI/ML Specifications

### 11.1 Model Architecture

```yaml
Model Name: LearningIntentClassifier
Version: 2.3.1
Framework: TensorFlow 2.x

Architecture:
  Type: Transformer-based Text Classification
  Base Model: DistilBERT (multilingual)
  Fine-tuned: Yes, on YouTube search queries
  
  Layers:
    - Input: Tokenized query (max 64 tokens)
    - Embedding: DistilBERT embeddings (768 dimensions)
    - Pooling: Mean pooling over sequence
    - Dense: 256 units, ReLU activation
    - Dropout: 0.3
    - Dense: 64 units, ReLU activation
    - Output: 4 units (softmax) - learning, entertainment, navigation, unknown

Input Processing:
  - Tokenizer: DistilBERT tokenizer
  - Max length: 64 tokens
  - Padding: Right-side
  - Truncation: Yes

Output:
  - Class probabilities for each intent type
  - Confidence = probability of predicted class
```

### 11.2 Training Data

```yaml
Training Dataset:
  Source: Historical YouTube search queries with labeled intent
  Size: 10 million queries
  Split: 80% train, 10% validation, 10% test
  
  Class Distribution:
    - Learning: 15% (~1.5M)
    - Entertainment: 70% (~7M)
    - Navigation: 10% (~1M)
    - Unknown: 5% (~500K)
  
  Labeling:
    - Primary: User behavior signals (clicked on tutorial vs short)
    - Secondary: Manual annotation by trained annotators
    - Quality: Inter-annotator agreement > 90%

Data Augmentation:
  - Synonym replacement
  - Random word deletion
  - Back-translation (for multilingual support)

Learning Intent Patterns (Training Examples):
  High Confidence:
    - "{topic} tutorial"
    - "how to {verb}"
    - "learn {topic}"
    - "{topic} for beginners"
    - "{topic} course"
    - "{topic} explained"
    - "step by step {topic}"
    - "{topic} guide"
    - "introduction to {topic}"
    - "{topic} basics"
  
  Medium Confidence:
    - "{topic} tips"
    - "{topic} tricks"
    - "best way to {verb}"
    - "{topic} masterclass"
  
  Low/No Confidence (Entertainment):
    - "{topic} funny"
    - "{artist} music video"
    - "{show} clips"
    - "{topic} fails"
    - "{topic} compilation"
```

### 11.3 Model Performance

```yaml
Metrics (Test Set):
  Overall Accuracy: 94.2%
  
  Learning Intent:
    Precision: 91.5%
    Recall: 89.3%
    F1 Score: 90.4%
  
  Entertainment Intent:
    Precision: 96.1%
    Recall: 97.2%
    F1 Score: 96.6%

Latency:
  p50: 15ms
  p95: 35ms
  p99: 48ms

Throughput:
  Batch size 1: 200 QPS per instance
  Batch size 32: 2000 QPS per instance

Resource Usage:
  Memory: 1.2GB
  CPU: 0.5 cores (inference)
  GPU: Optional (2x faster with GPU)
```

### 11.4 Signal Detection Rules

```python
# Keyword-based signals (used alongside ML model)

LEARNING_SIGNALS = {
    "high_confidence": [
        "tutorial", "course", "learn", "how to", "guide",
        "for beginners", "explained", "step by step",
        "introduction to", "basics", "fundamentals",
        "complete guide", "full course", "masterclass"
    ],
    "medium_confidence": [
        "tips", "tricks", "best practices", "deep dive",
        "walkthrough", "demo", "example", "lesson"
    ],
    "negative_signals": [
        "funny", "fail", "compilation", "meme", "reaction",
        "vlog", "unboxing", "review", "trailer", "music video",
        "shorts", "tiktok", "reels"
    ]
}

def detect_signals(query: str) -> dict:
    """
    Extract learning signals from query.
    Returns dict with detected signals and their confidence levels.
    """
    query_lower = query.lower()
    detected = {
        "high": [],
        "medium": [],
        "negative": []
    }
    
    for signal in LEARNING_SIGNALS["high_confidence"]:
        if signal in query_lower:
            detected["high"].append(signal)
    
    for signal in LEARNING_SIGNALS["medium_confidence"]:
        if signal in query_lower:
            detected["medium"].append(signal)
    
    for signal in LEARNING_SIGNALS["negative_signals"]:
        if signal in query_lower:
            detected["negative"].append(signal)
    
    return detected
```

### 11.5 Model Monitoring & Retraining

```yaml
Monitoring:
  Metrics Tracked:
    - Prediction distribution (should match training distribution)
    - Confidence score distribution
    - Override rate (proxy for false positives)
    - Feature drift (embedding space drift)
  
  Alerting Thresholds:
    - Override rate > 15%: Warning
    - Override rate > 25%: Critical
    - Avg confidence < 0.7: Warning
    - Prediction latency p99 > 100ms: Critical

Retraining Schedule:
  - Frequency: Monthly
  - Trigger: Manual or when override rate > 20% for 7 days
  
  Retraining Pipeline:
    1. Export last 30 days of queries with user feedback
    2. Label new data using user behavior signals
    3. Combine with existing training data
    4. Train new model version
    5. Evaluate on held-out test set
    6. A/B test new model (5% traffic)
    7. Full rollout if metrics improve

Model Versioning:
  - Stored in: Google Cloud Storage
  - Format: SavedModel (TensorFlow)
  - Naming: learning_intent_v{major}.{minor}.{patch}
  - Rollback: Keep last 5 versions for instant rollback
```

---

## 12. Analytics & Metrics

### 12.1 Key Performance Indicators (KPIs)

```yaml
North Star Metric:
  Name: Tutorial Search Abandonment Rate
  Definition: % of educational searches where user exits within 30 seconds without clicking
  Current: ~33%
  Target: <23% (30% reduction)
  Measurement: Daily, segmented by region/language

Primary Metrics:
  1. AI Detection Trigger Rate:
     Definition: % of searches where learning intent detected with >85% confidence
     Target: 15-20% of all searches
     Why: Ensures AI is selective, not over-triggering
  
  2. Time-to-First-Click (Learning Searches):
     Definition: Time from search to first video click for educational queries
     Current: ~45 seconds
     Target: <30 seconds
     Why: Measures efficiency improvement
  
  3. Tutorial Video CTR:
     Definition: Click-through rate on videos >10 minutes in learning mode
     Current: ~4%
     Target: >6%
     Why: Measures relevance of promoted tutorials

Secondary Metrics:
  4. User Override Rate:
     Definition: % of learning mode sessions where user clicks "Show Shorts"
     Target: <10%
     Why: Proxy for false positive rate
  
  5. Session Duration (Learning Users):
     Definition: Avg session length for users with learning intent
     Target: +20% vs control
     Why: Measures engagement improvement
  
  6. Return Rate (Learning Users):
     Definition: % of learning users who return within 7 days
     Target: +10% vs control
     Why: Measures long-term satisfaction

Guardrail Metrics (Must Not Regress):
  7. Overall Shorts Watch Time:
     Definition: Total watch time on Shorts platform-wide
     Threshold: Cannot decrease by >2%
     Why: Shorts is a key product; cannot harm it
  
  8. Overall Search Engagement:
     Definition: % of searches resulting in video click
     Threshold: Cannot decrease by >1%
     Why: Ensures optimization doesn't hurt discovery
  
  9. Creator Sentiment:
     Definition: NPS score from educational content creators
     Threshold: Cannot decrease by >5 points
     Why: Creator ecosystem health
```

### 12.2 Dashboard Specifications

```yaml
Executive Dashboard:
  Refresh: Real-time
  Audience: Leadership, PM
  
  Widgets:
    - North Star Metric (abandonment rate) with trend
    - A/B test results summary
    - Regional performance heatmap
    - Daily active users in learning mode
    - Override rate trend (early warning)

Operational Dashboard:
  Refresh: Every 5 minutes
  Audience: Engineering, On-call
  
  Widgets:
    - Intent service latency (p50, p95, p99)
    - Error rate by endpoint
    - Model confidence distribution
    - Cache hit rate
    - Infrastructure health status

ML Model Dashboard:
  Refresh: Daily
  Audience: ML Engineers
  
  Widgets:
    - Prediction distribution over time
    - Confidence score histogram
    - Feature importance (top signals)
    - Model version comparison
    - Retraining triggers status
```

### 12.3 Experiment Design

```yaml
A/B Test Framework:
  Name: Learning Intent Detection v1
  Hypothesis: AI-based result optimization will reduce abandonment rate by 30%
  
  Variants:
    Control (50%):
      - Standard search ranking
      - No intent detection
      - No AI indicator
    
    Treatment (50%):
      - Intent detection enabled
      - Result optimization for learning queries
      - AI indicator shown
  
  Randomization: User-level (consistent experience per user)
  Duration: 4 weeks minimum
  
  Success Criteria:
    Primary:
      - Abandonment rate: Treatment < Control by 25%+ (statistically significant)
    Secondary:
      - Time-to-click: Treatment < Control by 20%+
      - Tutorial CTR: Treatment > Control by 30%+
    Guardrails:
      - Shorts watch time: Treatment >= Control - 2%
      - Overall engagement: Treatment >= Control - 1%
  
  Sample Size Calculation:
    - Baseline abandonment: 33%
    - Expected effect: -30% (relative)
    - Statistical power: 80%
    - Significance level: 5%
    - Required sample: ~50,000 users per variant
```

---

## 13. Edge Cases & Error Handling

### 13.1 Edge Cases

```yaml
EC-001 Ambiguous Queries:
  Example: "Python" (could be tutorial or Monty Python)
  Handling:
    - Confidence will be medium (60-80%)
    - Do not activate learning mode
    - Log for model training
  
EC-002 Mixed Intent Queries:
  Example: "funny Python tutorial memes"
  Handling:
    - Negative signals ("funny", "memes") override positive
    - Confidence reduced by 30%
    - Standard ranking applied
  
EC-003 Trending Topic Collision:
  Example: "Taylor Swift documentary" during album release
  Handling:
    - Check if query matches trending entertainment
    - Reduce confidence if match
    - Prefer entertainment intent for trending topics
  
EC-004 Non-English Queries:
  Example: "Python ट्यूटोरियल" (Hindi)
  Handling:
    - Use multilingual model
    - Fall back to keyword matching if confidence low
    - Log for language-specific model training
  
EC-005 Very Short Queries:
  Example: "tutorial"
  Handling:
    - Mark as ambiguous (no specific topic)
    - Do not activate learning mode
    - User needs to be more specific
  
EC-006 Creator Name Queries:
  Example: "Traversy Media"
  Handling:
    - Detect as navigation intent, not learning
    - Show creator's content without optimization
    - Preserve creator channel experience
  
EC-007 Returning User with Override:
  Example: User who previously clicked "Show Shorts"
  Handling:
    - Override expires with session
    - New session starts fresh
    - No permanent preference storage
  
EC-008 API Timeout:
  Example: Intent service takes >100ms
  Handling:
    - Use cached result if available
    - Fall back to standard ranking if no cache
    - Log timeout for debugging
```

### 13.2 Error Handling

```yaml
Error Scenarios:

E-001 Intent Service Unavailable:
  HTTP Status: 503
  User Impact: None (graceful degradation)
  Handling:
    - Fall back to standard search ranking
    - Do not show AI indicator
    - Alert on-call if >1% of requests affected
    - Auto-retry with exponential backoff

E-002 Model Inference Failure:
  Cause: TensorFlow serving error
  User Impact: None (graceful degradation)
  Handling:
    - Return default intent (unknown, confidence=0)
    - Log error with query details
    - Circuit breaker after 10 failures in 1 minute

E-003 Invalid Query Input:
  Cause: Empty query, special characters only
  User Impact: Validation error shown
  Handling:
    - Return 400 Bad Request
    - Message: "Please enter a valid search query"
    - Do not log to training data

E-004 Rate Limiting:
  Cause: User exceeds 100 searches/minute
  User Impact: Temporary slowdown
  Handling:
    - Return 429 Too Many Requests
    - Include Retry-After header
    - Log potential abuse

E-005 Cache Miss + Service Slow:
  Cause: Cold cache, high latency
  User Impact: Slightly slower results
  Handling:
    - Proceed with search, intent analysis in background
    - Update indicator after analysis completes
    - Results animate to new positions

Error Response Format:
  {
    "error": {
      "code": "INTENT_SERVICE_UNAVAILABLE",
      "message": "Intent analysis temporarily unavailable",
      "fallback_applied": true,
      "retry_after_seconds": 30
    }
  }
```

### 13.3 Fallback Strategies

```yaml
Fallback Priority Chain:

1. Primary: ML Model Inference
   - Full intent analysis
   - Confidence scoring
   - Signal extraction

2. Secondary: Cached Result
   - Query hash lookup
   - TTL: 1 hour
   - ~40% cache hit rate expected

3. Tertiary: Keyword Rules
   - Regex-based signal detection
   - Simple confidence calculation
   - No ML model required

4. Final: Standard Ranking
   - No optimization applied
   - No AI indicator shown
   - Zero user impact

Fallback Activation Logic:
  if ml_model.is_available():
      return ml_model.predict(query)
  elif cache.has(query_hash):
      return cache.get(query_hash)
  elif keyword_rules.enabled:
      return keyword_rules.analyze(query)
  else:
      return default_intent()
```

---

## 14. Security & Privacy

### 14.1 Data Privacy

```yaml
Data Collection:
  What We Collect:
    - Search queries (anonymized after 30 days)
    - Click behavior (aggregated)
    - Override actions (session-level only)
  
  What We Don't Collect:
    - Personal identifiable information (PII)
    - Exact video watch history
    - User demographics
  
  Data Retention:
    - Raw queries: 30 days (for debugging)
    - Anonymized queries: 2 years (for model training)
    - Aggregated metrics: Indefinitely

User Controls:
  - Users can disable "Personalized search" in settings
  - Disabling removes intent optimization entirely
  - No individual opt-out for learning mode specifically
  
GDPR Compliance:
  - Data processing: Legitimate interest (service improvement)
  - Right to access: Query logs available in Google Takeout
  - Right to deletion: Honored within 30 days
  - Data portability: Export available

CCPA Compliance:
  - No sale of personal information
  - Opt-out mechanism available
  - Annual privacy audit required
```

### 14.2 Security Requirements

```yaml
Authentication:
  - Internal APIs: mTLS between services
  - External APIs: OAuth 2.0 with YouTube scopes
  - Service accounts: Least privilege principle

Authorization:
  - Intent service: Read-only access to query data
  - Analytics: Aggregated data only, no PII access
  - Model training: Anonymized data only

Data Encryption:
  - In transit: TLS 1.3
  - At rest: AES-256
  - Keys: Google Cloud KMS, rotated quarterly

Input Validation:
  - Query length: Max 500 characters
  - Character set: Unicode allowed, sanitized for injection
  - Rate limiting: 100 requests/minute per user

Audit Logging:
  - All API calls logged
  - Model predictions logged (anonymized)
  - Access to logs restricted to security team
```

---

## 15. Accessibility Requirements

### 15.1 WCAG 2.1 AA Compliance

```yaml
Perceivable:
  - Color contrast ratio: Minimum 4.5:1 for text
  - AI indicator: Not color-only (includes icon + text)
  - Focus indicators: Visible outline on all interactive elements

Operable:
  - Keyboard navigation: All features accessible via keyboard
  - Focus order: Logical tab order (indicator → results → override)
  - No keyboard traps: Easy to navigate away from indicator

Understandable:
  - Clear language: "Learning mode" is user-friendly
  - Consistent behavior: Override works the same everywhere
  - Error identification: Clear messages for invalid queries

Robust:
  - Semantic HTML: Proper use of ARIA roles
  - Screen reader compatible: Tested with NVDA, VoiceOver
  - Graceful degradation: Works without JavaScript
```

### 15.2 Screen Reader Announcements

```html
<!-- Learning Mode Indicator -->
<div 
  role="status" 
  aria-live="polite"
  aria-label="Learning mode is active. Search results have been optimized to show tutorials first. Press Tab to access the Show Shorts button."
>
  🎓 Learning mode • Showing tutorials first
  <button aria-label="Show Shorts instead of tutorials">
    Show Shorts
  </button>
</div>

<!-- After Override -->
<div 
  role="status" 
  aria-live="polite"
  aria-label="Now showing all results including Shorts."
>
  Showing all results
  <button aria-label="Return to learning mode">
    Learning mode
  </button>
</div>
```

---

## 16. Internationalization

### 16.1 Supported Languages (v1)

```yaml
Tier 1 (Full Support):
  - English (en)
  - Spanish (es)
  - Portuguese (pt)
  - Hindi (hi)
  - Japanese (ja)
  - Korean (ko)
  - German (de)
  - French (fr)

Tier 2 (Keyword Only):
  - Arabic (ar)
  - Russian (ru)
  - Indonesian (id)
  - Thai (th)
  - Vietnamese (vi)
  - Turkish (tr)
  - Italian (it)
  - Polish (pl)

Implementation:
  - ML model: Multilingual DistilBERT handles Tier 1
  - Keyword rules: Translated signal lists for Tier 2
  - Fallback: English model + translation for unsupported
```

### 16.2 Localized Strings

```yaml
en:
  learning_mode_active: "Learning mode • Showing tutorials first"
  show_shorts: "Show Shorts"
  showing_all: "Showing all results"
  learning_mode_button: "Learning mode"

es:
  learning_mode_active: "Modo aprendizaje • Mostrando tutoriales primero"
  show_shorts: "Mostrar Shorts"
  showing_all: "Mostrando todos los resultados"
  learning_mode_button: "Modo aprendizaje"

hi:
  learning_mode_active: "लर्निंग मोड • पहले ट्यूटोरियल दिखा रहे हैं"
  show_shorts: "Shorts दिखाएं"
  showing_all: "सभी परिणाम दिखा रहे हैं"
  learning_mode_button: "लर्निंग मोड"

ja:
  learning_mode_active: "学習モード・チュートリアルを優先表示中"
  show_shorts: "ショートを表示"
  showing_all: "すべての結果を表示中"
  learning_mode_button: "学習モード"
```

---

## 17. Testing Requirements

### 17.1 Test Strategy

```yaml
Unit Tests:
  Coverage Target: 80%
  Focus Areas:
    - Intent classification logic
    - Ranking adjustment algorithms
    - Signal detection rules
    - Input validation
  
  Framework: Jest (Frontend), PyTest (Backend)

Integration Tests:
  Scope:
    - Intent service ↔ Search service
    - Search service ↔ Ranking service
    - Frontend ↔ API layer
  
  Framework: Cypress (E2E), Postman (API)

Load Tests:
  Scenarios:
    - 10,000 concurrent users
    - Sustained 5,000 QPS for 30 minutes
    - Spike: 0 to 20,000 QPS in 1 minute
  
  Tools: Locust, k6
  
  Success Criteria:
    - p99 latency < 100ms
    - Error rate < 0.1%
    - No memory leaks

ML Model Tests:
  - Accuracy on test set > 92%
  - Latency p99 < 50ms
  - No bias across languages (max 5% variance)
  - Adversarial input handling
```

### 17.2 Test Cases

```yaml
TC-001 Basic Learning Intent Detection:
  Input: "Python tutorial for beginners"
  Expected:
    - intent_type: "learning"
    - confidence: >= 0.90
    - detected_signals: ["tutorial", "beginners"]
  Result: PASS

TC-002 Entertainment Query (No Detection):
  Input: "funny cat videos"
  Expected:
    - intent_type: "entertainment"
    - confidence: >= 0.85
    - should_optimize: false
  Result: PASS

TC-003 Ambiguous Query:
  Input: "Python"
  Expected:
    - intent_type: "unknown" OR confidence < 0.85
    - should_optimize: false
  Result: PASS

TC-004 User Override:
  Precondition: Learning mode active
  Action: Click "Show Shorts"
  Expected:
    - Shorts return to original positions
    - Indicator changes to "Showing all results"
    - Override persists for session only
  Result: PASS

TC-005 Service Degradation:
  Precondition: Intent service unavailable
  Action: Perform search
  Expected:
    - Search returns results (standard ranking)
    - No AI indicator shown
    - No error visible to user
  Result: PASS

TC-006 Multilingual Query (Hindi):
  Input: "Python सीखें शुरुआत से"
  Expected:
    - intent_type: "learning"
    - confidence: >= 0.80
    - detected_signals: ["सीखें", "शुरुआत"] OR translated equivalents
  Result: PASS

TC-007 Mixed Intent Query:
  Input: "Python tutorial fails compilation"
  Expected:
    - Confidence reduced due to "fails"
    - should_optimize: false OR confidence < 0.85
  Result: PASS

TC-008 Accessibility - Screen Reader:
  Action: Navigate to search results with learning mode
  Expected:
    - Screen reader announces "Learning mode is active"
    - Tab navigates to "Show Shorts" button
    - Button announces its purpose
  Result: PASS
```

---

## 18. Launch Plan

### 18.1 Rollout Phases

```yaml
Phase 1 - Internal Alpha (Week 1-2):
  Audience: YouTube employees only (~10,000)
  Goals:
    - Validate technical stability
    - Gather initial feedback
    - Fix critical bugs
  Success Criteria:
    - Error rate < 1%
    - No P0 bugs
    - Positive internal feedback

Phase 2 - Limited Beta (Week 3-4):
  Audience: 1% of users (random selection)
  Goals:
    - Validate A/B test infrastructure
    - Measure early metrics
    - Tune confidence thresholds
  Success Criteria:
    - No regression in guardrail metrics
    - Abandonment rate trending down
    - Override rate < 15%

Phase 3 - Expanded Beta (Week 5-8):
  Audience: 10% of users
  Goals:
    - Statistical significance on primary metrics
    - Regional performance validation
    - Model performance monitoring
  Success Criteria:
    - Abandonment rate -20% vs control
    - Tutorial CTR +40% vs control
    - No Shorts watch time regression

Phase 4 - General Availability (Week 9+):
  Audience: 100% of users
  Goals:
    - Full rollout
    - Monitor for long-term effects
    - Plan v2 features
  Success Criteria:
    - Abandonment rate -30% sustained
    - Positive user sentiment
    - Creator sentiment stable
```

### 18.2 Go/No-Go Criteria

```yaml
Go Criteria (All Must Be Met):
  Technical:
    - [ ] p99 latency < 100ms for 7 consecutive days
    - [ ] Error rate < 0.5% for 7 consecutive days
    - [ ] No P0 or P1 bugs open
  
  Business:
    - [ ] Abandonment rate reduced by 25%+ (statistically significant)
    - [ ] Tutorial CTR increased by 30%+
    - [ ] Shorts watch time regression < 2%
  
  User Experience:
    - [ ] Override rate < 15%
    - [ ] No major accessibility issues
    - [ ] Positive sentiment in user feedback
  
  Operational:
    - [ ] Runbook completed and tested
    - [ ] On-call training completed
    - [ ] Rollback procedure verified

No-Go Triggers (Any Triggers Hold):
  - Shorts watch time drops > 5%
  - Error rate spikes > 2%
  - Override rate exceeds 25%
  - P0 bug in production
  - Legal/privacy concern raised
```

### 18.3 Rollback Plan

```yaml
Rollback Triggers:
  - Automatic: Error rate > 5% for 5 minutes
  - Manual: PM/Engineering decision

Rollback Procedure:
  1. Kill switch: Set feature flag to 0% (< 1 minute)
  2. Verify: Confirm intent service no longer called
  3. Monitor: Watch error rates return to baseline
  4. Communicate: Update status page, notify stakeholders
  5. Investigate: Root cause analysis

Rollback Timeline:
  - Detection to decision: < 5 minutes
  - Decision to execution: < 2 minutes
  - Execution to verification: < 5 minutes
  - Total: < 15 minutes

Post-Rollback:
  - Incident report within 24 hours
  - Fix identified within 48 hours
  - Re-launch plan within 1 week
```

---

## 19. Future Roadmap

### 19.1 v2 Features (Q2 2026)

```yaml
F-001 Personalized Learning Paths:
  Description: Create structured learning sequences based on user goals
  Value: Increase course completion rates
  Complexity: High
  Dependencies: User goal input, playlist integration

F-002 Progress Tracking:
  Description: Track learning progress across sessions
  Value: Re-engage learners, measure impact
  Complexity: Medium
  Dependencies: Watch history integration, user accounts

F-003 Skill Assessment:
  Description: Quiz-based skill evaluation before/after tutorials
  Value: Validate learning outcomes
  Complexity: High
  Dependencies: Quiz platform, content creator tools

F-004 Learning Mode Toggle:
  Description: Persistent toggle in settings for learning mode
  Value: User control, accessibility
  Complexity: Low
  Dependencies: Settings UI, preference storage
```

### 19.2 v3 Features (Q4 2026)

```yaml
F-005 AI-Powered Recommendations:
  Description: "What to learn next" based on watch history
  Value: Increase learning engagement
  Complexity: High
  Dependencies: Recommendation engine, learning graph

F-006 Creator Tools for Educators:
  Description: Analytics dashboard for educational creators
  Value: Creator ecosystem growth
  Complexity: Medium
  Dependencies: Creator Studio integration

F-007 Live Learning Sessions:
  Description: Scheduled live tutorials with Q&A
  Value: Interactive learning
  Complexity: High
  Dependencies: Live streaming, calendar integration
```

---

## 20. Appendix

### 20.1 Glossary

| Term | Definition |
|------|------------|
| **Learning Intent** | User's goal to acquire knowledge or skills through video content |
| **Shorts** | YouTube videos under 60 seconds, vertical format |
| **Tutorial** | Educational video typically >10 minutes with structured content |
| **Intent Detection** | AI/ML process of classifying user query purpose |
| **Confidence Score** | Probability (0-1) that detected intent is correct |
| **Override** | User action to disable learning mode optimization |
| **Abandonment Rate** | % of searches where user exits without clicking |
| **CTR** | Click-through rate (clicks / impressions) |
| **Guardrail Metric** | Metric that must not regress during experiment |

### 20.2 References

- [YouTube Search Quality Guidelines](internal-link)
- [ML Model Training Pipeline Documentation](internal-link)
- [A/B Testing Framework Guide](internal-link)
- [Accessibility Compliance Checklist](internal-link)
- [Privacy Review Process](internal-link)

### 20.3 Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-12-26 | Swapan Kumar Soren | Initial PRD |

### 20.4 Approval Sign-Off

| Role | Name | Date | Signature |
|------|------|------|-----------|
| Product Manager | | | |
| Engineering Lead | | | |
| UX Design Lead | | | |
| ML Engineering Lead | | | |
| Legal/Privacy | | | |

---

## Quick Start for AI Platforms

### For Lovable / v0 / Bolt.new:

```
Build a YouTube clone with AI-powered learning intent detection:

1. Create a search interface with dark theme (colors in Section 8.1)
2. When user searches, analyze query for learning keywords:
   - "tutorial", "how to", "learn", "beginners", "course", "explained"
3. If learning intent detected (>85% confidence):
   - Show blue indicator: "🎓 Learning mode • Showing tutorials first"
   - Prioritize videos >10 minutes, demote Shorts (<60s)
   - Add "Show Shorts" override link
4. Include before/after comparison view for demo
5. Use mock video data (tutorials and shorts)
6. Mobile responsive design
```

### For Google AI Studio / Claude:

```
Generate React components for:
1. LearningModeIndicator component (specs in Section 8.2.1)
2. VideoResultCard component with tutorial/short variants
3. IntentDetector service class with keyword analysis
4. SearchResults container with before/after comparison

Use TypeScript, Tailwind CSS, and follow the design system in Section 8.1.
```

---

*End of Document*
