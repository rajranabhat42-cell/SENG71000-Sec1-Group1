# Mobile Driver Analytics System

A smartphone-based driver analytics system for an insurance use case. The system replaces a dedicated vehicle tracking device with the customer's Android smartphone. The app records driving information during a manually started and stopped trip, calculates a driver score, provides driving insights, and allows authorized insurance administrators to review customer-approved information.

## Project Context

**Course:** SENG71000 - Systems Analysis and Design  
**Project:** Mobile Driver Analytics System  
**Group:** Group 1  
**Professor:** Akrem El-ghazal

### Problem

The current solution uses a device installed in a customer's vehicle to collect driving information such as speed, location, and acceleration. The device has hardware, shipping, and SIM data-plan costs.

The proposed system uses the customer's smartphone instead. This reduces the need for dedicated tracking hardware and separate connectivity while allowing the company to collect driving information and support insurance risk assessment and pricing decisions.

### Initial Version

For the initial version:

- Customers manually start a trip.
- Customers manually stop a trip.
- The Android phone collects the required driving information during the session.
- Automatic trip detection is a future enhancement.
- The project should start small and grow incrementally.

## Core System Capabilities

1. **Customer Accounts and Privacy**
   - Register and log in.
   - Manage consent for sharing driving information.
   - Restrict records according to roles and permissions.

2. **Trip Recording and Data Management**
   - Manually start and stop trip recording.
   - Collect location, speed, acceleration, deceleration, distance, duration, and time-of-day information.
   - Transfer and store trip information.

3. **Driving Analysis and Scoring**
   - Compare recorded speeds with available posted speed limits.
   - Detect speeding, harsh braking, and rapid acceleration.
   - Generate a driver score out of 100.
   - Provide a score breakdown.

4. **Trip History and Driving Insights**
   - View previous trips.
   - View mapped routes.
   - View detected driving events.
   - View driving statistics and score breakdowns.

5. **Personalized Driving Progress**
   - Compare performance across categories and time periods.
   - Display charts.
   - Highlight improvements, recurring issues, and priority areas.

6. **Insurance Assessment Support**
   - Allow authorized insurance administrators to review customer-approved scores, trip details, and performance summaries.
   - Support risk assessment, pricing, and potential discount decisions.

## Proposed Technology Stack

| Area | Technology |
|---|---|
| Mobile app | React Native + TypeScript |
| Mobile platform | Android |
| Web/admin dashboard | Next.js + TypeScript |
| API/backend | Node.js + TypeScript |
| Database | PostgreSQL through Supabase |
| Authentication | Supabase Auth |
| Deployment | Vercel for the web application; Supabase for database/auth services |
| Version control | Git + GitHub |
| Testing | Jest + React Native Testing Library |
| Location | Android phone GPS |

### Why This Stack?

The group wants a stack that is realistic for a course project but also useful for industry learning.

The main language is **TypeScript**. It can be used across the mobile application, web application, and Node.js API. This reduces the number of different languages the group needs to learn.

React Native is being used for the Android mobile application. The course material presents React Native with JavaScript/TypeScript as a cross-platform mobile development option.

Node.js + TypeScript is used for the API so the group can learn real backend concepts instead of relying entirely on a managed backend.

Supabase is used for PostgreSQL and authentication so the group does not have to spend project time building database infrastructure and authentication from scratch.

Next.js + TypeScript is planned for the insurance administrator web dashboard.

## High-Level Architecture

```text
                    Android Phone
              React Native + TypeScript
                         |
                      REST API
                         |
                         v
               Node.js + TypeScript
                    Backend API
                         |
                         v
               Supabase PostgreSQL
                  + Supabase Auth
                         ^
                         |
                Next.js + TypeScript
                 Admin Web Dashboard
```

### Main Data Flow

```text
Customer
   |
   v
Start Trip
   |
   v
Phone collects driving data
   |
   v
React Native app
   |
   v
Node.js REST API
   |
   v
PostgreSQL / Supabase
   |
   +--> Driving analysis
   |
   +--> Driver score
   |
   +--> Trip history
   |
   +--> Admin dashboard
```

## Important Domain Data

The likely domain entities include:

- Customer
- Trip
- Trip Data Point
- Driving Event
- Driver Score
- Consent
- Insurance Administrator

These should be validated against the team's use cases and domain model before the database schema is finalized.

## Suggested Database Direction

A possible starting structure is:

```text
users
customers
trips
trip_data
driving_events
scores
consents
```

Example trip information:

```text
trip_id
customer_id
start_time
end_time
distance
duration
score
```

Example trip data point:

```text
trip_data_id
trip_id
timestamp
latitude
longitude
speed
acceleration
```

The final schema should be based on the team's domain model and use cases rather than being treated as final now.

## API Direction

The backend should expose REST-style endpoints.

Possible starting endpoints:

