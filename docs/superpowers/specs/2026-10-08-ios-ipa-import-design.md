# Thiết kế: nhập IPA trực tiếp trong iDared

Ngày: 2026-10-08
Repo: `phai-nguyen/iDared32bit-iOS-Build`
Nhánh thiết kế: `codex/ios-ipa-import`

## Mục tiêu

Cho phép người dùng mở bộ chọn tệp iOS từ màn hình chính iDared và chọn một file `.ipa`. Ứng dụng sao chép file vào thư mục `Documents/touchHLE_apps` để bộ chọn game hiện có thể đọc nó ở lần khởi chạy tiếp theo.

## Hiện trạng

Nút **File manager** trong app picker gọi `shareddocuments://` để mở thư mục tài liệu của iDared trong ứng dụng Tệp. Nó không trình bày một bộ chọn file và không nhập IPA. Vì vậy người dùng phải tự thao tác giữa thư mục Tải về và `touchHLE_apps`.

## Luồng đề xuất

1. Thêm nút **Chọn IPA** trên màn hình chính.
2. Nút gọi cầu nối native iOS để mở bộ chọn tài liệu hệ thống.
3. Người dùng chọn một file có đuôi `.ipa`; thao tác hủy đóng bộ chọn mà không báo lỗi.
4. App tạo `Documents/touchHLE_apps` nếu chưa tồn tại, sau đó sao chép file đã chọn vào đó.
5. Không ghi đè game cùng tên. Nếu tên đã tồn tại, tạo tên kế tiếp như `Game (2).ipa`.
6. Hiển thị thông báo tiếng Việt khi thành công hoặc thất bại. Thông báo thành công nhắc đóng rồi mở lại iDared để làm mới danh sách.
7. Nút **File manager** hiện tại vẫn mở thư mục tài liệu để người dùng quản lý file thủ công.

## Thành phần

- **Giao diện Rust app picker:** thêm selector cho nút **Chọn IPA**, giữ lại hành động File manager.
- **Cầu nối native iOS:** trình bày `UIDocumentPickerViewController`, giữ quyền truy cập tệp trong lúc sao chép, kiểm tra đuôi file, chép IPA vào thư mục đích, và báo kết quả.
- **GitHub Actions:** checkout repo cấu hình/patch riêng trước khi checkout upstream; áp dụng bản vá sau khi lấy mã nguồn upstream; sau đó build và đóng gói IPA như hiện tại.

## Lỗi và giới hạn

- Chỉ nhập file `.ipa`; thao tác chọn file khác phải bị từ chối với thông báo rõ ràng.
- Hủy bộ chọn không thay đổi dữ liệu.
- Lỗi quyền truy cập, tạo thư mục, hoặc sao chép phải được hiển thị bằng tiếng Việt.
- Tính năng chỉ nhập IPA. Nó không ký/cài IPA vào iPhone và không bảo đảm game đó tương thích với touchHLE/iDared.
- Phiên bản đầu yêu cầu khởi động lại iDared để quét lại danh sách game; không xây dựng cơ chế refresh động.

## Kiểm tra

- Build iOS Release qua GitHub Actions phải thành công và tạo IPA.
- Bản build trên iPhone iOS 18.7: mở bộ chọn, nhập IPA hợp lệ, xác nhận file nằm trong `touchHLE_apps`, khởi chạy lại app và thấy game trong danh sách.
- Kiểm tra riêng: hủy bộ chọn, chọn file không phải IPA, nhập hai IPA trùng tên, và kiểm tra thông báo khi thao tác sao chép thất bại.

## Tiêu chí chấp nhận

- Người dùng có thể chọn IPA từ Files/iCloud Drive ngay trong app picker.
- IPA được sao chép vào đúng thư mục mà touchHLE quét.
- File trùng tên không bị ghi đè.
- File manager hiện tại vẫn dùng được.
- Build unsigned IPA vẫn giữ nguyên quy trình ESign hiện có.
