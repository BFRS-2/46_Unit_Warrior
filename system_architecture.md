# AI-Powered Courier Rule Engine - System Architecture

## High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           FRONTEND (Angular 12)                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐  │
│  │   Rule Builder  │    │   Rule Listing  │    │     Local Storage       │  │
│  │   Component     │    │   Component     │    │   (localStorage API)    │  │
│  │                 │    │                 │    │                         │  │
│  │ • Natural Lang  │    │ • View Saved    │    │ • myRules[]             │  │
│  │   Input         │    │   Rules         │    │ • Rule History          │  │
│  │ • Rule Preview  │    │ • Delete Rules  │    │ • Persistent Data       │  │
│  │ • Save Rules    │    │ • Toggle Views  │    │                         │  │
│  └─────────────────┘    └─────────────────┘    └─────────────────────────┘  │
│           │                       │                       │                  │
│           └───────────────────────┼───────────────────────┘                  │
│                                   │                                          │
│  ┌─────────────────────────────────┼─────────────────────────────────────────┐  │
│  │                    API Service Layer                                     │  │
│  │                                                                           │  │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────┐   │  │
│  │  │   ApiService    │    │   HttpClient    │    │   Rule Parser       │   │  │
│  │  │                 │    │   (Angular)     │    │   (OpenAI GPT)      │   │  │
│  │  │ • generateRule()│◄──►│ • HTTP Requests │◄──►│ • Natural Lang      │   │  │
│  │  │ • buildPrompt() │    │ • JSON Parsing  │    │   Processing        │   │  │
│  │  │ • Error Handling│    │ • Headers Mgmt  │    │ • Rule Structuring  │   │  │
│  │  └─────────────────┘    └─────────────────┘    └─────────────────────┘   │  │
│  └───────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           EXTERNAL SERVICES                                   │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────────┐ │
│  │                        OpenAI API (GPT-3.5-turbo)                          │ │
│  │                                                                             │ │
│  │  • Natural Language Processing                                              │ │
│  │  • Rule Parsing & Structuring                                              │ │
│  │  • JSON Response Generation                                                 │ │
│  │  • Context-Aware Rule Interpretation                                       │ │
│  └─────────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           DATA LAYER                                          │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────────┐ │
│  │                    Static Data (JSON Assets)                               │ │
│  │                                                                             │ │
│  │  • couriers_detailed.json                                                  │ │
│  │    - 50+ Courier Profiles                                                  │ │
│  │    - Weight Limits, Service Types                                          │ │
│  │    - Tracking Features, Contact Info                                       │ │
│  │    - Geographic Coverage, Payment Modes                                    │ │
│  └─────────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## Component Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           Angular Application                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                           App Module                                     │ │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────┐  │ │
│  │  │  BrowserModule  │  │  FormsModule    │  │   HttpClientModule      │  │
│  │  │                 │  │                 │  │                         │  │
│  │  │ • DOM Rendering │  │ • Two-way       │  │ • HTTP Communication    │  │
│  │  │ • Lifecycle     │  │   Binding       │  │ • API Calls             │  │
│  │  │ • Change        │  │ • Form          │  │ • JSON Handling         │  │
│  │  │   Detection     │  │   Validation    │  │                         │  │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────────────┘  │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
│                                   │                                          │
│                                   ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────────┐ │
│  │                      Rule Builder Component                             │ │
│  │                                                                         │ │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────┐  │ │
│  │  │   Template      │  │   Component     │  │       Service           │  │ │
│  │  │   (HTML)        │  │   (TypeScript)  │  │      (ApiService)       │  │ │
│  │  │                 │  │                 │  │                         │  │ │
│  │  │ • Tab Interface │  │ • State Mgmt    │  │ • OpenAI Integration    │  │ │
│  │  │ • Form Inputs   │  │ • Data Binding  │  │ • Rule Generation       │  │ │
│  │  │ • Results       │  │ • Business      │  │ • Error Handling        │  │ │
│  │  │   Display       │  │   Logic         │  │                         │  │ │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────────────┘  │ │
│  │                                   │                                      │ │
│  │  ┌─────────────────────────────────┼──────────────────────────────────────┐ │ │
│  │  │                           Styles (SCSS)                              │ │ │
│  │  │                                                                       │ │ │
│  │  │ • Responsive Design                                                 │ │ │
│  │  │ • Modern UI Components                                              │ │ │
│  │  │ • Animation & Transitions                                           │ │ │
│  │  │ • Mobile-First Approach                                             │ │ │
│  │  └──────────────────────────────────────────────────────────────────────┘ │ │
│  └─────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Data Flow Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   User Input    │───►│  Rule Builder   │───►│   ApiService    │
│                 │    │   Component     │    │                 │
│ • Natural Lang  │    │                 │    │ • HTTP Request  │
│ • Rule Text     │    │ • Input         │    │ • Prompt Build  │
│                 │    │   Validation    │    │ • Response      │
└─────────────────┘    └─────────────────┘    │   Parsing       │
                                              └─────────────────┘
                                                       │
                                                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   LocalStorage  │◄───│  Rule Builder   │◄───│   OpenAI API    │
