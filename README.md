# Dịch nhanh

Web app dịch siêu nhẹ cho điện thoại. Gọi thẳng Groq từ trình duyệt, không backend, không build.

## Cấu trúc

```
index.html     Toàn bộ giao diện + CSS + JS (1 file cho nhanh)
sw.js          Service worker: chỉ cache giao diện, không cache request tới Groq
manifest.json  Thông tin PWA để cài lên màn hình chính
icon.svg       Icon app
```

## Lấy key Groq

1. Vào https://console.groq.com/keys, đăng nhập.
2. Bấm **Create API Key**, copy key (dạng `gsk_…`).

## Dán key trong app

Mở app → bấm bánh răng → dán key → **Lưu** → **Thử key** để kiểm tra.
Key chỉ lưu trong localStorage của máy đó, không gửi đi đâu ngoài Groq.

Model cố định trong code: `openai/gpt-oss-120b` (production, free tier dùng được, dịch tiếng Việt tốt; app tự đặt mức suy nghĩ thấp cho nhanh). Nếu Groq ngừng model này, app tự chuyển sang `openai/gpt-oss-20b`. Muốn đổi thì sửa mảng `MODELS` trong `index.html`.

## Chạy thử local

```bash
npx serve .
```

Rồi mở địa chỉ nó in ra (VD http://localhost:3000). Service worker cần `http://localhost` hoặc `https`, mở file trực tiếp (`file://`) thì vẫn dịch được nhưng không cài PWA được.

## Đưa lên host

App là file tĩnh thuần, dùng đường dẫn tương đối. Bỏ cả thư mục lên bất kỳ static host nào (Netlify, Vercel, GitHub Pages, Cloudflare Pages…), kể cả trong thư mục con, là chạy. Không cần cấu hình gì.

Cài lên màn hình chính: Safari → Chia sẻ → Thêm vào MH chính; Chrome Android → menu → Cài đặt ứng dụng.

Ghi chú: iOS không dùng icon SVG cho màn hình chính, nên trên iPhone icon có thể là ảnh chụp trang. Muốn icon đẹp thì thêm một file `icon-180.png` và trỏ `apple-touch-icon` vào nó.
