# Database (MongoDB) & Setup

## Collections Structure

### Users
```javascript
{ _id: ObjectId, email: String, password: String, username: String, role: String, createdAt: Date, updatedAt: Date }
```

### Anime
```javascript
{ _id: ObjectId, title: String, description: String, genre: [String], releaseDate: Date, status: String, rating: Number, posterUrl: String, trailerUrl: String, episodes: [ObjectId], createdAt: Date, updatedAt: Date }
```

### Episodes
```javascript
{ _id: ObjectId, animeId: ObjectId, episodeNumber: Number, title: String, description: String, videoUrl: String, duration: Number, releaseDate: Date, createdAt: Date, updatedAt: Date }
```

### UserAnimeProgress
```javascript
{ _id: ObjectId, userId: ObjectId, animeId: ObjectId, episodeId: ObjectId, watchedAt: Date, progressPercentage: Number, createdAt: Date, updatedAt: Date }
```

### Ratings
```javascript
{ _id: ObjectId, userId: ObjectId, animeId: ObjectId, rating: Number, createdAt: Date, updatedAt: Date }
```

### Comments
```javascript
{ _id: ObjectId, userId: ObjectId, animeId: ObjectId, content: String, createdAt: Date, updatedAt: Date }
```

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
DB.createCollection("users")
DB.createCollection("anime")
DB.createCollection("episodes")
DB.createCollection("useranimeprogress")
DB.createCollection("ratings")
DB.createCollection("comments")
```

### Index gợi ý
- users.email unique
- anime.title text
- episodes.animeId

### Lưu trữ tệp lớn
- Ưu tiên object storage (S3/Cloud); GridFS nếu cần trong DB
