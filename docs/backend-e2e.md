# Backend E2E Checklist (NestJS + MongoDB)

## Chuẩn bị
- Node.js, MongoDB/Atlas, DB `aniplay_db`
- `.env` theo `backend-setup.md`

## Cài đặt
- Tạo project NestJS
- Cài deps: mongoose, jwt/passport, bcrypt, validators, throttler
- Kết nối Mongoose + ValidationPipe + CORS

## Schemas & DTOs
- Tạo schemas: User, Anime, Episode, Progress, Rating, Comment, Favorite
- Tạo DTOs: Auth, Anime, Episode, Rating, Comment, Progress, User, Paging

## Modules thứ tự
1) Auth
2) Anime, Episodes
3) Ratings, Comments, Favorites, Progress

## Seeding (tuỳ chọn)
- Thêm vài anime/episodes/user test

## Chạy dev
```bash
npm run start:dev
```

## Kiểm thử
- Register/Login, list/detail anime, playback, rate/comment, favorites, progress

## Deploy tối thiểu
- Atlas hoặc Docker MongoDB
- Build NestJS, thiết lập env production
