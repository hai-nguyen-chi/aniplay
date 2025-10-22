# Tổng quan & Kiến trúc

## Mục đích
Xây dựng website xem anime trực tuyến với tìm kiếm, xem, đánh giá, bình luận, và quản trị nội dung.

## Công nghệ sử dụng
- Frontend: NextJS (React framework)
- Backend: NestJS (Node.js framework)
- Database: MongoDB (NoSQL)
- Authentication: JWT tokens

## Các Actor
- Guest: xem danh sách/chi tiết, tìm kiếm, đăng ký
- Registered: tất cả Guest + đăng nhập/đăng xuất, xem anime, yêu thích, đánh giá, bình luận, theo dõi tiến độ, quản lý profile
- Premium: tất cả Registered + không giới hạn, không quảng cáo, truy cập sớm, chất lượng cao
- Admin: quản trị nội dung, người dùng, bình luận, thống kê

## Chức năng chính
- Quản lý người dùng: đăng ký, đăng nhập/đăng xuất, profile, đổi mật khẩu, subscription
- Quản lý Anime: danh sách, lọc/tìm kiếm, chi tiết, xem video, tập phim, trạng thái xem
- Tương tác: đánh giá, bình luận, yêu thích, theo dõi tiến độ, chia sẻ
- Quản trị: CRUD anime/tập, người dùng, bình luận, thống kê, cài đặt

## Kiến trúc hệ thống (cao cấp)
- Frontend (NextJS): App Router, Components, State (Context/Redux), Styling (Tailwind)
- Backend (NestJS): Controllers, Services, Guards (JWT/Roles), Interceptors (Logging/Transform)
- Database (MongoDB): Mongoose ODM, indexes, aggregation, GridFS (tùy chọn)
