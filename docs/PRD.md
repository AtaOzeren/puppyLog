# PuppyLog - Pet Care Tracking Application PRD

## 1. Product Overview

PuppyLog is a mobile application designed to help pet owners track their pets' daily activities, health records, and set reminders for routine care tasks. The application uses Expo for frontend development and implements a local backend using Prisma with SQLite.

## 2. Technical Architecture

### Frontend

- **Framework:** Expo (React Native)
- **UI Components:** TailwindCSS with NativeWind
- **State Management:** React Context or Redux

### Backend (Local)

- **ORM:** Prisma
- **Database:** SQLite
- **Storage:** Local device storage for offline functionality
  - SQLite database file stored in app's local storage
  - Automatic database versioning and migrations

### Project Structure

```
puppyLog/
├── src/
│   ├── features/
│   │   ├── user/
│   │   │   ├── components/
│   │   │   └── services/
│   │   ├── pet/
│   │   │   ├── components/
│   │   │   └── services/
│   │   ├── activity/
│   │   │   ├── components/
│   │   │   └── services/
│   │   └── reminder/
│   │       ├── components/
│   │       └── services/
│   ├── screens/
│   │   ├── HomeScreen.tsx
│   │   ├── DashboardScreen.tsx
│   │   ├── SettingsScreen.tsx
│   │   ├── ActivityListScreen.tsx
│   │   ├── AddActivityScreen.tsx
│   │   ├── ActivityDetailScreen.tsx
│   │   ├── ActivityHistoryScreen.tsx
│   │   ├── UserSetupScreen.tsx
│   │   ├── UserProfileScreen.tsx
│   │   ├── PetSetupScreen.tsx
│   │   ├── PetDetailScreen.tsx
│   │   ├── PetProfileScreen.tsx
│   │   ├── ReminderListScreen.tsx
│   │   ├── AddReminderScreen.tsx
│   │   └── ReminderSettingsScreen.tsx
│   ├── shared/
│   │   ├── components/
│   │   │   ├── buttons/
│   │   │   ├── cards/
│   │   │   ├── forms/
│   │   │   └── layout/
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── constants/
│   │   └── types/
│   ├── database/
│   │   ├── schema/
│   │   └── migrations/
│   └── navigation/
│       ├── AppNavigator.tsx
│       ├── HomeStack.tsx
│       ├── ActivityStack.tsx
│       └── ReminderStack.tsx
└── assets/
```

#### Folder Structure Details

- **features/**: Feature-based modules
  - Each feature contains its own components, screens, and services
  - Isolated business logic per feature
- **shared/**: Reusable components and utilities
  - Common UI components
  - Custom hooks
  - Helper functions
  - Type definitions
- **database/**: Database related files
- **navigation/**: Route configurations
- **assets/**: Images, fonts, and other static files

## 3. Core Features

### 3.1 Pet Management

#### Required Features

- User must register at least one pet
- Pet profile information:
  - Name
  - Species
  - Breed
  - Age
  - Weight
  - Photo

### 3.2 Activity Tracking

#### Daily Activities

- **Feeding Log**
  - Time of feeding
  - Type of food
  - Amount of food
- **Potty Log**
  - Time
  - Type (Number 1/Number 2)
  - Location (Indoor/Outdoor)
  - Condition (for health tracking)
  - Notes
- **Vaccination Records**
  - Vaccine name
  - Date administered
  - Next due date
  - Veterinarian notes

### 3.3 Reminder System

#### Alarm Features

- **Configurable Reminders for:**
  - Feeding times
  - Potty breaks
  - Medication schedules
  - Vaccination due dates
- **Notification System**
  - Push notifications
  - Customizable alert sounds
  - Snooze functionality

## 4. Database Schema

### Tables

```prisma
datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

model UserProfile {
  id        Int      @id @default(autoincrement())
  firstName String
  lastName  String
  email     String
  pets      Pet[]
  createdAt DateTime @default(now())
}

model Pet {
  id          Int       @id @default(autoincrement())
  userId      Int
  name        String
  species     String
  breed       String?
  age         Int
  weight      Float
  createdAt   DateTime  @default(now())
  activities  Activity[]
  reminders   Reminder[]
  user        UserProfile @relation(fields: [userId], references: [id])
}

model Activity {
  id          Int       @id @default(autoincrement())
  petId       Int
  type        String    // FEEDING, POTTY, VACCINATION
  subType     String?   // For POTTY: NUMBER_1, NUMBER_2
  location    String?   // For POTTY: INDOOR, OUTDOOR
  condition   String?   // For health monitoring
  timestamp   DateTime
  notes       String?
  pet         Pet       @relation(fields: [petId], references: [id])
}

model Reminder {
  id          Int       @id @default(autoincrement())
  petId       Int
  type        String
  time        DateTime
  isActive    Boolean   @default(true)
  pet         Pet       @relation(fields: [petId], references: [id])
}
```

## 5. User Interface Requirements

### 5.1 Main Screens

1. **Dashboard**

   - User profile summary (name, email)
   - Pet Cards Grid/List View
     - Each pet card contains:
       - Pet photo
       - Pet name and basic info
       - Last potty time
       - Next feeding time
       - Upcoming vaccination date
       - Quick action buttons for logging activities
     - Activity Timeline for each pet
       - Today's activities
       - Upcoming scheduled events
     - Color-coded status indicators for:
       - Feeding status
       - Relief schedule
       - Vaccination due dates
   - Card actions:
     - Tap to expand details
     - Quick-log buttons
     - Direct access to reminders

2. **Activity Logger**

   - Easy-to-use input forms
   - Activity history view
   - Calendar integration

3. **Reminder Management**
   - Reminder creation/editing
   - Schedule overview
   - Notification settings

## 6. Technical Requirements

### 6.1 Local Data Storage

- SQLite database file management
- Regular automatic backups of .db file
- Data export functionality
- Simple database migration handling

### 6.2 Notifications

- Background task support for alarms
- Custom notification sounds
- Configurable notification preferences

## 7. Future Considerations

- Cloud sync capabilities
- Multi-pet household support
- Social features for pet owners
- Veterinary appointment scheduling
- Medical record attachments

## 8. Success Metrics

- Daily Active Users (DAU)
- Activity logging frequency
- Reminder completion rate
- User retention rate
