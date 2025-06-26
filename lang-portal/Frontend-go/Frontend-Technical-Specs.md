# Frontend Techincal Specs

## Pages

### Dashboard/ `dashboard`

#### Purpose
The purpose of this page is to provide summary of learning and act as the default page when the user visits the web-app(Landing Page)

#### Components
This page contains the following components
- Last study session
    - Shows Last activity used
    - shows when last activity used
    - summarzes wrong vs correct from last activity
    - has a link to the group

- Study Progress
    - total words studied eg. 3/124
        -across all study sessions show the total words studied out of all possible words in our database
    - display a mastery progress bar eg. 0%

- Quick Stats
    - Success rate eg. 80%
    - total study sessions eg. 4
    - total active groups eg. 3
    - study streaks eg. 4 days

- Start Studying Button
    - Goes to study activities page

#### Needed API Endpoints

- GET/api/words
- GET/api/words/:id
- GET/api/groups
- GET/api/groups/:id
- GET/api/groups/:id/words
- GET/api/dashboard/last_study_session
- GET/api/dashboard/study_progress
- GET/api/dashboard/quick_stats


### Study Activities Index `study_activities`

#### Purpose
The purpose of this page is to show a collection of study actiivities with a thumbnailand its name, to either launch or view the study activity. 

#### Components

- Study Activity Card
    - Shows a Thumbnail of the study activity
    - The name of the study activity
    - a launch button to take us to the launch page
    - a view button to take us to view page which has more information about past study sessions for this study activity

#### Needed API Endpoints

- GET/api/study_activities


### Study Activity show `/study_activities/:id`

#### Purpose
The purpose of this page is to show the details of a study activity and its past study sessions

#### Components
- Name of the study activity
- Thumbnail of study activity
- Description of study activity
- Launch button
- Study activities Paginated list
    - id
    - activity_name
    - group name 
    - start time
    - end time(inferred by the last word_review item submitted)
    - Number of review items 


#### Needed Endpoints

- GET/api/study_activities/:id
- GET/api/study_activities/:id/study_sessions 


### Study Activities Launch `/study_activities/:id/launch`

#### Purpose
The purpose of this page is to launch a study activities

## Behaviour

After the form is submitted, a new tab opens with the study activity based on its URL provided in the database. 

Also after submitting form the page will redirect to study session showcase

#### Components
- Name of Study Activity
- Launch Form 
    - select field for group
    - launch now button


#### Needed Endpoints
- POST/api/study_activities


### Words Index `/words`

#### Purpose
The purpose of this page is to shows all words in our database.


#### Components
- Paginated Words list
   - Fields
        - Japanese
        - Romaji
        - English 
        - Correct Count
        - Wrong Count

- Pagination with 100 per page
- Clicking the japanese word will take us to the word page


#### Needed Endpoints
-- GET/api/words

### Word Show `/words/:id`

#### Purpose
The purpose of this page is to show information about a specific word

#### Components
- Japanese
- Romaji
- English
- Study Statistics
    - Correct count
    - Wrong count
- Word Groups 
    - shown as a series of pills eg. tags
    - when group name is clicked it will take us to the group show page


#### Needed Endpoints
- GET/api/words/:id


### Words Groups Index `/groups`

#### Purpose 
The purpose of this page is to show a list of groups in our database

#### Components
- Paginated Group List
    - Columns 
        - Group Name
        - Word Count
    - Clicking the group name will take us to group show page 

#### Needed Endpoints
- GET/api/groups


### Group Show `/groups/:id`

#### Purpose
The purpose of this page is to show information about a specific group.

#### Components
- Group Name 
- Group Statistics 
    - Total Word Count
- Words in Group(Paginated list of words)
    - Should use the same component as the word index page
- Study Sessions(Paginated list of Study Sessions)
    - Should use the same components as study sessions index page

#### Needed Endpoints
- GET/api/groups/:id (The name and group stats)
- GET/api/groups/:id/words
- GET/api/groups/:id/study_sessions

### Study Sessions Index `/study_sessions`


#### Purpose
The purpose of this page is to show a list of study sessions in our database.

#### Components
- Paginated Study Session List
    - Columns 
        - Id
        - Activity Name
        - Group Name 
        - Start Time
        - End Time
        - Number of Review Items
    - Clicking the study sessions Id will take us to study session show page

#### Needed Endpoints
- GET/api/study_sessions


### Study Sessions Show `/study_sessions/:id`

#### Purpose
The Purpose of this page is to show information about a speific study session 

#### Components
- Study Sessions Details
    - Activity Name
    - Group Name 
    - Start Time
    - End Time
    - Numbe of review items 
- Words Review Items (Paginated list of Words)
    - Should use the same components as words index page 

#### Needed Endpoints
- GET/api/study_sessions/:id
- GET/api/study_sessions/:id/words


### Settings Page `/settings`

#### Purpose 
The purpose of this page is to make configurations to the study portal.

#### Components
- Theme Selection eg. Light, Dark, System Default 
- Reset History
    - This will delete all study sessions and word review items 
- Full Reset Button
    - This will drop all tables and re-create with seed data 


#### Needed Endpoints
- POST/api/reset_history
- POST/api/full_reset





