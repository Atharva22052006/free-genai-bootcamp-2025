## Backend Server Technical Specs

## Business Goal: 
A language learning school wants to build a prototype of learning portal which will act as three things:
- Inventory of possible vocabulary that can be learned
- Act as a  Learning record store (LRS), providing correct and wrong score on practice vocabulary
- A unified launchpad to launch different learning apps

## Technical Requirements

- The Backend will be built using Go
- The Database will be SQLite3
- The API will be built using Gin
- The API will always return JSON
- There will be no authentication or authorization
- Everything will be treated as single user



## Database Schema

We have the following Tables:

- words - stored vocabulary words
    - id integer
    - Japanese Word string
    - Romanji string
    - English string
    - Parts JSON

- word_groups - Joint Table for words and groups many-to-many
    - id integer
    - word_id integer
    - group_id integer
    
- groups - thematic groups of words
    - id integer
    - name string

- study_sessions - records of study sessions grouping word_review_items
    - id integer
    - group_id integer
    - created_at datetime
    - study_activity_id integer
    
- study_activities - a specific study activity, linking a study session to a group
    - id integer
    - study_session_id integer
    - group_id integer
    - created_at datetime


- word_review_items - a record of word practice, determining if the word was correct or not
    - id integer
    - word_id integer
    - study_sessions_id integer
    - correct boolean
    - created_at datetime

## API Endpoints

We'll need the following endpoints to power this page


- GET/api/words
    - Pagination with 100 per page
- GET/api/words/:id
- GET/api/groups
    - Pagination with 100 per page
- GET/api/groups/:id
- GET/api/groups/:id/words
- GET/api/dashboard/last_study_session
    - Pagination with 100 per page
- GET/api/dashboard/study_progress
- GET/api/dashboard/quick_stats
- GET/api/study_activities
- GET/api/study_activities/:id
- GET/api/study_activities/:id/study_sessions 
- POST/api/study_activities
    - required params: group_id, study_activity_id
- GET/api/groups/:id/study_sessions
- GET/api/study_sessions
    - Pagination with 100 per page
- GET/api/study_sessions/:id
- GET/api/study_sessions/:id/words
- POST/api/reset_history
- POST/api/full_reset
- POST/api/study_sessions/:id/words/:word_id/review
    - required params: correct



### 1. GET /api/words

**Description:**  
Returns a paginated list of vocabulary words.

**Request Parameters:**
- `page` (optional, integer): Page number (default: 1)
- `per_page` (optional, integer): Items per page (default: 100, max: 100)

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "japanese_word": "猫",
      "romanji": "neko",
      "english": "cat",
      "parts": { "part_of_speech": "noun" },
      "correct_count": 5,
      "wrong_count": 2
    }
    // ... up to 100 items
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 500,
    "items_per_page": 100,
  }
}
```

---

### 2. GET /api/words/:id

**Description:**  
Returns a single vocabulary word by ID.

**Response:**
```json
{
  "japanese_word": "猫",
  "romanji": "neko",
  "english": "cat",
  "parts": { "part_of_speech": "noun" },
  "stats": {
    "correct_count": 5,
    "wrong_count": 5
  },
  "groups":{
    "id": 123,
    "name": "Basic Greetings"
  }
}
```

---

### 3. GET /api/groups

**Description:**  
Returns a paginated list of word groups.

**Request Parameters:**
- `page` (optional, integer): Page number (default: 1)
- `per_page` (optional, integer): Items per page (default: 100, max: 100)

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "name": "Animals",
      "word_count": 20
    }
    // ... up to 100 items
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 1,
    "items_per_page": 100,
    "total_items": 25
  }
}
```

---

### 4. GET /api/groups/:id

**Description:**  
Returns a single group by ID.

**Response:**
```json
{
  "id": 1,
  "name": "Animals",
  "stats": {
    "total_word_count": 20
  }
}
```

---

### 5. GET /api/groups/:id/words

