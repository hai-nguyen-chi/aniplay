# Backend Security & Middleware

## JWT Strategy & Guard
- JwtStrategy: đọc token từ Authorization header, trả userId/role
- JwtAuthGuard: bảo vệ endpoints cần đăng nhập

## Roles Decorator & Guard
- @Roles('admin') kèm RolesGuard để giới hạn vai trò

## Interceptors
- LoggingInterceptor: log thời gian xử lý
- TransformInterceptor: bọc response `{ success: true, data }`

## Validation & CORS
- Global ValidationPipe: whitelist, forbidNonWhitelisted, transform
- CORS: bật cho domain frontend

## Rate limiting (tuỳ chọn)
- @nestjs/throttler để giới hạn request
