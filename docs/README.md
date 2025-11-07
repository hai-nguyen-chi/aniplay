# Aniplay Documentation Index

Tài liệu được tách nhỏ theo chủ đề để dễ theo dõi và triển khai.

## 📚 Core Documentation

- Tổng quan & Kiến trúc: `architecture.md`
- Sơ đồ tuần tự: `sequences.md`
- Thiết kế Database (MongoDB) & Setup: `database.md`
- API Contract (Request/Response): `api-contract.md`
- Mongoose Schemas (Data Model): `backend-models.md`
- **Missing Features & Roadmap:** `missing-features.md` ⭐

## 🔧 Implementation Guides

- Service/Controller Contracts & DTOs: `backend-contracts.md`
- Middleware/Guards/Interceptors & Validation: `backend-security.md`
- Hướng dẫn Setup NestJS (Backend): `backend-setup.md`
- Hướng dẫn Setup NextJS (Frontend): `frontend-setup.md`

## ✅ Checklists

- Backend E2E Checklist: `backend-e2e.md`
- Frontend E2E Checklist: `frontend-e2e.md`

## 🚀 Operations

- Bảo mật, Hiệu năng, DevOps: `ops-security-perf.md`

## 🎯 Quick Start

1. Đọc `backend-setup.md` và `database.md`
2. Xem `missing-features.md` để biết các tính năng còn thiếu
3. Triển khai Auth theo `backend-contracts.md` và `backend-security.md`
4. Kết nối Frontend theo `frontend-setup.md` và `api-contract.md`

## 📊 Current Status

### ✅ Đã implement
- Authentication (JWT, refresh token)
- Anime CRUD
- Episodes CRUD
- Video streaming (S3, HLS support)
- Video transcoding (AWS MediaConvert)
- Permissions system

### ❌ Cần implement (xem `missing-features.md`)
- UserAnimeProgress (Lịch sử xem)
- Favorites (Yêu thích)
- Ratings (Đánh giá)
- Comments (Bình luận)
- User Profile Enhancement
- Advanced Search & Filter
- Recommendations
- Và nhiều tính năng khác...

