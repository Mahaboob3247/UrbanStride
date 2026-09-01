# UrbanStride Database Standards

**Version:** 1.0  
**Status:** Draft

---

## 1. Naming Standards

Use:

```text
snake_case
```

Table names must be plural:

```text
users
activities
communities
```

Primary key:

```text
id
```

Foreign key:

```text
<table_singular>_id
```

Examples:

```text
user_id
activity_id
community_id
```

Timestamp fields:

```text
created_at
updated_at
deleted_at
```

Boolean fields begin with:

```text
is_
has_
can_
```

Examples:

```text
is_active
is_primary
```

---

## 2. Identifier Strategy

Use UUID values for externally visible business entities:

```text
users
activities
routes
posts
communities
events
sos_incidents
```

High-volume internal child records may use bigint identifiers:

```text
activity_points
sos_location_updates
```

---

## 3. Date and Time Standards

Use:

```text
timestamptz
```

Store timestamps consistently in UTC.

Convert timestamps to the user's local timezone only at the application boundary.

---

## 4. Measurement Standards

Store measurements using stable base units:

```text
distance      meters
duration      seconds
speed         meters per second
altitude      meters
pace          seconds per kilometer
temperature   degrees Celsius
currency      decimal plus ISO currency code
```

The mobile application can format values according to user preferences.

---

## 5. Index Strategy

Initial indexes should include:

```text
users(email)
users(phone_number)
users(username)

activities(user_id, started_at)
activities(status)
activity_points(activity_id, sequence_number)

routes(city, activity_type)
route_safety_reports(route_id, reported_at)

posts(author_id, created_at)
comments(post_id, created_at)

community_members(user_id)
events(starts_at, status)
notifications(user_id, read_at, created_at)

sos_incidents(user_id, status)
```

Geospatial columns should use appropriate spatial indexes.

Do not create indexes without a confirmed access pattern because unnecessary indexes increase write and storage costs.

---

## 6. Constraint Standards

Use database constraints for critical data integrity.

Examples:

```text
distance_meters >= 0
duration_seconds >= 0
entry_fee >= 0
capacity >= 0
severity within the supported range
follower_id <> following_id
started_at <= ended_at
```

Use unique constraints for relationships that must not be duplicated.

---

## 7. Migration Standards

- Every schema change requires a migration.
- Existing migration files must not be silently modified after use.
- Migration names must describe the change.
- Destructive migrations require an explicit data-migration plan.
- Migrations must be reviewed with the corresponding entity changes.
- Seed data must be separate from production migrations.

Example migration name:

```text
20260901_create_activities_table
```

---

## 8. Query Standards

- Avoid `SELECT *` in production queries.
- Return only fields required by the use case.
- Use pagination for collections.
- Avoid unbounded feed, activity, comment, event, and notification queries.
- Use transactions for multi-table operations requiring atomicity.
- Check query plans before adding performance-specific indexes.
- Do not expose internal database entities directly through APIs.

---

## 9. Privacy Standards

- Treat GPS history as sensitive.
- Keep precise activity locations private by default.
- Allow users to hide start and end locations.
- Encrypt data in transit.
- Restrict SOS and trusted-contact access.
- Avoid placing personal information in logs.
- Maintain an auditable trail for privileged access.
- Define data retention before production launch.

---

## 10. Seed Data

Development seed data may include:

- Test users
- Sample activities
- Demonstration routes
- Sample communities
- Sample events

Seed data must be synthetic and must not contain real personal or emergency-contact information.