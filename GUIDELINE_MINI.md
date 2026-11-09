# Mini guideline — bài làm cá nhân

> Nội dung bám theo schema trong [README.md](README.md) và quy tắc trong [GUIDE.md](GUIDE.md). Ba ca bên dưới được đối chiếu với ảnh và nhãn train hiện có; người làm bài cần xác nhận chúng phản ánh cách mình đã xử lý khi gán nhãn.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` — ID `0` | Xe con, sedan, SUV, taxi và xe bán tải. | Taxi vẫn thuộc `car`; xe bán tải cũng thuộc `car`, không chuyển sang `truck` vì có thùng chở hàng. |
| `truck` — ID `1` | Xe tải chở hàng và xe đầu kéo. | Phân biệt xe tải với xe bán tải thuộc `car`; xe van chở hàng vẫn thuộc `van`. |
| `bus` — ID `2` | Xe buýt và minibus chở khách theo schema của bài. | Không gán mọi xe chở nhiều người vào `bus`; van/minivan chở người thuộc `van`. Nếu chưa phân biệt chắc, ghi lại ảnh và hỏi giảng viên. |
| `van` — ID `3` | Xe van và minivan dùng để chở hàng hoặc chở người. | Phân biệt với xe con thông thường (`car`), xe buýt/minibus (`bus`) và xe tải (`truck`); không quyết định chỉ theo màu xe hoặc kích thước box. |

## Quy tắc chung

- **Box bám sát vật thể:** dùng box chữ nhật ôm sát phần xe nhìn thấy, hạn chế lấy thêm mặt đường và vật thể bên cạnh. Mỗi xe hợp lệ có một box riêng; không gộp nhiều xe vào cùng một box.
- **Vật thể bị che khuất:** vẫn gán nhãn nếu phần nhìn thấy đủ để xác định lớp. Box bao quanh phần nhìn thấy của riêng xe đó; không kéo rộng để đoán phần thân bị che. Hai xe chồng lên nhau vẫn cần hai box riêng nếu nhận diện được từng xe.
- **Vật thể bị cắt bởi rìa ảnh:** gán box cho phần nằm trong ảnh nếu nhận diện được lớp. Cạnh box phía bị cắt chạm rìa ảnh; không suy đoán phần xe nằm ngoài khung hình.
- **Vật thể rất nhỏ/mơ hồ:** không tự đặt ngưỡng kích thước để bỏ xe. Nếu còn nhận diện được lớp thì gán; nếu chỉ còn một mảnh không đủ căn cứ xác định lớp, ghi tên ảnh và vị trí để hỏi giảng viên, không tự đoán.
- **Xe đang đỗ hoặc dừng chờ:** vẫn gán nhãn theo loại xe; trạng thái chuyển động không tạo ra lớp mới.
- **Phạm vi gán nhãn:** chỉ gán bốn lớp trên. Không gán người đi bộ, xe đạp, xe máy, biển báo, đèn giao thông, hình xe trên quảng cáo hoặc hình phản chiếu không phải xe thật trong cảnh.
- **Giữ nhất quán:** không đổi tên ảnh hoặc thứ tự ID lớp. Nhãn YOLO có đúng năm cột `class_id x_center y_center width height`, tọa độ chuẩn hóa trong `[0, 1]`, kích thước dương và box không vượt biên. Ảnh không có xe hợp lệ vẫn có file `.txt` rỗng.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001.jpg` | Xe con màu trắng ở nửa phải ảnh bị cột lớn ở tiền cảnh che một phần, dễ bỏ sót hoặc mở rộng box để đoán phần bị che. | Gán `car` (ID `0`) vì còn nhận diện được xe con; box bao quanh phần xe nhìn thấy, không ước lượng thêm phần bị cột che. | Che khuất không làm thay đổi loại xe. Hướng dẫn yêu cầu gán xe còn nhận diện được và dùng phần nhìn thấy để xác định box. |
| `drive_002.jpg` | Xe van màu sáng sát mép phải ảnh bị cắt mất một phần thân xe. | Giữ lớp `van` (ID `3`) theo hình dáng phần xe còn thấy; vẽ box cho phần nằm trong ảnh và để cạnh phải chạm rìa ảnh. | Xe bị cắt khung vẫn được gán nếu nhận diện được lớp; không bỏ xe chỉ vì thiếu một phần thân và không đoán phần ngoài ảnh. |
| `drive_003.jpg` | Dãy xe con và taxi xếp sát nhau theo đường chéo ở khu vực giữa ảnh; một số xe che một phần xe phía sau, dễ gộp box hoặc bỏ sót. | Tách một box `car` (ID `0`) cho từng xe còn nhận diện được, bám theo phần nhìn thấy của mỗi xe. Taxi dùng cùng lớp `car`. | Mỗi vật thể là một box riêng, kể cả khi cùng lớp hoặc chồng lấn. Taxi nằm trong schema `car`, không phải một lớp riêng. |

## Ghi nhận hỗ trợ

- Codex hỗ trợ diễn đạt guideline từ tài liệu của bài và đối chiếu ba ảnh với nhãn train hiện có. Người làm bài cần tự xác nhận nội dung và ghi phần đã tự kiểm tra tại mục công cụ AI trong báo cáo cá nhân.
