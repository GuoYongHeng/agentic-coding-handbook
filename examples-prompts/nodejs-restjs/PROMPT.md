# NestJS 企业级后端助手

## 使命
使用最新的企业标准创建生产就绪的 NestJS 后端应用，采用六边形/整洁架构。所有工具必须经过配置、测试，并在零配置部署环境下验证可正常工作。

---

## 强制 WEB 验证
**在任何安装之前，请访问官方文档：**
1. **最新版本：** NestJS (https://docs.nestjs.com), TypeORM (https://typeorm.io), Prisma (https://www.prisma.io/docs), Swagger (https://docs.nestjs.com/openapi/introduction)
2. **Node.js 兼容性：** 查看 GitHub 上最新 NestJS、TypeORM、Prisma 和 JWT 包的 package.json，确定兼容的 Node.js 版本
3. **使用各包官方文档中的精确安装命令**
4. **关键：确定兼容版本** - 使用与所有包兼容的最高版本，不一定是 @latest

---

## 项目设置

### 初始化与验证
```bash
npm i -g @nestjs/cli@latest
nest new my-backend-api --package-manager npm
cd my-backend-api
# 验证 Node.js 版本与所有计划包的兼容性
```

### 包安装 - 遵循官方文档
**核心框架：** @nestjs/common, @nestjs/core, @nestjs/platform-express, @nestjs/config, @nestjs/mapped-types
**数据库：** @nestjs/typeorm + typeorm + pg (PostgreSQL) 或 prisma + @prisma/client + @nestjs/prisma
**认证：** @nestjs/jwt, @nestjs/passport, passport-jwt, passport-local, bcrypt, @types/bcrypt
**验证：** class-validator, class-transformer, @nestjs/throttler
**文档：** @nestjs/swagger, swagger-ui-express
**安全：** helmet, @nestjs/throttler, cookie-parser, express-rate-limit
**测试：** @nestjs/testing, supertest, @types/supertest
**开发工具：** eslint, prettier, @typescript-eslint/eslint-plugin, @typescript-eslint/parser, jest
**工具库：** lodash, @types/lodash, uuid, @types/uuid, moment

---

## 六边形架构

### 文件夹结构
```
src/
├── common/
│   ├── decorators/     # 自定义装饰器（角色、公开等）
│   ├── filters/        # 异常过滤器
│   ├── guards/         # 认证、角色、限流守卫
│   ├── interceptors/   # 日志、转换拦截器
│   ├── pipes/          # 验证管道
│   └── types/          # 共享接口和类型
├── config/
│   ├── database.config.ts    # 数据库配置
│   ├── jwt.config.ts         # JWT 设置
│   └── app.config.ts         # 全局应用配置
├── modules/
│   ├── auth/
│   │   ├── domain/
│   │   │   ├── entities/     # 用户、Token 实体
│   │   │   ├── repositories/ # 认证仓储接口
│   │   │   └── services/     # 领域业务逻辑
│   │   ├── application/
│   │   │   ├── dto/          # 请求/响应 DTO
│   │   │   ├── use-cases/    # 应用服务
│   │   │   └── commands/     # CQRS 命令（可选）
│   │   ├── infrastructure/
│   │   │   ├── repositories/ # TypeORM/Prisma 实现
│   │   │   ├── adapters/     # 外部服务适配器
│   │   │   └── persistence/  # 数据库 Schema
│   │   └── presentation/
│   │       ├── controllers/  # REST 控制器
│   │       └── guards/       # 模块专属守卫
│   └── users/
│       ├── domain/
│       ├── application/
│       ├── infrastructure/
│       └── presentation/
├── shared/
│   ├── database/       # 数据库连接、迁移
│   ├── interfaces/     # 全局接口
│   └── utils/          # 辅助函数
└── main.ts
```

### 架构规范
✅ **应做：** 层次分离、依赖注入、基于接口的仓储
❌ **避免：** 在控制器中直接调用数据库，在 DTO 中编写业务逻辑

---

## 配置与功能

### 必需核心模块
- **认证：** JWT 策略、登录/注册、密码重置、邮箱验证
- **授权：** 基于角色的访问控制（RBAC）、守卫、装饰器
- **用户：** CRUD 操作、个人资料管理、软删除
- **健康检查：** 包含数据库连接的健康检查端点
- **文档：** 包含认证 Schema 的 Swagger

### 数据库集成
- **TypeORM 设置：** 实体、仓储、迁移、种子数据
- **Prisma 替代方案：** Schema、客户端生成、迁移
- **连接：** 数据库连接池、事务支持
- **验证：** 使用 class-validator 进行实体级验证

### 安全实现
- **JWT：** 访问/刷新 Token 机制
- **密码：** 带盐的 Bcrypt 哈希
- **限流：** API 限速配置
- **请求头：** Helmet 安全中间件
- **CORS：** 跨域配置
- **验证：** 请求/响应验证管道

---

## 验证

### 自动化检查（所有检查必须通过）
```bash
npm run start:dev     # 开发服务器在 3000 端口启动
npm run build         # 生产构建成功
npm run test          # 单元测试通过，0 个失败
npm run test:e2e      # E2E 测试通过，0 个失败
npm run lint          # 零 ESLint 错误/警告
npm run format        # Prettier 格式化检查
```

### API 端点测试
```bash
# 健康检查响应
curl http://localhost:3000/health

# Swagger 文档加载
curl http://localhost:3000/api

# 认证端点可用
POST /auth/register
POST /auth/login
GET /auth/profile (with JWT)
```

### 数据库验证
```bash
# 迁移成功运行
npm run migration:run

# 种子数据无错误执行
npm run seed:run

# 数据库连接已建立
# 检查应用日志确认连接成功
```

---

## 交付物

### 实现需求
1. **安装步骤**，包含来自官方 NestJS 文档的精确命令
2. **完整认证模块**（领域层、应用层、基础设施层、展示层）
3. **用户管理**，包含 CRUD 操作和验证
4. **数据库设置**，包含实体/模型、迁移和种子数据
5. **安全配置**（JWT、RBAC、限流、CORS）
6. **API 文档**，包含 Swagger 装饰器和示例
7. **全局中间件**（异常过滤器、验证管道、拦截器）
8. **Docker 配置**（Dockerfile + 包含 PostgreSQL 的 docker-compose.yml）
9. **环境配置**（开发/预发布/生产的 .env 文件）
10. **测试设置**（单元测试、E2E 测试、测试数据库）

### 配置文件
- **TypeScript：** 启用严格模式的 tsconfig.json
- **ESLint：** 包含 NestJS 规则的 .eslintrc.js
- **Prettier：** 保持一致格式化的 .prettierrc
- **Jest：** 用于测试的 jest.config.js
- **Docker：** 针对生产环境优化的多阶段 Dockerfile

### 代码示例
- **JWT 认证：** 完整的登录/注册流程
- **RBAC 系统：** 角色装饰器和守卫
- **仓储模式：** 使用 TypeORM/Prisma 的通用基础仓储
- **异常处理：** 包含结构化错误响应的全局过滤器
- **验证：** 使用 class-validator 装饰器的 DTO 类

---

## 成功标准
**生产就绪的 NestJS 后端，具备六边形架构、完整认证、数据库集成、API 文档和 Docker 部署——随时可供团队协作使用。**

### 质量标准
- 启用 TypeScript 严格模式
- 零构建/代码检查/测试错误
- 实现所有安全最佳实践
- 完整的 API 文档
- 正确配置数据库及迁移
- Docker 容器成功启动
- 所有端点均已安全防护并完成验证

**质量保证：** 在所有验证检查通过且 API 端点以正确的身份验证正常响应之前，任务未完成。
