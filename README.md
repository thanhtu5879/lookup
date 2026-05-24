# Xiaomi Lookup v4.2

Web tra cứu tính năng Xiaomi Vietnam, build theo chuẩn v4.2 và giữ phần media/workflow hữu ích từ handbook v4.1.

## Test local ngay

Mở `index.html` trong trình duyệt. Khi chưa có config, app tự chạy demo local.

- Nhân viên demo: `DEMO001`
- Admin demo: `TUADMIN`
- PIN admin demo: `2610`

## Điền config thật

Trong `index.html`, cập nhật:

```js
const CONFIG = {
  SUPABASE_URL: "https://...supabase.co",
  SUPABASE_ANON_KEY: "...",
  EDGE_FUNCTION_BASE_URL: "",
};
```

Nếu `EDGE_FUNCTION_BASE_URL` để trống, web tự dùng:

```text
SUPABASE_URL/functions/v1
```

## Supabase

1. Chạy SQL trong `supabase/schema.sql`.
2. Deploy 3 Edge Function:
   - `verify-employee`
   - `admin-action`
   - `process-feature-report`
3. Set secrets:

```text
SUPABASE_URL
SUPABASE_SERVICE_ROLE_KEY
ADMIN_PIN
ADMIN_EMPLOYEE_ID
ANTHROPIC_API_KEY
ANTHROPIC_MODEL
```

`ANTHROPIC_API_KEY` có thể để trống trong giai đoạn đầu; function sẽ fallback bằng steps hiện tại.

Để `process-feature-report` chạy tự động, tạo Database Webhook trong Supabase cho sự kiện `INSERT` của bảng `feature_reports`, trỏ tới Edge Function `process-feature-report`.

## Lưu ý bảo mật

Frontend chỉ dùng anon key. PIN admin không lưu trong file web; người quản trị nhập khi mở dashboard và Edge Function kiểm tra bằng secret `ADMIN_PIN`.
