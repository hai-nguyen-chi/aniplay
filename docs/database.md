# Database (MongoDB) & Setup

## Collections Structure

### ✅ Users (Đã implement)
```javascript
{
  _id: ObjectId,
  email: String,              // unique, index, lowercase
  password: String,           // hashed, select: false
  username: String,
  refreshTokenHash: String,   // optional, select: false
  // permissions theo resource: bitmask action (VIEW, CREATE, UPDATE, DELETE)
  // ví dụ: { anime: 1, rating: 1, comment: 3, "*": 15 }
  permissions: Map<String, Number>,
  createdAt: Date,
  updatedAt: Date
}
```

#### Permission bitmask
- VIEW = 1, CREATE = 2, UPDATE = 4, DELETE = 8
- Có thể đặt mặc định toàn cục qua key `"*"` (wildcard), ví dụ 15 = full CRUD
- Ví dụ user A:
  - `permissions.anime = 1` (xem anime)
  - `permissions.rating = 1` (xem rating)
  - `permissions.comment = 3` (xem + tạo comment)

### ✅ Anime (Đã implement)
```javascript
{
  _id: ObjectId,
  title: String,              // required, index
  slug: String,               // required, unique, index, lowercase
  description: String,        // optional
  genres: [String],           // array, default: []
  status: String,             // enum: 'ongoing' | 'completed' | 'hiatus' | 'announced', default: 'ongoing'
  year: Number,               // optional, min: 1900, max: 3000
  coverImage: String,         // optional, URL
  bannerImage: String,        // optional, URL
  studios: [String],          // array, default: []
  episodesCount: Number,      // default: 0, min: 0
  rating: Number,             // default: 0, min: 0, max: 10
  extra: Object,              // optional, mixed type for additional data
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `title` (text index for search)
- `slug` (unique)
- `createdAt` (for sorting)

### ✅ Episodes (Đã implement)
```javascript
{
  _id: ObjectId,
  animeId: ObjectId,          // required, ref: 'Anime', index
  title: String,              // required
  number: Number,             // required, min: 0, index
  synopsis: String,           // optional
  videoUrl: String,           // optional, deprecated (backward compatibility)
  thumbnailUrl: String,       // optional, URL
  durationSec: Number,        // optional, min: 0
  airDate: Date,              // optional
  // HLS streaming support
  hlsManifestUrl: String,     // optional, Master manifest URL (.m3u8)
  hlsVariants: {              // optional, object
    [quality: string]: {
      manifestUrl: String,    // Variant manifest URL
      resolution: String,     // "360p", "480p", "720p", "1080p"
      bandwidth: Number,      // bits per second
      codecs: String,         // optional, e.g. "avc1.42e01e,mp4a.40.2"
      frameRate: Number       // optional
    }
  },
  extra: Object,              // optional, mixed type (stores transcodingJobId, etc.)
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `{ animeId: 1, number: 1 }` (unique compound index)
- `animeId` (for filtering)

### ❌ UserAnimeProgress (Chưa implement - Cần thiết)
```javascript
{
  _id: ObjectId,
  userId: ObjectId,           // required, ref: 'User', index
  animeId: ObjectId,          // required, ref: 'Anime', index
  episodeId: ObjectId,        // required, ref: 'Episode', index
  currentTime: Number,        // seconds watched
  duration: Number,           // total duration in seconds
  progressPercentage: Number, // 0-100
  lastWatchedAt: Date,        // last watch timestamp
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes cần thiết:**
- `{ userId: 1, animeId: 1 }` (unique compound index)
- `userId` (for user's watch history)
- `lastWatchedAt` (for sorting)

### ❌ Favorites (Chưa implement - Cần thiết)
```javascript
{
  _id: ObjectId,
  userId: ObjectId,           // required, ref: 'User', index
  animeId: ObjectId,          // required, ref: 'Anime', index
  notes: String,              // optional, user's personal notes
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes cần thiết:**
- `{ userId: 1, animeId: 1 }` (unique compound index)
- `userId` (for user's favorites list)
- `createdAt` (for sorting)

### ❌ Ratings (Chưa implement - Cần thiết)
```javascript
{
  _id: ObjectId,
  userId: ObjectId,           // required, ref: 'User', index
  animeId: ObjectId,          // required, ref: 'Anime', index
  rating: Number,             // required, 1-10
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes cần thiết:**
- `{ userId: 1, animeId: 1 }` (unique compound index)
- `animeId` (for calculating average rating)
- `rating` (for filtering/sorting)

### ❌ Comments (Chưa implement - Cần thiết)
```javascript
{
  _id: ObjectId,
  userId: ObjectId,           // required, ref: 'User', index
  animeId: ObjectId,          // optional, ref: 'Anime', index (null if episode comment)
  episodeId: ObjectId,        // optional, ref: 'Episode', index (null if anime comment)
  parentId: ObjectId,         // optional, ref: 'Comment' (for replies)
  content: String,            // required
  likes: Number,              // default: 0
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes cần thiết:**
- `animeId` (for anime comments)
- `episodeId` (for episode comments)
- `parentId` (for replies)
- `createdAt` (for sorting)

## MongoDB Setup
- Local: cài đặt và chạy mongod
- Atlas: tạo cluster và lấy SRV connection string

### Env
```env
MONGODB_URI=mongodb://localhost:27017/aniplay_db
JWT_SECRET=supersecret
JWT_EXPIRES_IN=1d
```

### Collections (optional)
```javascript
use aniplay_db
// ✅ Đã có
DB.createCollection("users")
DB.createCollection("anime")
DB.createCollection("episodes")

// ❌ Chưa có - Cần tạo
DB.createCollection("useranimeprogress")
DB.createCollection("favorites")
DB.createCollection("ratings")
DB.createCollection("comments")
```

### Index gợi ý

**✅ Đã implement:**
- `users.email` (unique)
- `users.slug` (unique)
- `anime.title` (text index)
- `anime.slug` (unique)
- `episodes.animeId` (index)
- `episodes.{animeId, number}` (unique compound)

**❌ Cần thêm khi implement các schema mới:**
- `useranimeprogress.{userId, animeId}` (unique compound)
- `favorites.{userId, animeId}` (unique compound)
- `ratings.{userId, animeId}` (unique compound)
- `comments.animeId` (index)
- `comments.episodeId` (index)
- `comments.parentId` (index)

### Lưu trữ tệp lớn
- ✅ **Đã implement**: S3 integration cho video và thumbnail
- Video được lưu trên S3, không lưu trong MongoDB
- HLS transcoding sử dụng AWS MediaConvert
- GridFS không cần thiết vì đã dùng S3

## Schema mở rộng (Tương lai - Chưa implement)

### Notifications
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  type: String,               // 'new_episode', 'new_anime', 'comment_reply', etc.
  title: String,
  message: String,
  relatedId: ObjectId,        // animeId, episodeId, etc.
  read: Boolean,              // default: false
  createdAt: Date
}
```

### Playlists/Custom Lists
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  name: String,
  description: String,
  animeIds: [ObjectId],
  isPublic: Boolean,          // default: false
  createdAt: Date,
  updatedAt: Date
}
```

### Subtitles
```javascript
{
  _id: ObjectId,
  episodeId: ObjectId,
  language: String,           // 'vi', 'en', 'ja', etc.
  format: String,             // 'srt', 'vtt'
  url: String,
  isDefault: Boolean,
  createdAt: Date
}
```

### Related Anime
```javascript
{
  _id: ObjectId,
  animeId: ObjectId,
  relatedAnimeId: ObjectId,
  relationType: String        // 'sequel', 'prequel', 'spin_off', 'similar'
}
```
