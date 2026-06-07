# Câu A1 (5đ) — Viewport & Mobile-First
1. Viết chính xác thẻ <meta viewport> chuẩn. Giải thích từng thuộc tính.
<!-- Trong <head> — PHẢI CÓ -->
`<meta name="viewport" content="width=device-width, initial-scale=1.0">`
* Giải thích : 
 - width=device-width
    + Đặt chiều rộng trang bằng đúng chiều rộng màn hình thiết bị (iPhone, Android, tablet...).
    + Không bị “thu nhỏ” như trang desktop.
 - initial-scale=1.0
    + Mức zoom ban đầu khi mở trang là 100%.
    + Không phóng to hoặc thu nhỏ mặc định.

2. Nếu thiếu thẻ viewport thì iPhone hiển thị thế nào?
 - iPhone giả định trang rộng 980px (như desktop) → thu nhỏ lại → chữ bé xíu → UX tệ.

3. Mobile-First và Desktop-First khác nhau thế nào? Viết ví dụ CSS cho mỗi cách với breakpoint 768px. Tại sao Mobile-First được khuyên dùng?
* Mobile-First (khuyến nghị ✅): Viết CSS cho mobile trước, sau đó mở rộng cho màn hình lớn
 - Ví dụ: 
 /* 1. Code cho mobile trước (không cần @media) */
.product-grid {
    display: grid;
    grid-template-columns: 1fr;     /* 1 cột trên mobile */
    gap: 16px;
}

/* 2. Thêm complexity khi màn hình rộng hơn */
@media (min-width: 576px) {
    .product-grid {
        grid-template-columns: repeat(2, 1fr);   /* 2 cột tablet nhỏ */
    }
}

@media (min-width: 992px) {
    .product-grid {
        grid-template-columns: repeat(3, 1fr);   /* 3 cột desktop */
    }
}

@media (min-width: 1200px) {
    .product-grid {
        grid-template-columns: repeat(4, 1fr);   /* 4 cột desktop lớn */
    }
}

* Desktop-First (cách cũ ❌): Viết CSS cho desktop trước, sau đó thu nhỏ cho mobile.
 - Ví dụ: 
 .product-grid { grid-template-columns: repeat(4, 1fr); }
 @media (max-width: 1200px) { /* 3 cột */ }
 @media (max-width: 992px)  { /* 2 cột */ }
 @media (max-width: 576px)  { /* 1 cột */ }

* Lý do Mobile-First tốt hơn:
 - Mobile tải ít CSS hơn (mobile chỉ tải mobile styles, không download desktop styles)
 - Buộc bạn ưu tiên nội dung quan trọng trước (content thinking)
 - Google và performance tools đánh giá cao hơn

# Câu A2 (5đ) — Breakpoints
Breakpoints chuẩn
| Breakpoint        | Kích thước | Thiết bị đại diện                       | Ví dụ lưới sản phẩm |
| ----------------- | ---------- | --------------------------------------- | ------------------- |
| Mobile **xs**     | `< 576px`  | Điện thoại nhỏ (iPhone SE, Android nhỏ) | **1 cột**           |
|  Mobile L **sm**  | `≥ 576px`  | Điện thoại lớn                          | **1–2 cột**         |
| Tablet **md**     | `≥ 768px`  | Tablet dọc (iPad mini, tablet Android)  | **2–3 cột**         |
| Desktop **lg**    | `≥ 992px`  | Laptop nhỏ                              | **3 cột**           |
| Desktop L **xl**  | `≥ 1200px` | Laptop lớn / desktop                    | **4 cột**           |
| Desktop XL **xxl**| `≥ 1400px` | Màn hình lớn (4K, ultrawide)            | **4–6 cột**         |

# Câu A3 (5đ) — Media Queries

| Chiều rộng màn hình   | `.container` width |
| --------------------- | ------------------ |
| **375px (iPhone SE)** | **100%**           |
| **600px**             | **540px**          |
| **800px**             | **720px**          |
| **1000px**            | **960px**          |
| **1400px**            | **1140px**         |

# Câu A4 (5đ) — SCSS Basics

1. Variables ($primary-color): 
 - Dùng để lưu giá trị dùng lại nhiều lần (màu sắc, font, spacing...) (ở đây là màu chính)
 - Ví dụ:
 
 $primary-color: #7c3aed;
 $font-size: 16px;

 button {
   background: $primary-color;
  font-size: $font-size;
 }

 - Ý nghĩa:
    + Đổi 1 chỗ → toàn bộ thay đổi
    + Giống “biến” trong lập trình

2. Nesting (viết CSS lồng nhau)
 - Viết CSS theo cấu trúc HTML, dễ đọc hơn
 - Ví dụ:
 
 // SCSS — rõ ràng, dễ đọc
.navbar {
    background: #1a202c;
    padding: $space-4;
    display: flex;
    align-items: center;
    justify-content: space-between;

    // & = tham chiếu đến selector cha (.navbar)
    &__logo {
        color: white;
        font-size: $font-size-lg;
        font-weight: 700;
        text-decoration: none;
    }

    &__links {
        display: flex;
        gap: $space-6;
        list-style: none;
    }

    &__item {
        &__link {
            color: rgba(white, 0.8);
            text-decoration: none;
            transition: color $transition-base;

            &:hover {
                color: white;
            }

            &--active {
                color: $color-primary;
                font-weight: 600;
            }
        }
    }
}
 - Ý nghĩa:
    + Gọn hơn CSS thường
    + Dễ nhìn quan hệ cha–con

