# Missing Features & Implementation Roadmap

Tài liệu này tổng hợp các tính năng và schema còn thiếu để website giống như animevietsub/anime47.

## 📊 Tổng quan

### ✅ Đã có
- Authentication (JWT, refresh token)
- Anime CRUD
- Episodes CRUD
- Video streaming (S3, HLS support)
- Video transcoding (AWS MediaConvert)
- Permissions system
- User management cơ bản

### ❌ Còn thiếu - Ưu tiên cao (Phase 1)

#### 1. UserAnimeProgress (Lịch sử xem)
**Schema:** Xem `database.md`
**Endpoints cần:**
- `POST /progress` - Lưu tiến độ xem
- `GET /progress` - Lấy lịch sử xem
- `GET /user/continue-watching` - Danh sách anime đang xem dở

**Tác động:**
- User có thể tiếp tục xem từ vị trí đã dừng
- Hiển thị "Continue Watching" trên homepage
- Thống kê thời gian xem

#### 2. Favorites (Yêu thích)
**Schema:** Xem `database.md`
**Endpoints cần:**
- `POST /favorites/:animeId` - Thêm vào yêu thích
- `DELETE /favorites/:animeId` - Xóa khỏi yêu thích
- `GET /favorites` - Danh sách yêu thích

**Tác động:**
- User có thể lưu anime yêu thích
- Hiển thị danh sách yêu thích trong profile

#### 3. Ratings (Đánh giá)
**Schema:** Xem `database.md`
**Endpoints cần:**
- `POST /anime/:id/rating` - Đánh giá anime
- `GET /anime/:id/rating` - Xem đánh giá (tổng hợp + rating của user)
- Tự động cập nhật `anime.rating` khi có rating mới

**Tác động:**
- User có thể đánh giá anime (1-10)
- Hiển thị điểm trung bình trên trang anime
- Sắp xếp anime theo rating

#### 4. Comments (Bình luận)
**Schema:** Xem `database.md`
**Endpoints cần:**
- `GET /anime/:id/comments` - Lấy bình luận của anime
- `POST /anime/:id/comments` - Tạo bình luận
- `GET /episodes/:id/comments` - Lấy bình luận của episode
- `POST /episodes/:id/comments` - Tạo bình luận episode
- `POST /comments/:id/reply` - Trả lời bình luận
- `POST /comments/:id/like` - Like bình luận

**Tác động:**
- User có thể bình luận về anime/episode
- Hỗ trợ reply (nested comments)
- Like comments
- Tạo cộng đồng tương tác

### ❌ Còn thiếu - Ưu tiên trung bình (Phase 2)

#### 5. User Profile Enhancement
**Cần bổ sung vào User schema:**
- `avatar?: string` - Avatar URL
- `bio?: string` - Giới thiệu
- `favoriteGenres?: string[]` - Thể loại yêu thích
- `watchTime?: number` - Tổng thời gian xem (tính từ progress)
- `animeWatched?: number` - Số anime đã xem
- `settings?: object` - Cài đặt (notifications, privacy)

**Endpoints cần:**
- `GET /user/profile` - Xem profile
- `PUT /user/profile` - Cập nhật profile
- `GET /user/stats` - Thống kê (watchTime, animeWatched, favoritesCount)

#### 6. Advanced Search & Filter
**Cần bổ sung vào QueryAnimeDto:**
- `genres?: string[]` - Lọc theo thể loại
- `status?: AnimeStatus` - Lọc theo trạng thái
- `year?: number` - Lọc theo năm
- `minRating?: number` - Rating tối thiểu
- `sort?: string` - Sắp xếp (rating, year, createdAt, title)

**Endpoints cần:**
- Cải thiện `GET /anime` với filters mới
- `GET /anime/trending` - Anime đang hot
- `GET /anime/popular` - Anime phổ biến
- `GET /anime/recent` - Anime mới cập nhật

#### 7. Recommendations
**Endpoints cần:**
- `GET /user/recommendations` - Gợi ý dựa trên lịch sử xem

**Logic:**
- Dựa trên genres yêu thích
- Dựa trên anime đã xem
- Dựa trên ratings đã cho

### ❌ Còn thiếu - Ưu tiên thấp (Phase 3)

#### 8. Notifications
**Schema:** Xem `database.md`
**Endpoints cần:**
- `GET /notifications` - Lấy thông báo
- `PUT /notifications/:id/read` - Đánh dấu đã đọc
- `POST /notifications/mark-all-read` - Đánh dấu tất cả đã đọc

**Loại thông báo:**
- Anime mới được thêm
- Episode mới được upload
- Reply comment
- Anime yêu thích có episode mới

#### 9. Subtitles
**Schema:** Xem `database.md`
**Endpoints cần:**
- `GET /episodes/:id/subtitles` - Lấy danh sách phụ đề
- `POST /episodes/:id/subtitles` - Upload phụ đề
- `GET /episodes/:id/subtitles/:lang` - Lấy phụ đề theo ngôn ngữ