**Description:**  
Returns all words in a specific group.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "japanese_word": "猫",
      "romanji": "neko",
      "english": "cat",
      "parts": { "part_of_speech": "noun" }
    }
    // ...
  ]
}
```

---


### 6. GET /api/dashboard/last_study_session

**Description:**  
Returns information about the most recent study session

**Response:**
```json
{
  "id": 42,
  "group_id": 3,
  "created_at": "2024-06-01T12:34:56Z",
  "study_activity_id": 7
  "group":{
    "id": 456,
    "name": "Basic Greetings"
  }
}
```

---

### 7. GET /api/dashboard/study_progress

**Description:**  
Returns overall study progress statistics.
Please note that the frontend will determine progress bar based on total words studied and total available words.


**Response:**
```json
{

  "total_words_studied": 120,
  "total_available_words": 200,
}
```

---

### 8. GET /api/dashboard/quick_stats

**Description:**  
Returns quick statistics for the dashboard.

**Response:**
```json
{
  "words_learned": 80,
  "words_remaining": 40,
  "groups_completed": 2,
  "groups_total": 5
}
```

---

### 9. GET /api/study_activities

**Description:**  
Returns a list of all study activities.

**Response:**
```json
{
  "study_activities": [
    {
      "id": 7,
      "study_session_id": 42,
      "group_id": 3,
      "created_at": "2024-06-01T12:34:56Z"
    },
    // ...
  ]
}
```

---

### 10. GET /api/study_activities/:id

**Description:**  
Returns a single study activity by ID.

**Response:**
```json
{
  "id": 7,
  "name": "Vocabulary Quiz",
  "thumbnail url": "https://example,com/thumbnail.jpg",
  "Description": "Practice your Vocabulary with with Flashcards"

}
```

---

### 11. GET /api/study_activities/:id/study_sessions

**Description:**  
Returns all study sessions for a given study activity.

**Response:**
```json
{
  "items": [
    {
      "id": 7,
      "activity_name": "Vocabulary Quiz",
      "group_name": "Basic Greetings",
      "start_time": "2024-06-01T12:34:56Z",
      "end_time": "2024-06-01T14:51:04Z",
      "review_item_count": 20
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 100,
    "items_per_page": 20
  }
}
```

---

### 12. POST /api/study_activities

**Description:**  
Creates a new study activity.

**Request Body:**
```json
{
  "group_id": 3,
  "study_activity_id": 7
}
```

**Response:**
```json
{
  "id": 8,
  "group_id": 3
} 
```

---

### 13. GET /api/groups/:id/study_sessions

**Description:**  
Returns all study sessions for a given group.

**Response:**
```json
{
  "group": {
    "id": 3,
    "name": "Food"
  },
  "study_sessions": [
    {
      "id": 42,
      "group_id": 3,
      "created_at": "2024-06-01T12:34:56Z",
      "study_activity_id": 7
    },
    // ...
  ]
}
```

---

### 14. GET /api/study_sessions

**Description:**  
Returns a paginated list of all study sessions.

**Request Parameters:**
- `page` (optional, integer): Page number (default: 1)
- `per_page` (optional, integer): Items per page (default: 100, max: 100)

**Response:**
```json
{
  "study_sessions": [
    {
      "id": 42,
      "group_id": 3,
      "created_at": "2024-06-01T12:34:56Z",
      "study_activity_id": 7
    } 
    // ... up to 100 items
  ],
  "pagination": {
    "page": 1,
    "per_page": 100,
    "total": 250
  }
}
```

---

### 15. GET /api/study_sessions/:id

**Description:**  
Returns a single study session by ID.

**Response:**
```json
{
  "id": 42,
  "group_id": 3,
  "created_at": "2024-06-01T12:34:56Z",
  "study_activity_id": 7
}
```

---

### 16. GET /api/study_sessions/:id/words

**Description:**  
Returns all word review items for a given study session.

**Response:**
```json
{
  "study_session": {
    "id": 42,
    "group_id": 3,
    "created_at": "2024-06-01T12:34:56Z",
    "study_activity_id": 7
  },
  "word_review_items": [
    {
      "id": 101,
      "word_id": 1,
      "study_sessions_id": 42,
      "correct": true,
      "created_at": "2024-06-01T12:35:00Z"
    },
    {
      "id": 102,
      "word_id": 2,
      "study_sessions_id": 42,
      "correct": false,
      "created_at": "2024-06-01T12:36:00Z"
    }
    // ...
  ]
}
```

---

### 17. POST /api/reset_history

**Description:**  
Resets the user's study history (but not the vocabulary/groups).

**Response:**
```json
{
  "success": true,
  "message": "Study history has been reset."
}
```

---

### 18. POST /api/full_reset

**Description:**  
Resets all data, including vocabulary, groups, and study history.

**Response:**
```json
{
  "success": true,
  "message": "All data has been reset."
}
```

---

### 19. POST /api/study_sessions/:id/words/:word_id/review

**Description:**  
Records a review for a word in a study session.

**Request Body:**
```json
{
  "correct": true
}
```

**Response:**
```json
{
  "id": 201,
  "word_id": 1,
  "study_sessions_id": 42,
  "correct": true,
  "created_at": "2024-06-01T12:40:00Z"
}
```

---