│                 │    │   Component     │    │                 │
│ • Rule History  │    │                 │    │ • GPT-3.5-turbo │
│ • Persistent    │    │ • Rule Parsing  │    │ • JSON Response │
│   Data          │    │ • Filtering     │    │ • Structured    │
└─────────────────┘    └─────────────────┘    │   Rules         │
                                              └─────────────────┘
                                                       │
                                                       ▼
                                              ┌─────────────────┐
                                              │  Courier Data   │
                                              │   (JSON File)   │
                                              │                 │
                                              │ • 50+ Couriers  │
                                              │ • Attributes    │
                                              │ • Constraints   │
                                              └─────────────────┘
```

## Security Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           SECURITY LAYERS                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────────┐  │
│  │   Frontend      │    │   API Layer     │    │   Data Layer            │  │
│  │   Security      │    │   Security      │    │   Security              │  │
│  │                 │    │                 │    │                         │  │
│  │ • Input         │    │ • API Key       │    │ • Static Data           │  │
│  │   Validation    │    │   Management    │    │   Only                  │  │
│  │ • XSS           │    │ • HTTPS         │    │ • No Sensitive          │  │
│  │   Prevention    │    │   Encryption    │    │   Information           │  │
│  │ • CSRF          │    │ • Rate Limiting │    │ • Local Storage         │  │
│  │   Protection    │    │ • Error         │    │   Encryption            │  │
│  └─────────────────┘    │   Handling      │    └─────────────────────────┘  │
│                         └─────────────────┘                                 │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Technology Stack

### Frontend
- **Framework**: Angular 12
- **Language**: TypeScript
- **Styling**: SCSS
- **HTTP Client**: Angular HttpClient
- **State Management**: Local Storage + Component State
- **UI Components**: Custom Components

### External Services
- **AI/ML**: OpenAI GPT-3.5-turbo
- **API**: RESTful HTTP API
- **Authentication**: API Key-based

### Data Storage
- **Static Data**: JSON files (couriers_detailed.json)
- **User Data**: Browser LocalStorage
- **Session Data**: In-memory component state

### Development Tools
- **Package Manager**: npm
- **Build Tool**: Angular CLI
- **Version Control**: Git
- **Development Server**: Angular Dev Server

## Key Features & Capabilities

### 1. Natural Language Processing
- Plain English rule input
- AI-powered rule parsing
- Context-aware interpretation
- Multi-language support potential

### 2. Rule Management
- Create custom rules
- Save/load rules locally
- Delete rules
- Rule history tracking

### 3. Courier Matching
- Real-time filtering
- Multi-criteria matching
- Weight-based selection
- Geographic considerations

### 4. User Experience
- Responsive design
- Tab-based interface
- Real-time feedback
- Error handling

### 5. Data Management
- Local data persistence
- Static courier database
- Rule validation
- Data integrity

## Scalability Considerations

### Current Architecture (POC)
- Single-page application
- Client-side processing
- Local data storage
- Direct API integration

### Future Scalability Options
- **Backend Integration**: Node.js/Express server
- **Database**: MongoDB/PostgreSQL for rules
- **Caching**: Redis for performance
- **Microservices**: Rule engine as separate service
- **Cloud Deployment**: AWS/Azure/GCP
- **API Gateway**: Rate limiting, authentication
- **Load Balancing**: Multiple instances
- **Monitoring**: Application performance monitoring

## Performance Characteristics

### Frontend Performance
- **Bundle Size**: ~2-3MB (Angular + dependencies)
- **Load Time**: <3 seconds (initial load)
- **Rule Generation**: <5 seconds (API dependent)
- **Filtering**: <100ms (client-side)

### API Performance
- **Response Time**: 2-4 seconds (OpenAI dependent)
- **Throughput**: Limited by OpenAI rate limits
- **Caching**: None (real-time processing)

### Data Performance
- **Courier Data**: <100KB (static JSON)
- **Rule Storage**: <1MB (localStorage)
- **Memory Usage**: <50MB (typical usage) 