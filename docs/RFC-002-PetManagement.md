# RFC 002: Pet Management System

## Overview

This RFC proposes the implementation of the pet management system, allowing users to add, edit, and manage their pets' profiles in the PuppyLog application.

## Background

Each user must have at least one pet registered in the system. The pet management system is core to the application as all activities, reminders, and tracking features are associated with specific pets.

## Goals

- Enable users to register pets
- Maintain pet profiles with detailed information
- Support multiple pets per user
- Provide pet photo management
- Enable basic health information tracking

## Non-Goals

- Breed-specific feature customization
- Pet social networking
- Pet marketplace features
- Veterinary clinic integration

## Detailed Design

### Data Structure

```typescript
interface Pet {
  id: number;
  userId: number;
  name: string;
  species: string;
  breed: string | null;
  age: number;
  weight: number;
  photo: string | null;
  createdAt: Date;
  lastCheckup?: Date;
}

enum PetSpecies {
  DOG = 'DOG',
  CAT = 'CAT',
  BIRD = 'BIRD',
  OTHER = 'OTHER',
}
```

### Pet Management Flow

1. Pet Registration

   - Required during first app launch
   - Available anytime through dashboard
   - Photo upload optional but recommended

2. Pet Profile Management
   - View pet details
   - Edit information
   - Upload/update photos
   - Archive/remove pets

### Storage Implementation

```typescript
interface IPetManagementService {
  addPet(petData: Omit<Pet, 'id' | 'createdAt'>): Promise<Pet>;
  updatePet(id: number, petData: Partial<Pet>): Promise<Pet>;
  getPet(id: number): Promise<Pet>;
  getAllPets(userId: number): Promise<Pet[]>;
  archivePet(id: number): Promise<void>;
  uploadPetPhoto(id: number, photo: File): Promise<string>;
}
```

### UI Components

1. **Pet Registration Form**

   - Basic information inputs
   - Photo upload
   - Breed selection (based on species)
   - Age and weight inputs

2. **Pet Profile View**

   - Photo gallery
   - Information display
   - Quick action buttons
   - Activity summary

3. **Pet List Component**
   - Grid/List view toggle
   - Sort and filter options
   - Quick action buttons
   - Status indicators

### Error Handling

- Photo upload validation
- Required field validation
- Age/weight range validation
- Duplicate pet name detection

## File Structure

```
src/
└── features/
    └── pet/
        ├── components/
        │   ├── PetForm.tsx
        │   ├── PetCard.tsx
        │   ├── PetList.tsx
        │   └── PetPhotoUploader.tsx
        ├── screens/
        │   ├── PetRegistrationScreen.tsx
        │   ├── PetProfileScreen.tsx
        │   └── PetEditScreen.tsx
        ├── services/
        │   └── petManagement.service.ts
        └── types/
            └── pet.types.ts
```

## Implementation Steps

1. Create pet database schema
2. Implement PetManagementService
3. Create pet registration form
4. Add photo upload functionality
5. Implement pet list view
6. Create pet profile screen
7. Add edit capabilities
8. Implement validation
9. Add error handling
10. Create unit tests

## Testing Plan

1. Unit Tests

   - Service methods
   - Form validation
   - Photo upload
   - Data operations

2. Integration Tests

   - Pet registration flow
   - Profile updates
   - Photo management
   - List operations

3. UI Tests
   - Form interactions
   - Photo upload process
   - List view interactions
   - Profile view rendering

## Success Metrics

- Pet registration completion rate
- Photo upload rate
- Profile completion percentage
- Profile update frequency
- Average pets per user

## Timeline

- Design Review: 1 day
- Core Implementation: 3-4 days
- Photo Management: 2 days
- Testing: 2 days
- Documentation: 1 day
- Total: 8-10 days

## Open Questions

1. Should we limit the number of pets per user?
2. Do we need breed-specific features?
3. Should we add pet medical history tracking?
4. How should we handle pet archiving vs deletion?
