# Frontend E2E Checklist (NextJS)

## Chuẩn bị
- Tạo project NextJS + Tailwind
- `.env.local` với NEXT_PUBLIC_API_BASE_URL

## Cơ sở
- App Router, layout, styles
- Axios client, AuthContext, Protected

## Trang
- /, /anime, /anime/[id], /anime/[id]/watch/[episode]
- /auth/login, /auth/register, /profile

## Tích hợp API
- Theo `api-contract.md` (auth, anime, episodes, rating, comments, favorites, progress)

## UI/UX
- Header/Nav, responsive grid, skeletons, toasts, dark mode (tuỳ chọn)

## State/Caching
- SWR hoặc React Query, cache theo query string, invalidate sau mutate

## Bảo vệ route
- Ẩn/hạn chế hành động khi chưa đăng nhập, redirect login

## Build/Start
```bash
npm run build
npm run start
```

## Deploy
- Vercel/Netlify + CORS backend
