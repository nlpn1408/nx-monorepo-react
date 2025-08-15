# 🚀 Hướng Dẫn Deploy Nx Monorepo React lên Vercel

## 📋 Tổng Quan

Dự án này sử dụng Nx monorepo với 3 ứng dụng React riêng biệt. Mỗi ứng dụng sẽ được deploy như một project Vercel riêng biệt để tối ưu performance và quản lý.

## 🏗️ Kiến Trúc Deploy

```
nx-monorepo-react/
├── admin-console    → admin-console.vercel.app
├── landing-page     → landing-page.vercel.app (hoặc domain chính)
└── shop-dashboard   → shop-dashboard.vercel.app
```

## ⚙️ Cấu Hình Đã Setup

### 1. **vercel.json** (Root level)
- Cấu hình build cho cả 3 ứng dụng
- Routing rules cho multi-app deployment

### 2. **Build Scripts**
- `npm run build:admin` - Build admin console
- `npm run build:landing` - Build landing page  
- `npm run build:shop` - Build shop dashboard
- `npm run build:all` - Build tất cả ứng dụng
- `npm run vercel-build` - Build script cho Vercel

### 3. **Package.json cho từng app**
- Mỗi app có `vercel-build` script riêng
- Sử dụng Nx build commands

## 🚀 Cách Deploy

### **Phương án 1: Deploy Từng App Riêng Biệt (Khuyến nghị)**

#### **Bước 1: Cài đặt Vercel CLI**
```bash
npm install -g vercel
```

#### **Bước 2: Login Vercel**
```bash
vercel login
```

#### **Bước 3: Deploy Admin Console**
```bash
cd apps/admin-console
vercel --prod
```

#### **Bước 4: Deploy Landing Page**
```bash
cd ../landing-page
vercel --prod
```

#### **Bước 5: Deploy Shop Dashboard**
```bash
cd ../shop-dashboard
vercel --prod
```

### **Phương án 2: Deploy qua Vercel Dashboard**

#### **Bước 1: Push code lên GitHub**
```bash
git add .
git commit -m "Add Vercel deployment configuration"
git push origin main
```

#### **Bước 2: Tạo Projects trên Vercel**

1. Truy cập [vercel.com](https://vercel.com)
2. Click "New Project"
3. Import repository của bạn
4. Tạo **3 projects riêng biệt**:

**Project 1: Admin Console**
- Project Name: `admin-console`
- Root Directory: `apps/admin-console`
- Build Command: `npm run vercel-build`
- Output Directory: `dist`

**Project 2: Landing Page**
- Project Name: `landing-page`
- Root Directory: `apps/landing-page`
- Build Command: `npm run vercel-build`
- Output Directory: `dist`

**Project 3: Shop Dashboard**
- Project Name: `shop-dashboard`
- Root Directory: `apps/shop-dashboard`
- Build Command: `npm run vercel-build`
- Output Directory: `dist`

### **Phương án 3: Vercel CLI với Root Directory**

Phương án này deploy từng app riêng biệt nhưng dùng Vercel CLI:

#### **Deploy Admin Console:**
```bash
cd apps/admin-console
vercel --prod
```

#### **Deploy Landing Page:**
```bash
cd apps/landing-page  
vercel --prod
```

#### **Deploy Shop Dashboard:**
```bash
cd apps/shop-dashboard
vercel --prod
```

**Mỗi app có file `vercel.json` riêng với cấu hình:**
- Build command: `cd ../.. && npx nx build <app-name>`
- Output directory: `../../dist/apps/<app-name>`
- Install command: `cd ../.. && npm install`

## 🔧 Environment Variables

Nếu cần thiết, thêm environment variables cho từng project:

```bash
# Local development
cp .env.example .env.local

# Vercel Dashboard
# Project Settings → Environment Variables
```

### Ví dụ Environment Variables:
```
VITE_API_URL=https://api.yourdomain.com
VITE_APP_NAME=Your App Name
VITE_ENVIRONMENT=production
```

## 🏷️ Custom Domains

### Setup Custom Domain:
1. Vercel Dashboard → Project → Settings → Domains
2. Add domain: `yourdomain.com`
3. Configure DNS:
   - `admin.yourdomain.com` → Admin Console
   - `shop.yourdomain.com` → Shop Dashboard
   - `yourdomain.com` → Landing Page

## 📊 Monitoring & Analytics

### Vercel Analytics:
```bash
npm install @vercel/analytics
```

Thêm vào `main.tsx` của từng app:
```typescript
import { Analytics } from '@vercel/analytics/react';

// Trong component App
<Analytics />
```

## 🚨 Troubleshooting

### **Lỗi Build**
```bash
# Kiểm tra build local trước
npm run build:all

# Xem logs chi tiết
vercel logs
```

### **Lỗi Dependencies**
- Đảm bảo `pnpm-lock.yaml` được commit
- Vercel sẽ tự detect package manager

### **Lỗi Routing**
- Kiểm tra `vercel.json` routes config
- Đảm bảo SPA routing được handle đúng

## 📈 Performance Tips

1. **Code Splitting**: Mỗi app được build riêng biệt
2. **Caching**: Vercel Edge caching tự động
3. **CDN**: Static assets qua Vercel CDN
4. **Bundle Analysis**: 
   ```bash
   npm run build:all -- --analyze
   ```

## 🔄 CI/CD Workflow

### GitHub Actions (Optional):
```yaml
name: Deploy to Vercel
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
```

## 📚 Tài Liệu Tham Khảo

- [Vercel Nx Documentation](https://vercel.com/guides/deploying-nx-monorepos)
- [Nx Deployment Guide](https://nx.dev/recipes/deployment)
- [Vercel CLI Reference](https://vercel.com/docs/cli)

---

## ✅ Checklist Deploy

- [ ] Build thành công local
- [ ] Push code lên repository
- [ ] Tạo Vercel projects
- [ ] Configure build settings
- [ ] Setup custom domains (nếu có)
- [ ] Test tất cả routes
- [ ] Setup monitoring
- [ ] Configure environment variables

🎉 **Chúc mừng! Dự án của bạn đã sẵn sàng deploy lên Vercel!**
