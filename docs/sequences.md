# Sequence Diagrams

## Đăng ký tài khoản
```
User -> Frontend: Nhập thông tin đăng ký
Frontend -> Backend: POST /auth/register
Backend -> Database: Lưu thông tin user
Database -> Backend: Xác nhận lưu thành công
Backend -> Frontend: Trả về token JWT
Frontend -> User: Chuyển hướng đến trang chủ
```

## Đăng nhập
```
User -> Frontend: Nhập email/password
Frontend -> Backend: POST /auth/login
Backend -> Database: Kiểm tra thông tin
Database -> Backend: Trả về thông tin user
Backend -> Frontend: Trả về token JWT
Frontend -> User: Đăng nhập thành công
```

## Xem danh sách anime
```
User -> Frontend: Truy cập trang anime
Frontend -> Backend: GET /anime
Backend -> Database: Lấy danh sách anime
Database -> Backend: Trả về danh sách
Backend -> Frontend: Trả về dữ liệu anime
Frontend -> User: Hiển thị danh sách anime
```

## Xem video anime
```
User -> Frontend: Click xem anime
Frontend -> Backend: GET /anime/{id}/episode/{episode}
Backend -> Database: Kiểm tra quyền truy cập
Database -> Backend: Xác nhận quyền
Backend -> Frontend: Trả về URL video
Frontend -> User: Phát video anime
```

## Đánh giá anime
```
User -> Frontend: Chọn rating
Frontend -> Backend: POST /anime/{id}/rating
Backend -> Database: Lưu rating
Database -> Backend: Xác nhận lưu
Backend -> Frontend: Cập nhật rating thành công
Frontend -> User: Hiển thị rating mới
```

## Admin thêm anime mới
```
Admin -> Frontend: Nhập thông tin anime
Frontend -> Backend: POST /admin/anime
Backend -> Database: Lưu thông tin anime
Database -> Backend: Xác nhận lưu
Backend -> Frontend: Trả về anime mới
Frontend -> Admin: Hiển thị anime đã thêm
```
