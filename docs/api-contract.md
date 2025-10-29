# API Contract

Base URL: `http://localhost:3001`

- Authorization: Bearer token cho endpoints cần đăng nhập
- Error format: `{ "statusCode": 400, "message": "...", "error": "Bad Request" }`

## Auth
- POST /auth/register
- POST /auth/login
  - Body: `{ email, password }`
  - 200: `{ accessToken, refreshToken }`
- POST /auth/refresh
  - Body: `{ refreshToken }`
  - 200: `{ accessToken, refreshToken }`
- POST /auth/logout
  - Header: `Authorization: Bearer <accessToken>`
  - 200: `{ success: true }`
- GET /auth/me
  - Header: `Authorization: Bearer <accessToken>`
  - 200: `{ sub, email, username, permissions }`

## Anime
- GET /anime (q, genre, status, page, limit, sort)
- GET /anime/:id
- GET /anime/:id/episodes
- GET /anime/:id/episode/:episodeNumber (playback)

## Actions
- POST /progress
- POST /favorites/:animeId
- DELETE /favorites/:animeId
- POST /anime/:id/rating
- GET/POST /anime/:id/comments

## User
- GET /user/profile
- PUT /user/profile

## Admin
- POST /admin/anime
- PUT /admin/anime/:id
- DELETE /admin/anime/:id
- GET /admin/users
- GET /admin/stats
