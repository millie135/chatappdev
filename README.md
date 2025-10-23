This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.




<!-- rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users collection
    match /users/{userId} {
      // Allow read if logged in
      allow read: if request.auth != null;
      // Allow write only for the authenticated user creating/updating their own doc
      allow write: if request.auth != null && request.auth.uid == userId;
    }

    // Chats collection
    match /chats/{userId}/{chatWithUserId}/{messageId} {
      allow read, write: if request.auth != null
                         && (request.auth.uid == userId || request.auth.uid == chatWithUserId);
    }
  }
} -->



<!-- real time database -->
<!-- 
{
  "rules": {
    "status": {
      "$uid": {
        ".read": "auth != null",          // signed-in users can read any status
        ".write": "auth != null && auth.uid === $uid" // can write own status
      }
    }
  }
} -->


<!-- rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users
    match /users/{userId} {
      allow read, write: if request.auth != null;
    }

    // Groups
    match /groups/{groupId} {
      allow read, write: if request.auth != null;
    }

    // 1-on-1 chats
    match /chats/{userId}/{chatId}/{messageId} {
      allow read, write: if request.auth != null;
    }

    // Group chat messages
    match /groupChats/{groupId}/messages/{messageId} {
      allow read, write: if request.auth != null;
    }
  }
} -->



<!-- // Groups
    match /groups/{groupId} {
      allow read, write: if request.auth != null;
    }

    // 1-on-1 chats
    match /chats/{userId}/{chatId}/{messageId} {
      allow read, write: if request.auth != null;
    }

    // Group chat messages
    match /groupChats/{groupId}/messages/{messageId} {
      allow read, write: if request.auth != null;
    } 

    // Users
    match /users/{userId} {
      allow read, write: if request.auth != null;
    }
    
    
    -->



<!-- 
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Users
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }

    // Groups
    match /groups/{groupId} {
      allow read: if request.auth != null && (
        request.auth.uid in resource.data.members || 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"
      );

      allow create: if request.auth != null &&
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";

      allow update: if request.auth != null &&
        (request.auth.uid in resource.data.members ||
         get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader");
    }

    // 1-on-1 Chats
    match /chats/{userId}/{chatId}/{messageId} {
      allow read, write: if request.auth != null && (request.auth.uid == userId || request.auth.uid == chatId);
    }

    // Group Chats
    match /groupChats/{groupId}/messages/{messageId} {
      allow read, write: if request.auth != null &&
        exists(/databases/$(database)/documents/groups/$(groupId)) &&
        request.auth.uid in get(/databases/$(database)/documents/groups/$(groupId)).data.members;
    }
  }
} -->



<!-- rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // -----------------
    // Users Collection
    // -----------------
    match /users/{userId} {
      // Anyone logged in can read basic info
      allow read: if request.auth != null;

      // Users can write only their own document
      allow write: if request.auth != null && request.auth.uid == userId;
    }

    // -----------------
    // Groups Collection
    // -----------------
    match /groups/{groupId} {

      // Read rules
      allow read: if request.auth != null &&
                  (
                    request.auth.uid in resource.data.members ||       // Members can read their groups
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"  // Leaders can read all
                  );

      // Create rules
      allow create: if request.auth != null &&
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";

      // Update rules
      allow update: if request.auth != null &&
                    (
                      request.auth.uid in resource.data.members || 
                      get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"
                    );

      // Delete rules
      allow delete: if request.auth != null &&
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";
    }

    // -----------------
    // Chat Messages (one-to-one)
    // -----------------
    match /chats/{userId}/{otherUserId}/{messageId} {
      allow read, write: if request.auth != null
                        && (request.auth.uid == userId || request.auth.uid == otherUserId);
    }

    // -----------------
    // Group Chat Messages
    // -----------------
    match /groupChats/{groupId}/messages/{messageId} {
      allow read, write: if request.auth != null
                        && (
                             request.auth.uid in get(/databases/$(database)/documents/groups/$(groupId)).data.members ||
                             get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"
                           );
    }
  }
} -->



<!-- rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // -----------------
    // Users Collection
    // -----------------
    match /users/{userId} {
      // Anyone logged in can read basic info
      allow read: if request.auth != null;

      // Users can write only their own document
      allow write: if request.auth != null && request.auth.uid == userId;
    }

    // -----------------
    // Groups Collection
    // -----------------
    match /groups/{groupId} {

      // Read rules
      allow read: if request.auth != null &&
                  (
                    request.auth.uid in resource.data.members ||       // Members can read their groups
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"  // Leaders can read all
                  );

      // Create rules
      allow create: if request.auth != null &&
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";

      // Update rules
      allow update: if request.auth != null &&
                    (
                      request.auth.uid in resource.data.members || 
                      get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"
                    );

      // Delete rules
      allow delete: if request.auth != null &&
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";
    }

    // -----------------
    // One-to-One Chats
    // -----------------
    match /chats/{userId}/{otherUserId}/{messageId} {
      allow read, write: if request.auth != null
                         && (request.auth.uid == userId || request.auth.uid == otherUserId);
    }

    // -----------------
    // Group Chats
    // -----------------
    match /groupChats/{groupId}/messages/{messageId} {
      allow read, write: if request.auth != null
                         && (request.auth.uid in get(/databases/$(database)/documents/groups/$(groupId)).data.members
                             || get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader");
    }

    // -----------------
    // Time Logs
    // -----------------
    match /timeLogs/{userId}/logs/{logId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      allow read: if request.auth != null && get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";
    }
  }
} -->



<!-- rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // -----------------
    // Users Collection
    // -----------------
    match /users/{userId} {
      // Anyone logged in can read basic info
      allow read: if request.auth != null;

      // Users can write only their own document
      allow write: if request.auth != null && request.auth.uid == userId;
    }

    // -----------------
    // Groups Collection
    // -----------------
    match /groups/{groupId} {

      // Read rules
      allow read: if request.auth != null &&
                  (
                    resource.data.members != null && request.auth.uid in resource.data.members ||       // Members can read their groups
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"  // Leaders can read all
                  );

      // Create rules
      allow create: if request.auth != null &&
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";

      // Update rules
      allow update: if request.auth != null &&
                    (
                      resource.data.members != null && request.auth.uid in resource.data.members || 
                      get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader"
                    );

      // Delete rules
      allow delete: if request.auth != null &&
                    get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";
    }

    // -----------------
    // One-to-One Chats
    // -----------------
    // match /chats/{userId}/{otherUserId}/{messageId} {
    //   allow read, write: if request.auth != null
    //                      && (request.auth.uid == userId || request.auth.uid == otherUserId);
    // }
    
    match /chats/{userId}/{otherUserId}/{messageId} {
      allow read, write: if request.auth != null && (
        // must be one of the two users
        (request.auth.uid == userId || request.auth.uid == otherUserId)
        &&
        // one must be a Leader if the current user is a User
        (
          // if current user is Leader → allow chatting with anyone
          get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader" ||

          // if current user is User → other user must be Leader
          (
            get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "User" &&
            get(/databases/$(database)/documents/users/$(otherUserId)).data.role == "Leader"
          )
        )
      );
    }

    // -----------------
    // Group Chats
    // -----------------
    match /groupChats/{groupId}/messages/{messageId} {
      allow read, write: if request.auth != null
                         && ( resource.data.members != null && request.auth.uid in get(/databases/$(database)/documents/groups/$(groupId)).data.members
                             || get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader");
    }

    // -----------------
    // Time Logs
    // -----------------
    match /timeLogs/{userId}/logs/{logId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      allow read: if request.auth != null && get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "Leader";
    }
  }
} -->