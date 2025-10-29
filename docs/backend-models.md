# Mongoose Schemas (Data Model)

Bao gồm: User, Anime, Episode, UserAnimeProgress, Rating, Comment, Favorite và đăng ký schema vào module.

## User schema (permissions theo resource)
- `email: string` (unique, index)
- `password: string` (select: false)
- `username: string`
- `permissions: Map<string, number>`: bitmask actions trên từng resource
  - Action: VIEW=1, CREATE=2, UPDATE=4, DELETE=8
  - Ví dụ: `{ anime: 1, rating: 1, comment: 3, "*": 15 }`

Xem thêm chi tiết phần Database để biết cách seed/cập nhật permissions.