#### 10. Multiple Video Sources
**Cần bổ sung vào Episode schema:**
- `videoSources?: VideoSource[]` - Nhiều nguồn video
  - `serverName: string`
  - `videoUrl: string`
  - `quality: string`
  - `isPrimary: boolean`

**Endpoints cần:**
- `GET /episodes/:id/sources` - Lấy danh sách sources
- User có thể chọn server khác nếu server chính lỗi

#### 11. Related Anime
**Schema:** Xem `database.md`
**Endpoints cần:**
- `GET /anime/:id/related` - Lấy anime liên quan
- `POST /anime/:id/related` - Thêm anime liên quan

**Relation types:**
- sequel (phần tiếp theo)
- prequel (phần trước)
- spin_off
- similar (tương tự)

#### 12. Playlists/Custom Lists
**Schema:** Xem `database.md`
**Endpoints cần:**
- `GET /playlists` - Lấy danh sách playlist
- `POST /playlists` - Tạo playlist
- `PUT /playlists/:id` - Cập nhật playlist
- `DELETE /playlists/:id` - Xóa playlist
- `POST /playlists/:id/anime/:animeId` - Thêm anime vào playlist
- `DELETE /playlists/:id/anime/:animeId` - Xóa anime khỏi playlist

#### 13. Admin Features
**Endpoints cần:**
- `GET /admin/users` - Quản lý users
- `PUT /admin/users/:id` - Cập nhật user (permissions, ban, etc.)
- `GET /admin/stats` - Thống kê tổng quan
- `GET /admin/analytics` - Phân tích chi tiết

#### 14. Characters & Staff
**Schemas:** Xem `database.md`
**Endpoints cần:**
- `GET /anime/:id/characters` - Lấy danh sách nhân vật
- `POST /anime/:id/characters` - Thêm nhân vật
- `GET /anime/:id/staff` - Lấy đội ngũ sản xuất
- `POST /anime/:id/staff` - Thêm staff

#### 15. News/Announcements
**Schema:** Xem `database.md`
**Endpoints cần:**
- `GET /news` - Lấy tin tức
- `POST /news` - Tạo tin tức (admin)
- `GET /anime/:id/news` - Tin tức liên quan đến anime

## 📝 Cải tiến Schema hiện có

### Anime Schema - Cần bổ sung
- `alternativeTitles?: string[]` - Tên khác
- `trailerUrl?: string` - Trailer YouTube
- `totalEpisodes?: number` - Tổng số tập dự kiến
- `season?: string` - Mùa (Spring/Summer/Fall/Winter)
- `seasonYear?: number` - Năm phát sóng
- `ageRating?: string` - Độ tuổi (G, PG, PG-13, R, etc.)
- `source?: string` - Nguồn (manga/light novel/original)
- `viewCount?: number` - Lượt xem
- `favoriteCount?: number` - Lượt yêu thích (tính từ favorites)
- `averageRating?: number` - Điểm trung bình (tính từ ratings, tự động cập nhật)

### Episode Schema - Cần bổ sung
- `viewCount?: number` - Lượt xem
- `commentsCount?: number` - Số bình luận (tính từ comments, tự động cập nhật)

## 🎯 Roadmap Implementation

### Phase 1 (Core Features) - Ưu tiên cao
1. ✅ UserAnimeProgress
2. ✅ Favorites
3. ✅ Ratings
4. ✅ Comments (cơ bản)

**Thời gian ước tính:** 2-3 tuần

### Phase 2 (User Experience) - Ưu tiên trung bình
5. ✅ User Profile Enhancement
6. ✅ Advanced Search & Filter
7. ✅ Recommendations
8. ✅ Continue Watching

**Thời gian ước tính:** 2-3 tuần

### Phase 3 (Social & Content) - Ưu tiên thấp
9. ✅ Reply Comments
10. ✅ Like Comments
11. ✅ Subtitles
12. ✅ Multiple Video Sources
13. ✅ Related Anime
14. ✅ Notifications

**Thời gian ước tính:** 3-4 tuần

### Phase 4 (Advanced Features)
15. ✅ Playlists
16. ✅ Admin Features
17. ✅ Characters & Staff
18. ✅ News/Announcements

**Thời gian ước tính:** 2-3 tuần

## 📌 Notes

- Tất cả endpoints cần có permissions check
- Cần implement validation cho tất cả DTOs
- Cần có error handling đầy đủ
- Cần có tests cho các features mới
- Cần update API documentation khi implement

## 🔗 Related Documents

- `database.md` - Chi tiết schema
- `api-contract.md` - Chi tiết endpoints
- `backend-models.md` - Chi tiết Mongoose schemas

