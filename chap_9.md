# Chương 9: Blockchain

### 1. Giới thiệu
- Cấu trúc dữ liệu blockchain là một danh sách backlink gồm các block đc sắp xếp theo thứ tự (thường đc hình dung như 1 ngăn xếp theo chiều dọc, block xếp chồng nhau và block đầu là nền tảng, block sau có liên kết với block trc nó)
- Mỗi block đc xác định bằng 1 mã băm (SHA256) trên tiêu đề block và tham chiếu tới block trc đó (block mẹ) -> trong block chứa mã băm block mẹ trên tiêu đề
- Mỗi block chỉ có 1 block mẹ nhưng tạm thời có nhiều block con (phân nhánh khi các thợ đào tìm ra các block khác nhau) -> cuối cùng 1 block mẹ chỉ có 1 block con (phân nhánh đc giải quyết)
- Khi block mẹ bị thay đổi thì block con cũng bị thay đổi (mã băm) do tiêu đề block con chứa mã băm block mẹ -> ảnh hưởng đến các block cháu -> nếu 1 block dài sẽ có tính bất biến

<br>

### 2. Cấu trúc của block
- Block là một cấu trúc dữ liệu dạng container, nó gom các giao dịch đưa vào blockchain. Block gồm 1 tiêu đề (80 byte) theo sau là các giao dịch (tối thiểu 250 byte/giao dịch, trung bình 500 giao dịch)

<br>

### 3. Tiêu đề block
- Gồm 3 bộ siêu dữ liệu block:
    + Tham chiếu tới mã băm block trc
    + Bộ thứ 2 đc đặt tên là dificulty, timestamp, nonce (số dùng 1 lần)
    + Gốc cây merkle, một cấu trúc dữ liệu để tóm tắt hiệu quả toàn bộ giao dịch trong block

<br>

### 4. Định danh block: mã băm tiêu đề block và độ cao block
- Định danh chính block là mã băm mật mã của nó (băm tiêu đề 2 lần SHA256) -> kết quả 32 byte
- Mã băm block k thực sự bên trong cấu trúc của nó -> nó đc tính toán bởi mỗi nút khi nó nhận đc block từ mạng lưới
- Cách thứ 2 định danh block là bằng độ cao block (block k phải là định danh duy nhất vì có phân nhánh block)

<br>

### 5. Block gốc
- Block đầu trong blockchain đc gọi là block gốc
- Mỗi nút luôn khởi đầu với một blockchain có ít nhất là 1 block
- Block gốc chứa thông điệp ẩn bên trong nó

<br>

### 6. Liên kết các block trong blockchain
- Các nút bitcoin đầy đủ duy trì bản sao blockchain từ block gốc -> khi một nút nhận 1 block mới nó sẽ xác thực và liên kết với blockchain

<br>

### 7. Cây merkle
- Mỗi block chứa bản tóm tắt toàn bộ giao dịch trong cây merkle
- Cây merkle (cây băm nhị phân) là cấu trúc dữ liệu đc sử dụng để tóm lược, kiểm tra tính toàn vẹn của những tập dữ liệu lớn
- Cây merkle đc sử dụng để tạo thành 1 vân tay số của toàn bộ giao dịch (xác minh 1 giao dịch có trong block hay k) -> băm đệ quy các cặp nút tới khi còn 1 mã băm (SHA256 đúp)
- Các giao dịch k đc lưu trong cây merkle mà là mã băm của chúng -> băm liên tục các giao dịch
- Để chứng minh một giao dịch cụ thể nằm trong 1 block, một nút chỉ cần tạo các mã băm 32 byte log_2(N) -> tạo một đg dẫn chứng thực (đg dẫn merkle) đến gốc cây

<br>

### 8. Cây merkle và sử dụng thanh toán lược giản (SPV)
- Cây merkle đc sử dụng bởi các nút SPV -> nút SPV k thể tải toàn bộ block (tải các tiêu đề) -> xác thực giao dịch bằng đg merkle

<br>

### 9. Các blockchain thử nghiệm của bitcoin
##### 9.1 Testnet
- Testnet là tên của blockchain, mạng lưới và đồng tiền thử nghiệm (đầy đủ như mainnet, khác biệt đào dễ hơn -> k có giá trị)
- Testnet có độ khó -> khởi động lại k nó sẽ mang giá trị
- Sử dụng testnet (đọc)

##### 9.2 Segnet
- Sử dụng để kiểm tra bằng chứng tách biệt (giờ đã bổ sung vào testnet3)

##### 9.3 Regtest
- Tạo blockchain cục bộ với các mục đích thử nghiệm