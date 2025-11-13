# Omron Doctor Dashboard - Dataflow Diagram

## System Overview

This document describes the data flow architecture of the Omron Doctor Dashboard application, a healthcare monitoring system for medical professionals to manage patients and their vital signs.

## Architecture Diagram

```mermaid
graph TB
    %% User Layer
    User[Doctor/Healthcare Professional]
    NativeApp[Native Mobile App<br/>iOS/Android]

    %% Presentation Layer
    subgraph "Frontend Layer - React Framework7"
        Login[Login Screen]
        PatientList[Patient List Screen]
        PatientDetail[Patient Detail Screen]
        MedicationCheck[Medication Check Screen]
        ChatList[Chat List Screen]
        ChatPanel[Chat Panel Screen]

        subgraph "React Components"
            PatientInfo[Patient Info Component]
            HistoryGraph[History Graph Component]
            Medicine[Medicine Component]
            Prescription[Prescription Component]
            GoalSetting[Goal Setting Component]
            ConditionOfPatient[Condition Component]
            ChatPopup[Chat Popup Component]
            Sync[Sync Component]
        end
    end

    %% Service Layer
    subgraph "Service Layer"
        AuthGuard[AuthGuard Service<br/>Authentication Management]
        API[API Service<br/>HTTP Client]
        Network[Network Service<br/>Connection Monitoring]
        Cache[Cache Service<br/>Local Data Store]
        DataTransfer[DataTransfer Service<br/>State Management]
        Native[Native Service<br/>Bridge to Mobile]
    end

    %% Backend Layer
    subgraph "Backend API Layer"
        AuthAPI[Authentication API]
        PatientAPI[Patient Management API]
        VitalsAPI[Vitals/BP API]
        MedicationAPI[Medication/Drugs API]
        ChatAPI[Chat/Messaging API]
        BatchAPI[Batch Processing API]
        GoalAPI[Goal Setting API]
        ConditionsAPI[Conditions API]
    end

    %% Data Storage
    subgraph "Data Storage"
        Database[(Database<br/>Patient Records<br/>Vitals Data<br/>Medications)]
    end

    %% External Services
    subgraph "External Services"
        GoogleAnalytics[Google Analytics<br/>Tracking]
        PusherService[Pusher Service<br/>Real-time Messaging<br/>WebSocket]
        OmronDevices[Omron Medical Devices<br/>BP Monitors]
    end

    %% User Interactions
    User -->|Accesses| NativeApp
    User -->|Accesses| Login

    %% Native App Communication
    NativeApp <-->|Bridge Communication| Native

    %% Screen Navigation Flow
    Login -->|Successful Auth| PatientList
    PatientList -->|Select Patient| PatientDetail
    PatientList -->|View Messages| ChatList
    PatientDetail -->|View/Edit| PatientInfo
    PatientDetail -->|View History| HistoryGraph
    PatientDetail -->|Manage Meds| Medicine
    PatientDetail -->|Prescribe| Prescription
    PatientDetail -->|Set Goals| GoalSetting
    PatientDetail -->|View Conditions| ConditionOfPatient
    ChatList -->|Open Chat| ChatPanel
    PatientDetail -->|Sync Data| Sync

    %% Component to Service Communication
    Login --> AuthGuard
    PatientList --> API
    PatientDetail --> API
    MedicationCheck --> API
    ChatList --> API
    ChatPanel --> API

    %% All Components use these services
    PatientInfo --> API
    HistoryGraph --> API
    Medicine --> API
    Prescription --> API
    GoalSetting --> API
    ConditionOfPatient --> API
    Sync --> API

    %% Service Layer Communication
    AuthGuard --> API
    API --> Network
    API -.->|Caches Data| Cache
    Cache -.->|Retrieves Cache| API
    DataTransfer -.->|Shares State| PatientList
    DataTransfer -.->|Shares State| PatientDetail
    Network -.->|Monitors Connection| API

    %% API to Backend Communication
    API -->|POST /login<br/>GET /logout<br/>GET /is-authenticated| AuthAPI
    API -->|GET /doctor/patient_list<br/>GET /doctor/patient<br/>POST /doctor/patients<br/>GET /doctor/data| PatientAPI
    API -->|GET /doctor/graph<br/>POST /doctor/office-bp<br/>GET /doctor/history-office<br/>GET /doctor/history-home| VitalsAPI
    API -->|GET /doctor/drugs<br/>POST /doctor/drugs<br/>DELETE /doctor/drugs/{id}<br/>POST /doctor/drugs/refill<br/>POST /doctor/risk| MedicationAPI
    API -->|POST /chat/user/userLogin<br/>GET /chat/user/getMessageList<br/>POST /chat/user/createMessage<br/>GET /chat/user/doctorDialogsList<br/>POST /chat/user/createDialog<br/>GET /chat/user/findNotificationList<br/>POST /chat/user/pushNotification| ChatAPI
    API -->|GET /batch/execute/user/{id}| BatchAPI
    API -->|GET /doctor/goal<br/>POST /doctor/goal| GoalAPI
    API -->|GET /doctor/conditions<br/>POST /doctor/conditions| ConditionsAPI

    %% Backend to Database
    AuthAPI --> Database
    PatientAPI --> Database
    VitalsAPI --> Database
    MedicationAPI --> Database
    ChatAPI --> Database
    BatchAPI --> Database
    GoalAPI --> Database
    ConditionsAPI --> Database

    %% External Service Integration
    PatientList -.->|Track Page Views| GoogleAnalytics
    PatientDetail -.->|Track Interactions| GoogleAnalytics
    Login -.->|Track Login Events| GoogleAnalytics

    ChatAPI <-.->|Real-time Messages| PusherService
    ChatPanel <-.->|WebSocket Events| PusherService

    VitalsAPI <-.->|Device Data| OmronDevices
    Sync -.->|Sync Measurements| OmronDevices

    BatchAPI -.->|Batch Events<br/>BatchStarted<br/>BatchCompleted<br/>BatchFailed| PusherService

    %% Styling
    classDef userClass fill:#e1f5ff,stroke:#01579b,stroke-width:2px
    classDef frontendClass fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef serviceClass fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef backendClass fill:#e8f5e9,stroke:#1b5e20,stroke-width:2px
    classDef storageClass fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    classDef externalClass fill:#fff9c4,stroke:#f57f17,stroke-width:2px

    class User,NativeApp userClass
    class Login,PatientList,PatientDetail,MedicationCheck,ChatList,ChatPanel,PatientInfo,HistoryGraph,Medicine,Prescription,GoalSetting,ConditionOfPatient,ChatPopup,Sync frontendClass
    class AuthGuard,API,Network,Cache,DataTransfer,Native serviceClass
    class AuthAPI,PatientAPI,VitalsAPI,MedicationAPI,ChatAPI,BatchAPI,GoalAPI,ConditionsAPI backendClass
    class Database storageClass
    class GoogleAnalytics,PusherService,OmronDevices externalClass
```

