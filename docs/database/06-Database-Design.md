# UrbanStride Database Design

**Version:** 1.0  
**Status:** Draft  
**Database:** PostgreSQL  
**Geospatial Extension:** PostGIS

---

## 1. Purpose

This document defines the initial database architecture for UrbanStride.

The data model supports:

- User authentication and profiles
- Trusted emergency contacts
- Running, walking, and cycling activities
- GPS route recording
- Route discovery
- Route safety information
- Social following
- Activity feed posts
- Likes and comments
- Fitness communities
- Community membership
- Fitness events and registrations
- Notifications
- SOS incidents
- Device tokens

---

## 2. Database Strategy

UrbanStride will use PostgreSQL as the primary relational database.

PostGIS will be used for geospatial data such as:

- GPS coordinates
- Recorded activity paths
- Published fitness routes
- Event locations
- Community locations
- Nearby-route searches

Redis may be added later for:

- Feed caching
- Rate limiting
- Leaderboards
- Temporary live-location state
- OTP expiration
- Frequently accessed route data

Redis is not the source of truth. Persistent data remains in PostgreSQL.

---

## 3. Core Domains

### Identity Domain

Tables:

- users
- user_auth_identities
- user_devices
- trusted_contacts

### Activity Domain

Tables:

- activities
- activity_points
- activity_photos

### Route Domain

Tables:

- routes
- route_safety_reports
- route_amenities

### Social Domain

Tables:

- user_follows
- posts
- post_likes
- comments

### Community Domain

Tables:

- communities
- community_members
- community_events

### Event Domain

Tables:

- events
- event_registrations

### Communication Domain

Tables:

- notifications

### Safety Domain

Tables:

- sos_incidents
- sos_location_updates
- sos_contact_notifications

---

## 4. Relationship Summary

- One user can have multiple authentication identities.
- One user can register multiple devices.
- One user can have multiple trusted contacts.
- One user can record multiple activities.
- One activity can contain multiple GPS points.
- One activity can contain multiple photos.
- One activity can generate one social post.
- One user can follow multiple users.
- One post can have multiple likes.
- One post can have multiple comments.
- One user can create multiple communities.
- One community can have multiple members.
- One community can organize multiple events.
- One user can register for multiple events.
- One route can have multiple safety reports.
- One route can have multiple amenities.
- One user can trigger multiple SOS incidents.
- One SOS incident can contain multiple location updates.

---

## 5. Geospatial Data Model

### GPS Point

A GPS point represents one captured location during an activity.

Recommended PostGIS type:

```sql
geography(Point, 4326)
```

### Activity Path

The complete recorded path of an activity.

Recommended PostGIS type:

```sql
geography(LineString, 4326)
```

### Published Route

A curated or community-created route.

Recommended PostGIS type:

```sql
geography(LineString, 4326)
```

### Location

A single event, community, or SOS location.

Recommended PostGIS type:

```sql
geography(Point, 4326)
```

---

## 6. Activity Recording Strategy

GPS points will be stored separately from the activity summary.

The activity record stores summary information such as:

- Total distance
- Duration
- Average pace
- Maximum speed
- Calories
- Elevation gain
- Start time
- End time
- Final route path

The activity_points table stores individual samples such as:

- Latitude and longitude
- Altitude
- Accuracy
- Speed
- Heading
- Recorded time
- Sequence number

This separation allows summary screens to load without retrieving every GPS point.

---

## 7. Route Safety Model

UrbanStride will use a general route-safety model instead of labeling a route as completely safe or unsafe.

Possible safety signals include:

- Lighting
- Traffic
- Surface condition
- Isolation
- Accessibility
- Community reports
- Time relevance
- Verification status

A safety score is advisory and must not be presented as a guarantee of personal safety.

Initial scores may remain unavailable until sufficient reliable data exists.

---

## 8. Data Privacy Principles

- GPS data is private by default.
- Users control activity visibility.
- Precise home and work locations must not be publicly exposed.
- Public activity maps should support hiding start and end areas.
- Trusted-contact information is private.
- SOS information is restricted to authorized access.
- Sensitive location access should be logged.
- Account deletion must remove or anonymize personal data according to the final retention policy.

---

## 9. Activity Visibility

Supported visibility values:

```text
private
followers
community
public
```

Default value:

```text
private
```

Users must deliberately select broader visibility.

---

## 10. Deletion Strategy

Important business records will initially use soft deletion.

Soft-delete field:

```text
deleted_at
```

Applicable examples:

- users
- activities
- routes
- posts
- communities
- events

Join tables such as likes and follows may use physical deletion when a user removes the relationship.

---

## 11. Audit Fields

Most business tables must include:

```text
created_at
updated_at
```

Tables requiring soft deletion should also include:

```text
deleted_at
```

Security-sensitive operations may additionally record:

```text
created_by
updated_by
```

---

## 12. MVP Database Scope

The first implementation should prioritize:

1. users
2. user_auth_identities
3. trusted_contacts
4. activities
5. activity_points
6. routes
7. user_follows
8. posts
9. post_likes
10. comments
11. communities
12. community_members
13. events
14. event_registrations
15. notifications
16. sos_incidents
17. sos_location_updates
18. user_devices

Tables outside this scope should not block the first usable release.

---

## 13. Future Database Capabilities

Future versions may add:

- Challenges and challenge participants
- Badges and user achievements
- Training plans
- Subscription plans
- Payments and invoices
- AI recommendations
- Wearable-device integrations
- Route moderation
- Content reports
- Corporate wellness programs
- Admin audit logs