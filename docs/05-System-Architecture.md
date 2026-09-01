# UrbanStride System Architecture

Version: 1.0

Status: Draft

---

# Architecture Vision

UrbanStride is a mobile-first social fitness platform built using a scalable cloud-native architecture.

Goals:

- High Performance
- High Scalability
- Secure Authentication
- Real-Time GPS Tracking
- Event Driven Design
- Cloud Ready

---

# High Level Architecture

+-----------------------+
| Mobile Application    |
| React Native + Expo   |
+-----------------------+
            |
            |
            v
+-----------------------+
| API Gateway           |
+-----------------------+
            |
            |
            v
+-----------------------+
| NestJS Backend        |
+-----------------------+
            |
            |
+-----------+-----------+
|                       |
v                       v

PostgreSQL         Redis Cache

|
v

AWS Storage (S3)

---

# Frontend Architecture

Technology:

- React Native
- Expo
- TypeScript

Pattern:

Feature-Based Architecture

---

# Mobile Folder Structure

apps/mobile/

src/

features/

auth/
profile/
activities/
routes/
communities/
events/
safety/
notifications/

components/

ui/
forms/
cards/
modals/

services/

api/
storage/
gps/

navigation/

store/

utils/

---

# Backend Architecture

Technology:

- NestJS
- TypeScript

Architecture Pattern:

Modular Monolith

Reason:

- Easier development
- Easier deployment
- Can evolve into microservices later

---

# Backend Modules

auth

users

activities

routes

communities

events

notifications

safety

analytics

admin

---

# Database

Primary Database

PostgreSQL

Reason:

- ACID Compliance
- GIS Support
- JSON Support
- Scalable

---

# Cache Layer

Redis

Use Cases:

- Route Lookup Cache
- Community Feed Cache
- Leaderboards
- Session Storage

---

# Cloud Storage

AWS S3

Stores:

- User Photos
- Activity Images
- Community Assets
- Route Snapshots

---

# Authentication Architecture

Login Types:

- Google Login
- Mobile OTP

Authentication Flow:

User
→ Login
→ JWT Access Token
→ Refresh Token
→ Protected APIs

---

# GPS Tracking Flow

User Starts Run

↓

Mobile GPS

↓

Location Collection

↓

Activity Service

↓

Database Storage

↓

Analytics Engine

↓

Activity Summary

---

# Route Discovery Flow

User Search Route

↓

Route Service

↓

PostGIS Query

↓

Safety Score Service

↓

Result Display

---

# Community Flow

Create Post

↓

Community Service

↓

Database

↓

Followers Feed

↓

Notification Service

---

# Safety Architecture

SOS Trigger

↓

Emergency Service

↓

Trusted Contacts

↓

Live Location Sharing

↓

Notification Delivery

---

# Notifications

Technology:

Firebase Cloud Messaging (FCM)

Notifications:

- Community Activity
- Event Updates
- Follower Activity
- SOS Alerts

---

# API Communication

Protocol:

HTTPS

Data Format:

JSON

Authentication:

JWT

---

# Monitoring

Future Phase

Tools:

- CloudWatch
- Grafana
- Prometheus

---

# Logging

Application Logs

Error Logs

Audit Logs

Activity Logs

---

# Scalability Strategy

Phase 1

Single Server

↓

Phase 2

Load Balancer

↓

Phase 3

Containerized Services

↓

Phase 4

Microservices

---

# Security

HTTPS

JWT Authentication

Password Encryption

Rate Limiting

API Validation

Data Encryption

---

# Architecture Principles

1. Mobile First

2. Security First

3. Cloud Native

4. Performance Focused

5. Modular Design

6. Scalable by Design