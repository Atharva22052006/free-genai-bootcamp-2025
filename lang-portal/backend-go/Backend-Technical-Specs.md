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


