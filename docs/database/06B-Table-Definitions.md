# UrbanStride Table Definitions

**Version:** 1.0  
**Status:** Draft

---

## 1. users

Stores the primary user profile.

Important fields:

```text
id
display_name
username
email
phone_number
bio
city
profile_photo_url
preferred_activity
account_status
created_at
updated_at
deleted_at
```

Rules:

- `username` must be unique when present.
- `email` must be unique when present.
- `phone_number` must be unique when present.
- At least one verified authentication identity is required.
- Personal information must not be publicly exposed by default.

---

## 2. user_auth_identities

Connects users to login providers.

Important fields:

```text
id
user_id
provider
provider_user_id
verified_at
created_at
```

Provider examples:

```text
google
phone
apple
```

Constraint:

```text
UNIQUE(provider, provider_user_id)
```

---

## 3. user_devices

Stores mobile devices and push-notification tokens.

Important fields:

```text
id
user_id
platform
push_token
app_version
is_active
last_seen_at
created_at
updated_at
```

---

## 4. trusted_contacts

Stores contacts selected by a user for emergency communication.

Important fields:

```text
id
user_id
name
phone_number
relationship
is_primary
created_at
updated_at
```

Trusted contacts must never be visible in public profiles.

---

## 5. activities

Stores one completed, paused, or discarded fitness activity.

Important fields:

```text
id
user_id
activity_type
title
description
status
distance_meters
duration_seconds
average_speed_mps
maximum_speed_mps
average_pace_seconds_per_km
calories
elevation_gain_meters
route_path
visibility
started_at
ended_at
created_at
updated_at
deleted_at
```

Activity types:

```text
run
walk
cycle
```

Statuses:

```text
in_progress
paused
completed
discarded
```

---

## 6. activity_points

Stores ordered GPS samples captured during an activity.

Important fields:

```text
id
activity_id
sequence_number
location
altitude_meters
accuracy_meters
speed_mps
heading_degrees
recorded_at
```

Constraint:

```text
UNIQUE(activity_id, sequence_number)
```

GPS points may be retained, reduced, or archived according to the final privacy and storage policy.

---

## 7. routes

Stores published routes available for discovery.

Important fields:

```text
id
created_by
name
description
city
activity_type
difficulty
distance_meters
estimated_duration_seconds
route_path
safety_score
safety_score_updated_at
moderation_status
visibility
created_at
updated_at
deleted_at
```

The safety score may be null when reliable information is unavailable.

---

## 8. route_safety_reports

Stores time-stamped reports submitted about route conditions.

Important fields:

```text
id
route_id
reported_by
category
severity
description
location
verification_status
reported_at
expires_at
```

Categories may include:

```text
lighting
traffic
surface
construction
isolation
accessibility
other
```

Reports require moderation or verification before influencing public route information.

---

## 9. route_amenities

Stores useful facilities near a route.

Amenity types may include:

```text
water
public_toilet
medical
parking
transit
rest_area
```

Important fields:

```text
id
route_id
amenity_type
name
location
verification_status
last_verified_at
```

---

## 10. user_follows

Stores follow relationships between users.

Important fields:

```text
follower_id
following_id
status
created_at
```

Constraint:

```text
follower_id must not equal following_id
```

Composite primary key:

```text
(follower_id, following_id)
```

---

## 11. posts

Stores social-feed posts.

Important fields:

```text
id
author_id
activity_id
caption
visibility
created_at
updated_at
deleted_at
```

An activity can be saved without creating a public post.

---

## 12. post_likes

Stores likes on posts.

Composite primary key:

```text
(post_id, user_id)
```

This prevents duplicate likes from the same user.

---

## 13. comments

Stores comments and replies.

Important fields:

```text
id
post_id
author_id
parent_comment_id
body
created_at
updated_at
deleted_at
```

`parent_comment_id` is optional and supports comment replies.

---

## 14. communities

Stores local fitness groups.

Important fields:

```text
id
owner_id
name
slug
description
city
location
activity_type
visibility
moderation_status
created_at
updated_at
deleted_at
```

---

## 15. community_members

Stores community membership.

Important fields:

```text
community_id
user_id
role
status
joined_at
```

Roles:

```text
owner
admin
moderator
member
```

Composite primary key:

```text
(community_id, user_id)
```

---

## 16. events

Stores fitness events and community meetups.

Important fields:

```text
id
organizer_id
community_id
name
description
activity_type
location_name
location
starts_at
ends_at
registration_deadline
entry_fee
currency
capacity
status
created_at
updated_at
deleted_at
```

Status values:

```text
draft
published
cancelled
completed
```

---

## 17. event_registrations

Stores event registrations.

Important fields:

```text
event_id
user_id
status
registered_at
cancelled_at
```

Composite primary key:

```text
(event_id, user_id)
```

---

## 18. notifications

Stores in-app notification records.

Important fields:

```text
id
user_id
type
title
message
data
read_at
created_at
```

The `data` JSONB field may contain a route to the relevant app screen.

---

## 19. sos_incidents

Stores an emergency-mode session.

Important fields:

```text
id
user_id
activity_id
status
started_location
started_at
resolved_at
resolution_note
created_at
```

Status values:

```text
active
resolved
cancelled
```

This feature must not claim to replace official emergency services.

---

## 20. sos_location_updates

Stores location updates associated with an active SOS incident.

Important fields:

```text
id
sos_incident_id
location
accuracy_meters
recorded_at
```

Access must be tightly restricted and auditable.