## Data Flow Details

### 1. Authentication Flow
```
User → Login Screen → AuthGuard Service → API Service → Backend Auth API → Database
                                    ↓
                              Session Cookie
                                    ↓
                         Redirect to Patient List
```

### 2. Patient Data Flow
```
Doctor → Patient List → API Service → Patient API → Database
              ↓
       Select Patient
              ↓
      Patient Detail Screen → API Service → Multiple APIs
                                              ├── Patient API (demographics)
                                              ├── Vitals API (BP readings)
                                              ├── Medication API (prescriptions)
                                              ├── Goal API (treatment goals)
                                              └── Conditions API (patient conditions)
```

### 3. Blood Pressure Measurement Flow
```
Omron BP Device → Patient's Phone → Sync → Backend API → Database
                                                    ↓
Doctor Dashboard → History Graph Component → Vitals API → Display Graph
```

### 4. Medication Management Flow
```
Doctor → Patient Detail → Medicine Component → API Service → Medication API
                              ↓
                     View/Add/Edit Medications
                              ↓
                    Risk Assessment Check (Drug Interactions)
                              ↓
                    POST /doctor/risk → Database
                              ↓
                    Display Risk Warnings
```

### 5. Chat/Messaging Flow
```
Doctor → Chat List → API Service → Chat API → Database
            ↓
     Select Patient Chat
            ↓
       Chat Panel ←→ Pusher WebSocket ←→ Chat API
            ↓
    Real-time Messages
```

### 6. Batch Processing Flow
```
Trigger Batch Job → Batch API → Database
                        ↓
                  Pusher Events
                        ↓
           ├── BatchStarted Event
           ├── BatchCompleted Event
           └── BatchFailed Event
                        ↓
            Frontend Listeners Update UI
```

## Key Components

### Frontend Layer (React + Framework7)
- **Screens**: Login, PatientList, PatientDetail, MedicationCheck, ChatList, ChatPanel
- **Components**: Reusable UI components for patient info, graphs, medications, etc.
- **Routing**: Framework7 routing with main view navigation

### Service Layer
- **AuthGuard**: Manages authentication state and session validation
- **API Service**: Centralized HTTP client with jQuery AJAX, handles all API requests
- **Network Service**: Monitors online/offline status, shows alerts for connection issues
- **Cache Service**: Stores frequently accessed data (CONDITIONS, DRUGS)
- **DataTransfer**: Singleton for sharing state between components
- **Native Service**: Bridge communication with iOS/Android native features

