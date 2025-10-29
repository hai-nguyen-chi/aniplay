# Hướng dẫn Setup NestJS (Quickstart)

## Tạo dự án và cài đặt phụ thuộc
```bash
npx @nestjs/cli new backend -p npm
cd backend
npm install @nestjs/mongoose mongoose
npm install @nestjs/jwt @nestjs/passport passport-jwt
npm install bcryptjs class-validator class-transformer
npm install @nestjs/throttler
npm install -D @types/bcryptjs
```

## .env
```env
MONGODB_URI=mongodb://localhost:27017/aniplay_db
JWT_SECRET=supersecret
JWT_EXPIRES_IN=15m
REFRESH_JWT_SECRET=supersecret_refresh
REFRESH_JWT_EXPIRES_IN=7d
PORT=3001
```

## Kết nối MongoDB
```ts
// src/app.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';

@Module({
  imports: [
    MongooseModule.forRoot(process.env.MONGODB_URI ?? 'mongodb://localhost:27017/aniplay_db'),
  ],
})
export class AppModule {}
```

## ValidationPipe, Interceptors, CORS
```ts
// src/main.ts
import { ValidationPipe, ClassSerializerInterceptor } from '@nestjs/common';
import { NestFactory, Reflector } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }));
  const reflector = app.get(Reflector);
  app.useGlobalInterceptors(new ClassSerializerInterceptor(reflector));
  app.enableCors({ origin: [/localhost:\\d+$/] });
  await app.listen(process.env.PORT || 3001);
}
bootstrap();
```

## Bước tiếp theo
- Auth đã bao gồm: login, refresh, logout, me; JwtStrategy load permissions từ DB mỗi request
- Thêm Anime/Episode và các module còn lại
- Xem thêm trong `backend-contracts.md`, `backend-models.md`, `backend-security.md`

## Cấu trúc thư mục (gợi ý)
```txt
backend/
├─ src/
│  ├─ main.ts
│  ├─ app.module.ts
│  ├─ config/
│  │  ├─ configuration.ts
│  │  ├─ validation.ts
│  │  └─ swagger.ts
│  ├─ common/
│  │  ├─ decorators/roles.decorator.ts
│  │  ├─ guards/
│  │  │  ├─ jwt-auth.guard.ts
│  │  │  └─ roles.guard.ts
│  │  ├─ interceptors/
│  │  │  ├─ logging.interceptor.ts
│  │  │  └─ transform.interceptor.ts
│  │  ├─ filters/
│  │  ├─ pipes/
│  │  └─ utils/
│  ├─ database/
│  │  ├─ database.module.ts
│  │  ├─ database.service.ts
│  │  └─ seeds/
│  ├─ auth/
│  │  ├─ auth.module.ts
│  │  ├─ auth.controller.ts
│  │  ├─ auth.service.ts
│  │  ├─ jwt.strategy.ts
│  │  └─ dto/auth.dto.ts
│  ├─ users/
│  │  ├─ users.module.ts
│  │  ├─ users.service.ts
│  │  ├─ users.controller.ts
│  │  └─ schemas/user.schema.ts
│  ├─ anime/
│  │  ├─ anime.module.ts
│  │  ├─ anime.service.ts
│  │  ├─ anime.controller.ts
│  │  ├─ dto/anime.dto.ts
│  │  └─ schemas/anime.schema.ts
│  ├─ episodes/
│  │  ├─ episodes.module.ts
│  │  ├─ episodes.service.ts
│  │  ├─ episodes.controller.ts
│  │  ├─ dto/episode.dto.ts
│  │  └─ schemas/episode.schema.ts
│  ├─ ratings/
│  │  ├─ ratings.module.ts
│  │  ├─ ratings.service.ts
│  │  ├─ ratings.controller.ts
│  │  └─ schemas/rating.schema.ts
│  ├─ comments/
│  │  ├─ comments.module.ts
│  │  ├─ comments.service.ts
│  │  ├─ comments.controller.ts
│  │  └─ schemas/comment.schema.ts
│  ├─ favorites/
│  │  ├─ favorites.module.ts
│  │  ├─ favorites.service.ts
│  │  ├─ favorites.controller.ts
│  │  └─ schemas/favorite.schema.ts
│  ├─ progress/
│  │  ├─ progress.module.ts
│  │  ├─ progress.service.ts
│  │  ├─ progress.controller.ts
│  │  └─ schemas/progress.schema.ts
│  └─ shared/
│     ├─ dto/common.dto.ts
│     └─ types/paged.ts
├─ test/
├─ tsconfig.json
├─ tsconfig.build.json
├─ nest-cli.json
└─ package.json
```