```text
POST   /api/auth/register
POST   /api/auth/login

POST   /api/trips
POST   /api/trips/:tripId/data
POST   /api/trips/:tripId/stop

GET    /api/trips
GET    /api/trips/:tripId
GET    /api/trips/:tripId/score

GET    /api/admin/customers
GET    /api/admin/customers/:customerId/trips
GET    /api/admin/customers/:customerId/summary
```

These are starting ideas, not final requirements. The final API should follow the team's use cases.

## First Development Plan

### Phase 1 - Project Setup

- Create GitHub repository.
- Set up React Native + TypeScript Android project.
- Set up Node.js + TypeScript backend.
- Set up Next.js + TypeScript dashboard.
- Create Supabase project.
- Establish environment-variable handling.
- Make sure every team member can run the project.

### Phase 2 - Simple Mobile UI

Build:

- Login screen
- Home screen
- Start Trip button
- Stop Trip button
- Current trip screen
- Trip history screen

Do not build advanced analytics yet.

### Phase 3 - GPS Prototype

- Request Android location permission.
- Start collecting GPS data when a trip begins.
- Record timestamp, latitude, longitude, and available speed information.
- Stop collection when the trip ends.
- Temporarily display collected data in the app.

### Phase 4 - API

Build the first REST API.

Example:

```text
Mobile App
    |
POST /api/trips
    |
Backend
    |
Database
```

Then add:

```text
POST /api/trips/:id/data
GET  /api/trips
GET  /api/trips/:id
```

### Phase 5 - Database

Create the PostgreSQL tables based on the domain model.

Connect:

```text
Node.js API -> Supabase PostgreSQL
```

### Phase 6 - Driving Analysis

Start with simple rules:

- Speeding event
- Rapid acceleration
- Harsh braking
- Trip distance
- Trip duration

Then create a score out of 100.

The exact scoring formula should be agreed on by the group and documented.

### Phase 7 - Customer Insights

Add:

- Trip history
- Route display
- Score breakdown
- Driving event list
- Basic charts
- Personal progress

### Phase 8 - Admin Dashboard

Build:

- Administrator login
- Customer list
- Customer trip history
- Score summaries
- Approved customer information
- Basic analytics

### Phase 9 - Testing

Test:

- Authentication
- Trip start/stop
- GPS data collection
- API requests
- Database operations
- Score calculation
- Permissions
- Admin access

### Phase 10 - Deployment

Deploy the web dashboard and backend/database services, then test the deployed system.

## Development Rules for the Group

1. Keep the first version simple.
2. Do not add features that are not needed for the current project phase.
3. Use TypeScript consistently.
4. Use GitHub for all code.
5. Create branches for significant features.
6. Make small commits with clear messages.
7. Pull/rebase or synchronize before starting new work.
8. Do not commit passwords, API keys, or Supabase secrets.
9. Keep environment variables in `.env` files that are excluded from Git.
10. Document important technical decisions.
11. Do not change the stack without discussing it with the group.
12. Make sure everyone can run the project locally before adding advanced features.

## Suggested Team Responsibilities

The group can divide work roughly into:

### Mobile Developer(s)
- React Native
- TypeScript
- Android UI
- GPS/location collection
- Trip recording

### Backend Developer(s)
- Node.js
- TypeScript
- REST API
- Authentication integration
- Business logic

### Database/Data Developer(s)
- PostgreSQL
- Supabase
- Database schema
- Queries
- Data relationships

### Web/Admin Developer(s)
- Next.js
- TypeScript
- Insurance administrator dashboard
- Charts and reporting

Everyone should understand the overall architecture even if each person owns a particular area.

## Important Technical Decisions Still Open

Do not pretend these are finalized:

- Exact Node.js API framework.
- Exact React Native location/GPS library.
- Exact scoring formula.
- Exact database schema.
- Exact role/permission model.
- Exact dashboard features.
- Exact deployment setup for the Node.js API.
- Whether all sensor values can be obtained directly from the Android phone or whether some values need to be calculated/simulated.

These should be decided after the first technical prototype.

## Definition of a Good First Prototype

The first prototype does NOT need the entire system.

A successful first prototype should demonstrate:

```text
Open Android App
      |
      v
Start Trip
      |
      v
Collect GPS information
      |
      v
Stop Trip
      |
      v
Send trip data to API
      |
      v
Store data in Supabase
      |
      v
Retrieve and display trip
```

Once this works, the team can build scoring, analytics, and the administrator dashboard.

## Notes for Future AI Assistance

When helping with this project:

- Treat the System Vision as the main source of project requirements.
- Do not invent requirements that are not supported by the System Vision or later team decisions.
- Keep the implementation appropriate for a student course project.
- Prefer simple solutions before advanced architecture.
- Explain unfamiliar technologies before using them.
- Use TypeScript throughout the main application stack.
- Keep the React Native Android application as the primary customer interface.
- Keep Node.js as the API/backend learning component.
- Use Supabase for PostgreSQL and authentication.
- Use Next.js for the administrator web dashboard.
- Help the team build incrementally.
- If a technical decision is still marked "open", ask before treating it as finalized.