3. Mixins (@mixin, @include)
 - Tạo “hàm CSS” để tái sử dụng code
 - Ví dụ: 
 @mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.box {
  @include flex-center;
  height: 200px;
}
 - Ý nghĩa:
    + Giảm lặp code
    + Dùng như function trong lập trình

4. @extend / Inheritance
 - Cho phép một class kế thừa style của class khác
 - ví dụ: 
 .btn {
  padding: 10px;
  border-radius: 6px;
  border: none;
}

.btn-primary {
  @extend .btn;
  background: blue;
} 

.btn-danger {
  @extend .btn;
  background: red;
}git pull origin main --rebase
 - Ý nghĩa:
    + Tái sử dụng style
    + Tránh viết lại CSS giống nhau

* Vì sao browser KHÔNG đọc được .scss?
 - Vì:    
    + Trình duyệt chỉ hiểu: HTML, CSS, JavaScript
    + .scss là ngôn ngữ tiền xử lý (preprocessor)
    + Có syntax đặc biệt: $variable, @mixin, nesting... -> Browser không hiểu các cú pháp này

* Các bước để chuyển SCSS → CSS
 - VS Code + Live Sass Compiler extension
   → Click "Watch Sass" ở status bar
   → Tự compile mỗi khi save

 - Vite (React/Vue project):
   npm install -D sass
   → Vite tự detect và compile .scss files 

 - Node.js script:
   npm install -D sass
   npx sass styles/main.scss styles/main.css --watch

 - Create React App:
   npm install sass
   → Đổi .css thành .scss → tự compile


# Bài B3 — SCSS Refactor

- Compiled: `scss/style.scss` → `style.css`
- Output file: `PBT_05/style.css`
- Time: 2026-05-27
- Result: SUCCESS

```bash
sass scss/style.scss style.css --style=compressed
```

# Câu C1 — Phân tích trang web thực
Chọn trang: YouTube

1. Mobile 375px
 - Navigation: xuất hiện hamburger trái, logo YouTube nhỏ, nút tìm kiếm, nút tạo, avatar.
 - Lưới content: 1 cột chính, feed video xếp dọc.
 - Elements ẩn: sidebar bên trái (Home/Shorts/Subscriptions…) và nhiều nút menu lớn, thanh  category đầy đủ.
 - Font: nhỏ hơn, chữ và nút hiển thị gọn hơn.

2. Tablet 768px
 - Navigation: vẫn có hamburger, nhưng thấy nhiều nút hơn ở top; thanh tìm kiếm rộng hơn, list category xuất hiện rõ hơn.
 - Lưới content: lưới nội dung bắt đầu mở rộng, ví dụ phần Shorts có 3 cards ngang; feed chính rộng hơn.
 - Elements ẩn: vẫn chưa hiện sidebar trái đầy đủ như desktop.
 - Font: lớn hơn mobile, khoảng cách thoáng hơn.

3. Desktop 1440px
 - Navigation: hiện đầy đủ sidebar bên trái, top bar rộng với thanh tìm kiếm, category pills, nút tạo, thông báo, avatar.
 - Lưới content: nhiều cột hơn, phần Shorts hiển thị nhiều thumbnails ngang, feed chính có dạng đa cột rộng.
 - Elements bị ẩn trên mobile: sidebar điều hướng, rất nhiều nút và tab phụ, các menu mở rộng.
 - Font: lớn nhất, hiển thị thoải mái hơn trên màn hình rộng.

# Câu C2 — Thiết kế Responsive Strategy

Mobile:
- Layout 1 cột.
- Header gồm logo bên trái và số điện thoại đặt bàn bên phải.
- Hero image full-width trên cùng.
- Form đặt bàn xuất hiện ngay sau hero để người dùng đặt nhanh.
- 6 ảnh món ăn xếp dọc 1 cột.
- Map có thể được đặt cuối trang hoặc ẩn vào nút "Xem bản đồ" để giảm chiều cao trang.
- Ẩn trên mobile: sidebar phụ, menu điều hướng rộng, nội dung quảng cáo không quan trọng.

Tablet:
- Layout chuyển sang 2 cột cho một số phần.
- Header vẫn hiển thị logo + hotline nhưng rộng hơn.
- Gallery ảnh xếp **2 cột**.
- Form đặt bàn có thể đứng dưới hero hoặc nằm ở bên phải nếu đủ rộng.
- Bản đồ Google Maps hiển thị phía dưới gallery hoặc cạnh form nếu đủ ngang.
- Ẩn trên tablet: các thanh chức năng desktop rộng, nhiều chi tiết phụ không cần thiết.

Desktop:
- Layout 2 cột rõ ràng, có sidebar có thể dùng cho form + map.
- Hero image full-width trên cùng.
- Gallery ảnh xếp **3 cột** cho 6 món ăn.
- Booking form và Google Maps nằm ở cột bên phải (sidebar) để giữ nội dung chính bên trái.
- Footer full-width dưới cùng.

CSS skeleton mobile-first (layout only):
- Mobile: 1 cột, gallery 1 cột, form ngay dưới hero.
- Tablet: gallery 2 cột, map vẫn hiển thị, layout mềm dẻo.
- Desktop: layout 2 cột với sidebar chứa form + map, gallery 3 cột.
- Dùng `display: grid` cho `.page`, `.gallery`, `.booking-form` và `@media (min-width: 768px)` / `@media (min-width: 1024px)`.

File skeleton: `PBT_05/c2-skeleton.css`











