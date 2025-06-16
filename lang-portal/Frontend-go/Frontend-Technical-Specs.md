# Frontend Techincal Specs

## Pages

### Dashboard/dashboard

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

- GET/words
- GET/words/:id
- GET/groups
- GET/groups/:id
- GET/groups/:id/words
- GET/dashboard/last_study_session
- GET/dashboard/study_progress
- GET/dashboard/quick-stats


### Study Activities/study-activities

#### Purpose
The purpose of this page is to show a collection of study actiivities with a thumbnailand its name, to either launch or view the study activity. 

#### Components

- Study Activity Card
    - Shows a Thumbnail of the study activity
    - The name of the study activity
    - a launch button to take us to the launch page
    - a view button to take us to view page which has more information about past study sessions for this study activity

#### Needed API Endpoints

- GET/study_activities
    - pagination

