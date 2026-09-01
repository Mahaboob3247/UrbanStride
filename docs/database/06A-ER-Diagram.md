# UrbanStride ER Diagram

**Version:** 1.0  
**Status:** Draft

```mermaid
erDiagram
    USERS {
        uuid id PK
        varchar display_name
        varchar username UK
        varchar email UK
        varchar phone_number UK
        varchar city
        varchar profile_photo_url
        varchar account_status
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    USER_AUTH_IDENTITIES {
        uuid id PK
        uuid user_id FK
        varchar provider
        varchar provider_user_id
        timestamptz created_at
    }

    USER_DEVICES {
        uuid id PK
        uuid user_id FK
        varchar platform
        varchar push_token
        boolean is_active
        timestamptz last_seen_at
    }

    TRUSTED_CONTACTS {
        uuid id PK
        uuid user_id FK
        varchar name
        varchar phone_number
        varchar relationship
        boolean is_primary
        timestamptz created_at
    }

    ACTIVITIES {
        uuid id PK
        uuid user_id FK
        varchar activity_type
        varchar title
        varchar status
        decimal distance_meters
        integer duration_seconds
        decimal average_speed_mps
        decimal calories
        varchar visibility
        geography route_path
        timestamptz started_at
        timestamptz ended_at
        timestamptz created_at
    }

    ACTIVITY_POINTS {
        bigint id PK
        uuid activity_id FK
        integer sequence_number
        geography location
        decimal altitude_meters
        decimal accuracy_meters
        decimal speed_mps
        timestamptz recorded_at
    }

    ROUTES {
        uuid id PK
        uuid created_by FK
        varchar name
        varchar city
        varchar difficulty
        decimal distance_meters
        geography route_path
        decimal safety_score
        varchar moderation_status
        timestamptz created_at
    }

    ROUTE_SAFETY_REPORTS {
        uuid id PK
        uuid route_id FK
        uuid reported_by FK
        varchar category
        integer severity
        text description
        geography location
        varchar verification_status
        timestamptz reported_at
    }

    ROUTE_AMENITIES {
        uuid id PK
        uuid route_id FK
        varchar amenity_type
        varchar name
        geography location
        varchar verification_status
    }

    USER_FOLLOWS {
        uuid follower_id PK, FK
        uuid following_id PK, FK
        varchar status
        timestamptz created_at
    }

    POSTS {
        uuid id PK
        uuid author_id FK
        uuid activity_id FK
        text caption
        varchar visibility
        timestamptz created_at
        timestamptz deleted_at
    }

    POST_LIKES {
        uuid post_id PK, FK
        uuid user_id PK, FK
        timestamptz created_at
    }

    COMMENTS {
        uuid id PK
        uuid post_id FK
        uuid author_id FK
        uuid parent_comment_id FK
        text body
        timestamptz created_at
        timestamptz deleted_at
    }

    COMMUNITIES {
        uuid id PK
        uuid owner_id FK
        varchar name
        text description
        varchar city
        geography location
        varchar visibility
        timestamptz created_at
    }

    COMMUNITY_MEMBERS {
        uuid community_id PK, FK
        uuid user_id PK, FK
        varchar role
        varchar status
        timestamptz joined_at
    }

    EVENTS {
        uuid id PK
        uuid organizer_id FK
        uuid community_id FK
        varchar name
        text description
        geography location
        timestamptz starts_at
        timestamptz ends_at
        decimal entry_fee
        integer capacity
        varchar status
    }

    EVENT_REGISTRATIONS {
        uuid event_id PK, FK
        uuid user_id PK, FK
        varchar status
        timestamptz registered_at
    }

    NOTIFICATIONS {
        uuid id PK
        uuid user_id FK
        varchar type
        varchar title
        text message
        jsonb data
        timestamptz read_at
        timestamptz created_at
    }

    SOS_INCIDENTS {
        uuid id PK
        uuid user_id FK
        uuid activity_id FK
        varchar status
        geography started_location
        timestamptz started_at
        timestamptz resolved_at
    }

    SOS_LOCATION_UPDATES {
        bigint id PK
        uuid sos_incident_id FK
        geography location
        decimal accuracy_meters
        timestamptz recorded_at
    }

    USERS ||--o{ USER_AUTH_IDENTITIES : has
    USERS ||--o{ USER_DEVICES : registers
    USERS ||--o{ TRUSTED_CONTACTS : defines
    USERS ||--o{ ACTIVITIES : records
    ACTIVITIES ||--o{ ACTIVITY_POINTS : contains

    USERS ||--o{ ROUTES : creates
    ROUTES ||--o{ ROUTE_SAFETY_REPORTS : receives
    USERS ||--o{ ROUTE_SAFETY_REPORTS : submits
    ROUTES ||--o{ ROUTE_AMENITIES : contains

    USERS ||--o{ USER_FOLLOWS : follows
    USERS ||--o{ USER_FOLLOWS : followed_by

    USERS ||--o{ POSTS : authors
    ACTIVITIES ||--o| POSTS : generates
    POSTS ||--o{ POST_LIKES : receives
    USERS ||--o{ POST_LIKES : creates
    POSTS ||--o{ COMMENTS : contains
    USERS ||--o{ COMMENTS : authors
    COMMENTS ||--o{ COMMENTS : replies

    USERS ||--o{ COMMUNITIES : owns
    COMMUNITIES ||--o{ COMMUNITY_MEMBERS : contains
    USERS ||--o{ COMMUNITY_MEMBERS : joins

    USERS ||--o{ EVENTS : organizes
    COMMUNITIES ||--o{ EVENTS : hosts
    EVENTS ||--o{ EVENT_REGISTRATIONS : receives
    USERS ||--o{ EVENT_REGISTRATIONS : makes

    USERS ||--o{ NOTIFICATIONS : receives
    USERS ||--o{ SOS_INCIDENTS : triggers
    ACTIVITIES ||--o{ SOS_INCIDENTS : relates_to
    SOS_INCIDENTS ||--o{ SOS_LOCATION