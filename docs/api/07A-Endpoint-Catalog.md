<<Authentication>>

POST   /auth/google
POST   /auth/otp/request
POST   /auth/otp/verify
POST   /auth/refresh-token
POST   /auth/logout
GET    /auth/me

<<Users>>


GET    /users/me
PATCH  /users/me

GET    /users/:id
GET    /users/:id/activities

POST   /users/:id/follow
DELETE /users/:id/follow

<<Activities>>

POST   /activities

GET    /activities/:id

GET    /activities

PATCH  /activities/:id

DELETE /activities/:id

<<GPS Tracking>>



POST /activities/:id/start

POST /activities/:id/pause

POST /activities/:id/resume

POST /activities/:id/finish

POST /activities/:id/location



<<Routes>>

GET /routes

GET /routes/:id

POST /routes

PATCH /routes/:id


<<Feed>>


GET /feed

POST /posts

GET /posts/:id

DELETE /posts/:id
``

<<Likes>>

POST   /posts/:id/like
DELETE /posts/:id/like


<<Comments>>

POST /posts/:id/comments

GET /posts/:id/comments

DELETE /comments/:id


<<Communities>>

POST /communities

GET /communities

GET /communities/:id

POST /communities/:id/join

POST /communities/:id/leave


<<Events>>

GET /events

GET /events/:id

POST /events

POST /events/:id/register

DELETE /events/:id/register


<<Notifications>>


GET /notifications

PATCH /notifications

<<SOS>>

POST /sos/start

POST /sos/location

POST /sos/stop

GET /sos/history

