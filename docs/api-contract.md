# API Contract

Base URL: `http://localhost:3001`

- Authorization: Bearer token cho endpoints cần đăng nhập
- Error format: `{ "statusCode": 400, "message": "...", "error": "Bad Request" }`
- Tất cả endpoints (trừ auth) đều yêu cầu JWT authentication và permissions

## ✅ Auth (Đã implement)

### POST /auth/register
- Body: `{ email, password, username }`
- Response 201: `{ accessToken, refreshToken }`

### POST /auth/login
- Body: `{ email, password }`
- Response 200: `{ accessToken, refreshToken }`

### POST /auth/refresh
- Body: `{ refreshToken }`
- Response 200: `{ accessToken, refreshToken }`

### POST /auth/logout
- Header: `Authorization: Bearer <accessToken>`
- Response 200: `{ success: true }`

### GET /auth/me
- Header: `Authorization: Bearer <accessToken>`
- Response 200: `{ sub, email, username, permissions }`

## ✅ Anime (Đã implement)

### GET /anime
- Query params: `q?` (search), `offset?` (default: 0), `limit?` (default: 20)
- Permissions: `Resource.Anime, Action.View`
- Response 200: `{ items: Anime[], total: number, offset: number, limit: number }`

### GET /anime/:id
- Permissions: `Resource.Anime, Action.View`
- Response 200: `Anime`

### POST /anime
- Body: `CreateAnimeDto` (title, slug, description?, genres?, status?, year?, coverImage?, bannerImage?, studios?, episodesCount?, rating?)
- Permissions: `Resource.Anime, Action.Create`
- Response 201: `Anime`

### PATCH /anime/:id
- Body: `UpdateAnimeDto` (tất cả fields optional)
- Permissions: `Resource.Anime, Action.Update`
- Response 200: `Anime`

### DELETE /anime/:id
- Permissions: `Resource.Anime, Action.Delete`
- Response 200: `{ deleted: true }`

## ✅ Episodes (Đã implement)

### GET /episodes
- Query params: `animeId?`, `q?` (search), `offset?` (default: 0), `limit?` (default: 20)
- Permissions: `Resource.Episode, Action.View`
- Response 200: `{ items: Episode[], total: number, offset: number, limit: number }`

### GET /episodes/:id
- Permissions: `Resource.Episode, Action.View`
- Response 200: `Episode`

### POST /episodes
- Body: `CreateEpisodeDto` (animeId, title, number, synopsis?, videoUrl?, thumbnailUrl?, durationSec?, airDate?)
- Permissions: `Resource.Episode, Action.Create`
- Response 201: `Episode`

### PATCH /episodes/:id
- Body: `UpdateEpisodeDto` (tất cả fields optional)
- Permissions: `Resource.Episode, Action.Update`
- Response 200: `Episode`

### DELETE /episodes/:id
- Permissions: `Resource.Episode, Action.Delete`
- Response 200: `{ deleted: true }`

### POST /episodes/:id/upload-video
- Content-Type: `multipart/form-data`
- Body: `video` (file)
- Permissions: `Resource.Episode, Action.Update`
- Response 200: `{ id, videoUrl, message }`
- Note: Tự động trigger HLS transcoding

### POST /episodes/:id/upload-thumbnail
- Content-Type: `multipart/form-data`
- Body: `thumbnail` (file)
- Permissions: `Resource.Episode, Action.Update`
- Response 200: `{ id, thumbnailUrl, message }`

### GET /episodes/:id/stream
- Header: `Range: bytes=start-end` (optional, for partial content)
- Permissions: `Resource.Episode, Action.View`
- Response 200/206: Video stream (with proper range headers)

### GET /episodes/:id/video-url
- Query params: `expiresIn?` (seconds, default: 3600)
- Permissions: `Resource.Episode, Action.View`
- Response 200: `{ videoUrl: string, expiresIn: number }`

### GET /episodes/:id/hls/master.m3u8
- Permissions: `Resource.Episode, Action.View`
- Response 200: HLS master manifest (Content-Type: `application/vnd.apple.mpegurl`)

### GET /episodes/:id/hls/:quality.m3u8
- Params: `quality` (e.g., "360p", "720p")
- Permissions: `Resource.Episode, Action.View`
- Response 200: HLS variant manifest

### GET /episodes/:id/hls/:quality/:segment
- Params: `quality`, `segment` (filename)
- Permissions: `Resource.Episode, Action.View`
- Response 200: HLS segment stream (Content-Type: `video/mp2t`)

### POST /episodes/:id/transcode
- Query params: `qualities?` (comma-separated, default: "360p,480p,720p,1080p")
- Permissions: `Resource.Episode, Action.Update`
- Response 200: `{ jobId, status }`

### GET /episodes/:id/transcode/status
- Permissions: `Resource.Episode, Action.View`
- Response 200: `{ status, progress?, hlsManifestUrl?, variants?, error? }`

### POST /episodes/transcode/callback
- Body: `{ detail: { jobId }, jobId }` (MediaConvert webhook)
- Response 200: `{ success: true, jobId }`
- Note: Webhook endpoint, nên được bảo vệ trong production

## ❌ User Actions (Chưa implement - Cần thiết)

### POST /progress
- Body: `{ animeId, episodeId, currentTime, duration }`
- Response 201: `UserAnimeProgress`

### GET /progress
- Query params: `animeId?`
- Response 200: `UserAnimeProgress[]`

### POST /favorites/:animeId
- Response 201: `Favorite`

### DELETE /favorites/:animeId
- Response 200: `{ deleted: true }`

### GET /favorites
- Response 200: `Favorite[]`

### POST /anime/:id/rating
- Body: `{ rating: number }` (1-10)
- Response 201: `Rating`

### GET /anime/:id/rating
- Response 200: `{ rating: number, userRating?: number, totalRatings: number }`

### GET /anime/:id/comments
- Query params: `offset?`, `limit?`
- Response 200: `{ items: Comment[], total: number }`

### POST /anime/:id/comments
- Body: `{ content: string, parentId?: string }`
- Response 201: `Comment`

### GET /episodes/:id/comments
- Query params: `offset?`, `limit?`
- Response 200: `{ items: Comment[], total: number }`

### POST /episodes/:id/comments
- Body: `{ content: string, parentId?: string }`
- Response 201: `Comment`

## ❌ User Profile (Chưa implement - Cần thiết)

### GET /user/profile
- Response 200: `User` (with additional stats)

### PUT /user/profile
- Body: `{ username?, avatar?, bio? }`
- Response 200: `User`

### GET /user/stats
- Response 200: `{ watchTime, animeWatched, favoritesCount, etc. }`

## ❌ Admin (Chưa implement - Cần thiết)

### GET /admin/users
- Query params: `offset?`, `limit?`, `q?`
- Permissions: Admin only
- Response 200: `{ items: User[], total: number }`

### GET /admin/stats
- Permissions: Admin only
- Response 200: `{ totalUsers, totalAnime, totalEpisodes, etc. }`

## ❌ Additional Features (Tương lai)

### Continue Watching
- GET /user/continue-watching
- Response 200: `Anime[]` (với progress info)

### Recommendations
- GET /user/recommendations
- Response 200: `Anime[]`

### Trending/Popular
- GET /anime/trending
- GET /anime/popular
- Response 200: `Anime[]`

### Recently Added
- GET /anime/recent
- Response 200: `Anime[]`
