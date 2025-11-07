# Mongoose Schemas (Data Model)

## ✅ Đã implement

### User Schema
**File:** `backend/src/users/schemas/user.schema.ts`

- `email: string` (required, unique, index, lowercase)
- `password: string` (required, select: false, minlength: 6)
- `username: string` (required, trim)
- `refreshTokenHash?: string` (optional, select: false)
- `permissions: Map<string, number>`: bitmask actions trên từng resource
  - Action: VIEW=1, CREATE=2, UPDATE=4, DELETE=8
  - Ví dụ: `{ anime: 1, rating: 1, comment: 3, "*": 15 }`
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes:**
- `email` (unique)

### Anime Schema
**File:** `backend/src/anime/schemas/anime.schema.ts`

- `title: string` (required, trim, index)
- `slug: string` (required, unique, index, lowercase, trim)
- `description?: string` (optional, trim)
- `genres: string[]` (default: [])
- `status: AnimeStatus` (enum: 'ongoing' | 'completed' | 'hiatus' | 'announced', default: 'ongoing')
- `year?: number` (optional, min: 1900, max: 3000)
- `coverImage?: string` (optional)
- `bannerImage?: string` (optional)
- `studios: string[]` (default: [])
- `episodesCount: number` (default: 0, min: 0)
- `rating: number` (default: 0, min: 0, max: 10)
- `extra?: Record<string, unknown>` (optional, mixed type)
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes:**
- `title` (text index for search)
- `slug` (unique)

### Episode Schema
**File:** `backend/src/episodes/schemas/episode.schema.ts`

- `animeId: ObjectId` (required, ref: 'Anime', index)
- `title: string` (required, trim)
- `number: number` (required, min: 0, index)
- `synopsis?: string` (optional, trim)
- `videoUrl?: string` (optional, deprecated - backward compatibility)
- `thumbnailUrl?: string` (optional)
- `durationSec?: number` (optional, min: 0)
- `airDate?: Date` (optional)
- `hlsManifestUrl?: string` (optional, Master manifest URL)
- `hlsVariants?: Record<string, HLSVariant>` (optional)
  - `HLSVariant`: `{ manifestUrl, resolution, bandwidth, codecs?, frameRate? }`
- `extra?: Record<string, unknown>` (optional, stores transcodingJobId, etc.)
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes:**
- `{ animeId: 1, number: 1 }` (unique compound)
- `animeId` (for filtering)

## ❌ Chưa implement - Cần thiết

### UserAnimeProgress Schema
**Cần tạo:** `backend/src/progress/schemas/progress.schema.ts`

- `userId: ObjectId` (required, ref: 'User', index)
- `animeId: ObjectId` (required, ref: 'Anime', index)
- `episodeId: ObjectId` (required, ref: 'Episode', index)
- `currentTime: number` (seconds watched)
- `duration: number` (total duration in seconds)
- `progressPercentage: number` (0-100)
- `lastWatchedAt: Date`
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes cần thiết:**
- `{ userId: 1, animeId: 1 }` (unique compound)
- `userId` (for user's watch history)

### Favorites Schema
**Cần tạo:** `backend/src/favorites/schemas/favorite.schema.ts`

- `userId: ObjectId` (required, ref: 'User', index)
- `animeId: ObjectId` (required, ref: 'Anime', index)
- `notes?: string` (optional, user's personal notes)
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes cần thiết:**
- `{ userId: 1, animeId: 1 }` (unique compound)
- `userId` (for user's favorites list)

### Ratings Schema
**Cần tạo:** `backend/src/ratings/schemas/rating.schema.ts`

- `userId: ObjectId` (required, ref: 'User', index)
- `animeId: ObjectId` (required, ref: 'Anime', index)
- `rating: number` (required, 1-10)
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes cần thiết:**
- `{ userId: 1, animeId: 1 }` (unique compound)
- `animeId` (for calculating average rating)

### Comments Schema
**Cần tạo:** `backend/src/comments/schemas/comment.schema.ts`

- `userId: ObjectId` (required, ref: 'User', index)
- `animeId?: ObjectId` (optional, ref: 'Anime', index - null if episode comment)
- `episodeId?: ObjectId` (optional, ref: 'Episode', index - null if anime comment)
- `parentId?: ObjectId` (optional, ref: 'Comment' - for replies)
- `content: string` (required)
- `likes: number` (default: 0)
- `createdAt: Date` (auto)
- `updatedAt: Date` (auto)

**Indexes cần thiết:**
- `animeId` (for anime comments)
- `episodeId` (for episode comments)
- `parentId` (for replies)
- `createdAt` (for sorting)

## Schema mở rộng (Tương lai)

Xem thêm trong `database.md` phần "Schema mở rộng" cho:
- Notifications
- Playlists/Custom Lists
- Subtitles
- Related Anime
- Characters
- Staff/Creators
- Video Sources
- News/Announcements

## Permission Resources

**File:** `backend/src/auth/permissions.ts`

Các resource hiện có:
- `Resource.Anime`
- `Resource.Episode`
- `Resource.Rating` (chưa implement nhưng đã có trong enum)
- `Resource.Comment` (chưa implement nhưng đã có trong enum)
- `Resource.User`

**Actions:**
- `Action.View = 1`
- `Action.Create = 2`
- `Action.Update = 4`
- `Action.Delete = 8`

Xem thêm chi tiết phần Database để biết cách seed/cập nhật permissions.
