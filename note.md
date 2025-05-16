### 5.16非常好，我们可以分阶段来搭建一个酒水销售外卖平台的 Web 前端和后端 API。建议使用 Next.js 15 + Tailwind CSS + shadcn/ui 做前端界面，Next.js App Router（Server Actions）+ Prisma + PostgreSQL 做后端 API，是现代化开发中的主流组合，适合快速构建原型并持续扩展。

### 数据模型

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  password  String
  name      String?
  orders    Order[]
  createdAt DateTime @default(now())
}

model Product {
  id          String   @id @default(cuid())
  name        String
  description String
  price       Float
  imageUrl    String
  stock       Int
  createdAt   DateTime @default(now())
}

model Order {
  id        String     @id @default(cuid())
  user      User       @relation(fields: [userId], references: [id])
  userId    String
  total     Float
  status    String     @default("pending")
  createdAt DateTime   @default(now())
  items     OrderItem[]
}

model OrderItem {
  id        String   @id @default(cuid())
  order     Order    @relation(fields: [orderId], references: [id])
  orderId   String
  product   Product  @relation(fields: [productId], references: [id])
  productId String
  quantity  Int
}

## 项目目录结构建议
my-liquor-app/
├── app/
│   ├── layout.tsx
│   ├── page.tsx        # 首页（商品列表）
│   └── product/[id]/   # 商品详情页
├── components/         # UI组件
├── lib/                # 数据库、auth配置
├── actions/            # Server Actions API逻辑
├── prisma/             # schema.prisma
├── public/             # 静态资源
├── styles/             # Tailwind 样式
├── .env
└── ...
