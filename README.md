# DVN Client - Phần mềm quản lý bán hàng

Ứng dụng frontend cho hệ thống quản lý bán hàng và hàng tồn kho DVN, được xây
dựng bằng React. Repository này hiện là **bộ khung khởi đầu của giao diện**:
trang chính đang hiển thị nội dung kiểm tra (`1`), còn `Header` và `Footer` là
các component placeholder. Các nghiệp vụ quản lý sản phẩm, kho, đơn hàng,
khách hàng và báo cáo chưa được triển khai trong phiên bản hiện tại.

> **Trạng thái hiện tại:** đang phát triển (scaffold). README này mô tả đúng
> những gì đã có trong mã nguồn và hướng dẫn để tiếp tục phát triển, không
> coi các module nghiệp vụ chưa có là chức năng đã hoàn thiện.

## Mục lục

- [Tổng quan](#tổng-quan)
- [Chức năng hiện có](#chức-năng-hiện-có)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Yêu cầu môi trường](#yêu-cầu-môi-trường)
- [Cài đặt và chạy dự án](#cài-đặt-và-chạy-dự-án)
- [Các lệnh npm scripts](#các-lệnh-npm-scripts)
- [Cấu hình biến môi trường](#cấu-hình-biến-môi-trường)
- [Build và triển khai](#build-và-triển-khai)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Định hướng phát triển chức năng](#định-hướng-phát-triển-chức-năng)
- [Kiểm thử và xử lý lỗi thường gặp](#kiểm-thử-và-xử-lý-lỗi-thường-gặp)
- [Đóng góp](#đóng-góp)

## Tổng quan

`qlhtk_dvn_client` là phần client chạy trên trình duyệt cho sản phẩm DVN.
Frontend được khởi tạo theo Create React App, sử dụng React Strict Mode và
Tailwind CSS làm nền tảng styling. Ứng dụng có thể xuất bản dưới dạng các file
tĩnh trong thư mục `build/`, phù hợp để phục vụ bằng Nginx, Apache hoặc các
nền tảng static hosting.

## Chức năng hiện có

### Đã có trong mã nguồn

- Khởi tạo ứng dụng React bằng `react-dom/client`.
- Có điểm vào ứng dụng tại `src/index.js`.
- Có component nền `App`, `Header` và `Footer`.
- Có thư mục tiện ích cho:
  - Tính thuế: `src/utils/calculateTax.js`.
  - Định dạng ngày: `src/utils/formatDate.js`.
  - Kiểm tra email: `src/utils/validateEmail.js`.
- Có tài nguyên nhận diện DVN trong `public/` và `src/assets/image/`.
- Có cấu hình SPA fallback cho Apache (`public/.htaccess`), Netlify
  (`public/_redirects`) và Nginx (`nginx.conf`).
- Có manifest để hỗ trợ cài đặt ứng dụng dạng PWA; service worker hiện có trong
  `public/` nhưng chưa được đăng ký trong entry point.
- Build production tắt source map thông qua `GENERATE_SOURCEMAP=false`.
- Có thể obfuscate JavaScript sau build khi bật `ENABLE_OBFUSCATION=true`.

### Chưa triển khai

Các chức năng dưới đây mới là phạm vi nghiệp vụ dự kiến, chưa có màn hình,
route, API hoặc lưu trữ dữ liệu tương ứng:

- Đăng nhập, phân quyền và quản lý phiên làm việc.
- Dashboard và các chỉ số doanh thu/tồn kho.
- CRUD sản phẩm, danh mục, đơn vị tính, nhà cung cấp.
- Nhập kho, xuất kho, điều chỉnh và kiểm kê tồn kho.
- Bán hàng, giỏ hàng, hóa đơn và trạng thái thanh toán.
- Quản lý khách hàng và lịch sử giao dịch.
- Báo cáo doanh thu, lợi nhuận, nhập - xuất - tồn.
- Kết nối backend/API, xử lý loading/error và phân trang.

## Công nghệ sử dụng

| Thành phần | Công nghệ |
| --- | --- |
| UI | React 19 |
| Khởi tạo/build | Create React App, `react-scripts` 5 |
| Định tuyến (đã cài sẵn) | `react-router-dom` 7 |
| Icon (đã cài sẵn) | `react-icons` |
| CSS | Tailwind CSS 3, PostCSS, Autoprefixer |
| Kiểm thử | Jest và React Testing Library |
| Đo hiệu năng | `web-vitals` |
| Package manager khuyến nghị | pnpm |

## Yêu cầu môi trường

- Node.js phiên bản LTS (khuyến nghị Node.js 20 hoặc mới hơn).
- pnpm phiên bản 8 trở lên. Có thể dùng npm nếu cần, nhưng nên ưu tiên
  `pnpm-lock.yaml` để kết quả cài đặt nhất quán.
- Git.

Kiểm tra phiên bản:

```bash
node --version
pnpm --version
```

## Cài đặt và chạy dự án

### 1. Clone repository

```bash
git clone <repository-url>
cd qlhtk_dvn_client
```

### 2. Cài dependency

Lệnh khuyến nghị:

```bash
pnpm install --frozen-lockfile
```

Nếu môi trường không có pnpm:

```bash
npm install
```

### 3. Khởi động môi trường phát triển

```bash
pnpm start
```

Mở [http://localhost:3000](http://localhost:3000). Dev server hỗ trợ
hot reload; thay đổi trong `src/` sẽ được cập nhật tự động.

## Các lệnh npm scripts

Các lệnh dưới đây có thể chạy bằng `pnpm <lệnh>` hoặc `npm run <lệnh>`:

| Lệnh | Mục đích |
| --- | --- |
| `start` | Chạy dev server tại cổng 3000 |
| `build` | Tạo bản production trong `build/` |
| `test` | Chạy Jest/React Testing Library ở chế độ watch |
| `eject` | Eject cấu hình Create React App; chỉ dùng khi thực sự cần |

Ví dụ:

```bash
pnpm test
pnpm build
```

Chạy test một lần trong CI hoặc terminal không tương tác:

```bash
CI=true pnpm test --watchAll=false
```

## Cấu hình biến môi trường

Hiện tại source code chưa đọc biến môi trường nào và file `.env.local` không
chứa cấu hình bắt buộc. Khi tích hợp backend, có thể bổ sung biến theo quy ước
của Create React App, ví dụ:

```dotenv
REACT_APP_API_URL=https://api.example.com
```

Chỉ các biến có tiền tố `REACT_APP_` được đưa vào bundle của Create React App.
Không đặt mật khẩu, private key, token bí mật hoặc thông tin nhạy cảm trong
các biến `REACT_APP_*`, vì chúng có thể được người dùng xem trong trình duyệt.

## Build và triển khai

### Build production

```bash
pnpm build
```

Kết quả nằm trong thư mục `build/`. Script `postbuild` luôn được gọi sau build,
nhưng mặc định **bỏ qua obfuscation**:

```text
[postbuild] Skip obfuscation. Set ENABLE_OBFUSCATION=true to enable.
```

### Obfuscate JavaScript (tùy chọn)

```bash
set ENABLE_OBFUSCATION=true
pnpm build
```

Trên PowerShell:

```powershell
$env:ENABLE_OBFUSCATION = "true"
pnpm build
```

Khi bật tùy chọn này, script `scripts/postbuild-obfuscate.js` gọi
`javascript-obfuscator` thông qua `npx` trên các file JavaScript trong
`build/static/js`. Việc obfuscate làm việc debug khó hơn và không thay thế
biện pháp bảo vệ secret ở backend.

### Nginx

Đưa toàn bộ nội dung `build/` vào `/usr/share/nginx/html` và dùng cấu hình
trong [`nginx.conf`](./nginx.conf). Quy tắc `try_files` chuyển các URL
client-side chưa phải file tĩnh về `index.html`, cần thiết cho SPA routing.

Ví dụ Docker image tĩnh tối giản:

```dockerfile
FROM nginx:alpine
COPY build/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

### Apache

Copy nội dung `build/` lên document root. File
[`public/.htaccess`](./public/.htaccess) cần được giữ lại trong document root
để các route của SPA fallback về `index.html`. Apache phải bật
`mod_rewrite` và cho phép override bằng `.htaccess`.

### Netlify

File [`public/_redirects`](./public/_redirects) chứa rule:

```text
/*    /index.html   200
```

Nếu deploy bằng Netlify, đặt thư mục publish là `build` và giữ rule này trong
output build (có thể copy `_redirects` vào thư mục build trong pipeline nếu
nền tảng không tự copy tài nguyên public).

## Cấu trúc thư mục

```text
.
├── public/
│   ├── .htaccess              # SPA fallback cho Apache
│   ├── _redirects             # SPA fallback cho Netlify
│   ├── index.html             # HTML shell
│   ├── manifest.json          # Metadata/PWA icons
│   ├── service-worker.js      # Service worker hiện có
│   └── logo*, favicon*         # Tài nguyên thương hiệu
├── scripts/
│   └── postbuild-obfuscate.js # Obfuscate JS khi được bật
├── src/
│   ├── App.js                 # Root component hiện tại
│   ├── index.js               # React entry point
│   ├── index.css              # Tailwind entry point
│   ├── components/
│   │   ├── header/
│   │   └── footer/
│   ├── utils/                 # Các hàm tiện ích dùng chung
│   └── assets/image/          # Ảnh dùng trong source
├── nginx.conf                 # Cấu hình Nginx cho SPA
├── package.json
├── pnpm-lock.yaml
└── tailwind.config.js
```

## Định hướng phát triển chức năng

Để phát triển từ scaffold hiện tại thành phần mềm quản lý bán hàng hoàn chỉnh,
có thể triển khai theo thứ tự:

1. Chuẩn hóa layout chung: header, sidebar, footer, trạng thái loading và
   thông báo lỗi.
2. Bổ sung router và route bảo vệ cho đăng nhập/phân quyền.
3. Khai báo lớp gọi API và cấu hình `REACT_APP_API_URL`.
4. Xây dựng danh mục dữ liệu nền: sản phẩm, danh mục, đơn vị tính, nhà cung
   cấp và khách hàng.
5. Xây dựng nghiệp vụ nhập/xuất/điều chỉnh/kiểm kê và cập nhật tồn kho.
6. Xây dựng bán hàng, hóa đơn, thanh toán và lịch sử giao dịch.
7. Thêm dashboard, bộ lọc, phân trang, export và báo cáo.
8. Bổ sung test cho utility, component, route và các luồng nghiệp vụ chính.

Mỗi module nên có component, route, kiểu dữ liệu, lớp API và test riêng; không
đưa token hoặc logic nghiệp vụ cần bảo mật vào frontend.

## Kiểm thử và xử lý lỗi thường gặp

### Cổng 3000 đã được sử dụng

Khi chạy `pnpm start`, CRA sẽ hỏi có chuyển sang cổng khác hay không. Chọn
đồng ý hoặc dừng tiến trình đang dùng cổng 3000.

### Trang con bị 404 sau khi refresh

Đây là lỗi cấu hình SPA fallback. Kiểm tra `try_files` trong
[`nginx.conf`](./nginx.conf), `mod_rewrite` của Apache hoặc rule `_redirects`
trên nền tảng hosting.

### Build không chạy

Xóa `node_modules`, cài lại dependency theo lockfile rồi chạy lại:

```bash
pnpm install --frozen-lockfile
pnpm build
```

Không xóa hoặc chỉnh sửa `pnpm-lock.yaml` tùy tiện vì lockfile giúp đồng bộ
phiên bản dependency giữa các môi trường.

### Obfuscation thất bại

Obfuscation là bước tùy chọn. Có thể build bản thông thường bằng cách bỏ biến
`ENABLE_OBFUSCATION` hoặc đặt biến về `false`. Nếu cần obfuscate, bảo đảm máy
được phép dùng `npx` và có thể tải package `javascript-obfuscator`.

## Đóng góp

1. Tạo branch theo phạm vi thay đổi, ví dụ `feature/product-management`.
2. Cài dependency từ lockfile và chạy test trước khi mở pull request.
3. Giữ thay đổi tập trung, cập nhật README khi thêm script, biến môi trường,
   route hoặc module nghiệp vụ mới.
4. Kiểm tra tối thiểu bằng:

   ```bash
   pnpm test -- --watchAll=false
   pnpm build
   ```

5. Mô tả rõ thay đổi, cách kiểm thử và ảnh hưởng triển khai trong pull request.

## Bản quyền

Chưa có thông tin giấy phép được khai báo trong repository. Không phân phối lại
source code hoặc tài nguyên thương hiệu nếu chưa được chủ sở hữu dự án cho
phép.