### Backend API Endpoints

#### Authentication
- `POST /login` - Doctor login
- `GET /logout` - End session
- `GET /is-authenticated` - Session validation
- `POST /doctor/reset` - Password reset

#### Patient Management
- `GET /doctor/patient_list?page={page}&limit={limit}` - Paginated patient list
- `GET /doctor/patient?user-id={id}` - Patient details
- `GET /doctor/data/{userId}` - Patient data for display
- `GET /doctor/data2/{userId}` - Patient data for chat
- `POST /doctor/patients` - Update patient information

#### Vitals/Blood Pressure
- `GET /doctor/graph?user-id={id}&start-date={date}&end-date={date}` - BP readings for graph
- `POST /doctor/office-bp` - Save office BP measurement
- `GET /doctor/history-office/{userId}` - Office BP history
- `GET /doctor/history-home/{userId}` - Home BP history

#### Medications/Drugs
- `GET /doctor/drugs/{userId}/{date}` - Get patient medications
- `POST /doctor/drugs` - Add/update medications
- `DELETE /doctor/drugs/{id}` - Remove medication
- `POST /doctor/drugs/refill` - Refill prescription
- `POST /doctor/risk` - Check drug interactions/risks

#### Goals & Conditions
- `GET /doctor/goal?user-id={id}` - Get BP goal
- `POST /doctor/goal` - Set BP goal
- `GET /doctor/conditions/{userId}/{date}` - Get patient conditions
- `POST /doctor/conditions` - Update conditions

#### Chat/Messaging
- `POST /chat/user/userLogin` - Authenticate for chat
- `GET /chat/user/doctorDialogsList?doctorId={id}` - List conversations
- `POST /chat/user/createDialog` - Start new conversation
- `GET /chat/user/getMessageList?userId={id}&chatDialogId={id}` - Get messages
- `POST /chat/user/createMessage` - Send message
- `GET /chat/user/findNotificationList` - Get notifications
- `POST /chat/user/pushNotification` - Send push notification
- `POST /doctor/syncUser` - Sync user data for chat

#### Batch Processing
- `GET /batch/execute/user/{userId}` - Execute batch operations

### External Integrations

#### Google Analytics
- Tracks page views and user interactions
- Events: Login, Patient views, Medication changes
- Configuration: GA_ID from environment variables

#### Pusher (Real-time)
- WebSocket service for real-time messaging
- Channels: Chat messages, Batch processing events
- Events: BatchStarted, BatchCompleted, BatchFailed

#### Omron Medical Devices
- Blood pressure monitors integration
- Data sync through patient mobile apps
- Measurements automatically uploaded to backend

## Security Features

1. **Authentication**: Session-based authentication with cookies
2. **CORS**: Credentials included in API requests (`withCredentials: true`)
3. **UTC Offset**: Timezone information sent with each request
4. **Headers**: Custom headers for app authentication (`X-app` header)
5. **Network Validation**: Requests blocked when offline

## Technology Stack

- **Frontend**: React 15.6.1, Framework7 1.5.3
- **Charts**: Recharts for BP graphs
- **HTTP Client**: jQuery AJAX
- **Real-time**: Laravel Echo + Pusher
- **Styling**: SCSS with node-sass
- **Build**: React Scripts (Webpack)
- **Routing**: Framework7 routing + React Router DOM

## Data Persistence

### Local Cache
- Conditions data (patient health conditions)
- Drugs data (medications)
- Temporary storage for offline scenarios

### Backend Database
- Patient demographics and profiles
- Vital signs measurements (BP readings)
- Medication prescriptions and history
- Chat messages and conversations
- Treatment goals and conditions
- Doctor accounts and sessions

## Network Flow Summary

```
[Doctor Device] → [React App] → [Service Layer] → [API Gateway] → [Backend Services] → [Database]
                       ↓                                                      ↑
                  [Local Cache]                                    [Pusher WebSocket]
                                                                            ↓
                                                                   [Real-time Updates]
```

## Error Handling

1. **Network Errors**: Network service detects offline status and shows modal alert
2. **API Errors**: Status codes handled in AJAX configuration
3. **Session Expiration**: AuthGuard redirects to login when session expires
4. **Validation**: Frontend validation before API calls

## Performance Optimizations

1. **Caching**: Frequently accessed data cached in Cache service
2. **Pagination**: Patient list uses pagination (page/limit parameters)
3. **Lazy Loading**: React Virtualized for large lists
4. **Data Transfer**: Shared state management to avoid redundant API calls

---

**Document Version**: 1.0
**Last Updated**: 2025-11-13
**Application**: Omron Doctor Dashboard
**Source Branch**: staging
