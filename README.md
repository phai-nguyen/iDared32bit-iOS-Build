# Dựng IPA iDared 32bit

Repo này chứa workflow GitHub Actions để lấy mã nguồn công khai của [iDared 32bit](https://github.com/iDared32bit-emu/iDared32bit), dựng ứng dụng iOS trên máy macOS của GitHub và đóng gói thành IPA.

## Cách chạy

1. Mở tab **Actions** của repo này.
2. Chọn workflow **Build iDared 32bit IPA**.
3. Nhấn **Run workflow**. Có thể để nguyên nhánh nguồn mặc định `idared32bit`.
4. Khi chạy xong, mở lần chạy đó và tải artifact `iDared32bit-IPA-<số-lần-chạy>`.

Artifact gồm IPA chưa ký và log build. IPA này cần được ký bằng ESign hoặc công cụ cài IPA tương tự trước khi cài lên iPhone. Không cần lưu chứng chỉ ký trong repo.

## Lưu ý

- Ứng dụng gốc yêu cầu iOS 16 trở lên; workflow giữ nguyên mức hỗ trợ của dự án.
- iDared chạy một số game/app 32-bit đời cũ; nó không khởi động toàn bộ hệ điều hành iOS cũ.
- Repo này không kèm game. Chỉ nạp các tệp game mà anh có quyền sử dụng.
- Mã nguồn iDared thuộc dự án riêng và được cấp phép MPL-2.0. Workflow không sửa mã nguồn upstream.
