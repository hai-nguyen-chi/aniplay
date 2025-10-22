# Hướng dẫn Setup NextJS Frontend

## Khởi tạo dự án
```bash
npx create-next-app@latest frontend --typescript --tailwind --eslint
cd frontend
npm run dev
```

## Cấu trúc thư mục gợi ý
```
frontend/
├─ src/
│  ├─ app/
│  ├─ components/
│  ├─ context/
│  ├─ lib/
│  └─ styles/
└─ .env.local
```

## Env
```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001
```

## Axios client (src/lib/apiClient.ts)
- Tạo instance với baseURL từ env, interceptor gắn Authorization token

## Auth Context & Protected component
- Lưu token/user trong localStorage, redirect login khi chưa auth

## Trang chính
- /, /anime, /anime/[id], /anime/[id]/watch/[episode], /auth/login, /auth/register, /profile

Xem chi tiết trong `api-contract.md` để gọi API khớp contract.
