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
    - japanese string
    - romaji string
    - english string
    - parts JSON
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
- study_activities - a specific study activity, linking a study session to group
    - id integer
    - study_session_id integer
    - group_id integer
    - created_at datetime
- word_review_items - a record of word practice, determining if the word was correct or not
    - id integer
    - word_id integer
    - study_session_id integer
    - correct boolean
    - created_at datetime

## API Endpoints

We'll need the following endpoints to power this page

- GET /api/words
    - Pagination with 100 per page
- GET /api/words/:id
- GET /api/groups
    - Pagination with 100 per page
- GET /api/groups/:id
- GET /api/groups/:id/words
- GET /api/dashboard/last_study_session
- GET /api/dashboard/study_progress
- GET /api/dashboard/quick_stats
- GET /api/study_activities
- GET /api/study_activities/:id
- GET /api/study_activities/:id/study_sessions
- POST /api/study_activities
    - required params: group_id, study_activity_id
- GET /api/groups/:id/study_sessions
- GET /api/study_sessions
    - Pagination with 100 per page
- GET /api/study_sessions/:id
- GET /api/study_sessions/:id/words
- POST /api/reset_history
- POST /api/full_reset
- POST /api/study_sessions/:id/words/:word_id/review
    - required params: correct

---

### 1. GET /api/words
**Description:**  Returns a paginated list of vocabulary words.

**Request Parameters:**
- `page` (optional, integer): Page number (default: 1)
- `per_page` (optional, integer): Items per page (default: 100, max: 100)

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "japanese": "猫",
      "romaji": "neko",
      "english": "cat",
      "parts": { "part_of_speech": "noun" },
      "correct_count": 5,
      "wrong_count": 2
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 500,
    "items_per_page": 100
  }
}
```

---

### 2. GET /api/words/:id
**Description:**  Returns a single vocabulary word by ID.

**Response:**
```json
{
  "id": 1,
  "japanese": "猫",
  "romaji": "neko",
  "english": "cat",
  "parts": { "part_of_speech": "noun" },
  "stats": {
    "correct_count": 5,
    "wrong_count": 2
  },
  "groups": [
    {
      "id": 1,
      "name": "Basic Greetings"
    }
  ]
}
```

---

### 3. GET /api/groups
**Description:**  Returns a paginated list of word groups.

**Request Parameters:**
- `page` (optional, integer): Page number (default: 1)
- `per_page` (optional, integer): Items per page (default: 100, max: 100)

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "name": "Basic Greetings",
      "word_count": 20
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 1,
    "total_items": 10,
    "items_per_page": 100
  }
}
```

---

### 4. GET /api/groups/:id
**Description:**  Returns a single group by ID.

**Response:**
```json
{
  "id": 1,
  "name": "Basic Greetings",
  "stats": {
    "total_word_count": 20
  }
}
```

---

### 5. GET /api/groups/:id/words
**Description:**  Returns all words in a specific group.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "japanese": "猫",
      "romaji": "neko",
      "english": "cat",
      "parts": { "part_of_speech": "noun" },
      "correct_count": 5,
      "wrong_count": 2
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 1,
    "total_items": 20,
    "items_per_page": 100
  }
}
```

---

### 6. GET /api/dashboard/last_study_session
**Description:**  Returns information about the most recent study session.

**Response:**
```json
{
  "id": 1,
  "group_id": 1,
  "created_at": "2024-06-01T12:34:56Z",
  "study_activity_id": 1,
  "group": {
    "id": 1,
    "name": "Basic Greetings"
  }
}
```

---

### 7. GET /api/dashboard/study_progress
**Description:**  Returns overall study progress statistics.

**Response:**
```json
{
  "total_words_studied": 120,
  "total_available_words": 200
}
```

---

### 8. GET /api/dashboard/quick_stats
**Description:**  Returns quick statistics for the dashboard.

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
**Description:**  Returns a list of all study activities.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "study_session_id": 1,
      "group_id": 1,
      "created_at": "2024-06-01T12:34:56Z"
    }
  ]
}
```

---

### 10. GET /api/study_activities/:id
**Description:**  Returns a single study activity by ID.

**Response:**
```json
{
  "id": 1,
  "name": "Vocabulary Quiz",
  "thumbnail_url": "https://example.com/thumbnail.jpg",
  "description": "Practice your vocabulary with flashcards"
}
```

---

### 11. GET /api/study_activities/:id/study_sessions
**Description:**  Returns all study sessions for a given study activity.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
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
**Description:**  Creates a new study activity.

**Request Body:**
```json
{
  "group_id": 1,
  "study_activity_id": 1
}
```

**Response:**
```json
{
  "id": 1,
  "group_id": 1
}
```

---

### 13. GET /api/groups/:id/study_sessions
**Description:**  Returns all study sessions for a given group.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
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

### 14. GET /api/study_sessions
**Description:**  Returns a paginated list of all study sessions.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
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

### 15. GET /api/study_sessions/:id
**Description:**  Returns a single study session by ID.

**Response:**
```json
{
  "id": 1,
  "activity_name": "Vocabulary Quiz",
  "group_name": "Basic Greetings",
  "start_time": "2024-06-01T12:34:56Z",
  "end_time": "2024-06-01T14:34:56Z",
  "review_item_count": 20
}
```

---

### 16. GET /api/study_sessions/:id/words
**Description:**  Returns all word review items for a given study session.

**Response:**
```json
{
  "items": [
    {
      "id": 1,
      "japanese": "猫",
      "romaji": "neko",
      "english": "cat",
      "parts": { "part_of_speech": "noun" },
      "correct_count": 5,
      "wrong_count": 2
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 1,
    "total_items": 20,
    "items_per_page": 100
  }
}
```

---

### 17. POST /api/reset_history
**Description:**  Resets the user's study history (but not the vocabulary/groups).

**Response:**
```json
{
  "success": true,
  "message": "Study history has been reset."
}
```

---

### 18. POST /api/full_reset
**Description:**  Resets all data, including vocabulary, groups, and study history.

**Response:**
```json
{
  "success": true,
  "message": "System has been fully reset."
}
```

---

### 19. POST /api/study_sessions/:id/words/:word_id/review
**Description:**  Records a review for a word in a study session.

**Request Params:**
- id (study_session_id) integer
- word_id integer
- correct boolean

**Request Payload:**
```json
{
  "correct": true
}
```

**Response:**
```json
{
  "success": true,
  "word_id": 1,
  "study_session_id": 1,
  "correct": true,
  "created_at": "2024-06-01T12:40:00Z"
}
```

