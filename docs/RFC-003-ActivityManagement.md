# RFC 003: Pet Activity Management System

## Overview

This RFC proposes the implementation of the pet activity tracking system, enabling users to log and monitor their pets' daily activities including feeding, potty, and vaccinations.

## Background

Activity tracking is a core feature of PuppyLog, allowing users to maintain detailed records of their pets' daily routines and health-related activities.

## Goals

- Track pet feeding schedule and details
- Monitor potty activities
- Record vaccination history
- Provide activity history and analytics
- Enable quick logging of common activities

## Non-Goals

- Real-time pet monitoring
- Automated activity detection
- Third-party integrations
- Veterinary record synchronization

## Detailed Design

### Data Structures

```typescript
enum ActivityType {
  FEEDING = 'FEEDING',
  POTTY = 'POTTY',
  VACCINATION = 'VACCINATION',
}

enum PottyType {
  NUMBER_1 = 'NUMBER_1',
  NUMBER_2 = 'NUMBER_2',
}

interface ActivityBase {
  id: number;
  petId: number;
  timestamp: Date;
  notes?: string;
}

interface FeedingActivity extends ActivityBase {
  type: ActivityType.FEEDING;
  foodType: string;
  amount: number;
  unit: 'g' | 'ml' | 'piece';
}

interface PottyActivity extends ActivityBase {
  type: ActivityType.POTTY;
  pottyType: PottyType;
  location: 'INDOOR' | 'OUTDOOR';
  condition?: string;
}

interface VaccinationActivity extends ActivityBase {
  type: ActivityType.VACCINATION;
  vaccineName: string;
  nextDueDate: Date;
  veterinarian?: string;
}
```

### Activity Management Service

```typescript
interface IActivityManagementService {
  logActivity(activity: ActivityBase): Promise<ActivityBase>;
  getActivities(petId: number, type?: ActivityType): Promise<ActivityBase[]>;
  getRecentActivities(petId: number, limit: number): Promise<ActivityBase[]>;
  updateActivity(id: number, data: Partial<ActivityBase>): Promise<ActivityBase>;
  deleteActivity(id: number): Promise<void>;
  getActivityStats(petId: number, type: ActivityType): Promise<ActivityStats>;
}
```

### UI Components

1. **Quick Action Buttons**

   - Feeding log
   - Potty log
   - Vaccination log

2. **Activity Forms**

   - Type-specific input fields
   - Time picker
   - Notes field
   - Location selector (for potty)

3. **Activity History**
   - Filterable list/timeline
   - Calendar view
   - Statistics dashboard

## File Structure

```
src/
└── features/
    └── activity/
        ├── components/
        │   ├── QuickActionBar.tsx
        │   ├── ActivityForm.tsx
        │   ├── ActivityList.tsx
        │   └── ActivityCalendar.tsx
        ├── screens/
        │   ├── ActivityDashboard.tsx
        │   ├── AddActivityScreen.tsx
        │   └── ActivityDetailScreen.tsx
        ├── services/
        │   └── activityManagement.service.ts
        └── types/
            └── activity.types.ts
```

## Implementation Steps

1. Create activity database schemas
2. Implement activity service
3. Create quick action components
4. Build activity forms
5. Develop history views
6. Add statistics tracking
7. Implement filters and search
8. Add data validation
9. Create unit tests

## Testing Plan

1. Unit Tests

   - Activity logging
   - Data validation
   - Statistics calculation

2. Integration Tests

   - Activity workflow
   - History tracking
   - Data persistence

3. UI Tests
   - Form submissions
   - Quick actions
   - History views
   - Filters and search

## Success Metrics

- Activity logging frequency
- Time to log activity
- Activity types distribution
- User engagement with history
- Error rate in logging

## Timeline

- Design Review: 1 day
- Core Implementation: 4-5 days
- UI Development: 3-4 days
- Testing: 2-3 days
- Documentation: 1 day
- Total: 11-14 days

## Open Questions

1. Should we add activity templates?
2. Do we need activity categories beyond the basic types?
3. How long should we retain activity history?
4. Should we add batch activity logging?
5. Do we need export functionality for activity data?
