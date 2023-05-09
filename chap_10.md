# Chương 10: Đào và đồng thuận

### 1. Giới thiệu
- Đào để tạo ra giá trị bitcoin mới, là nền tảng cho phòng thanh toán phi tập trung, qua đó các giao dịch đc xác thực và thanh toán
- Việc đào bảo mật hệ thống bitcoin (đồng thuận mạng lưới mà k cần cơ quan quyền lực trung tâm nào) -> phần thưởng là bitcoin và có phí đào
- Các thợ đào xác thực giao dịch mới và ghi vào sổ cái, các block đc đào sau mỗi 10p -> các giao dịch trong block đc đào có nghĩa đc xác nhận và có thể chi tiêu
- Các thợ đào nhận đc phần thưởng là bitcoin mới và phí giao dịch -> phải giải quyết bài toán băm mật mã (proof of work)
- Quá trình đào đc thiết kế để mô phỏng việc giảm dần lợi nhuận -> số lượng bitcoin đc tạo mới giảm dần trong 4 năm 1 lần
- Các thợ đào cũng kiếm đc phí giao dịch từ đào bitcoin -> khi lượng bitcoin đào đc giảm thì phí giao dịch trở thành mục tiêu duy nhất thợ đào đào

<br>

### 2. Kinh tế học và chế tạo tiền tệ bitcoin
- Bitcoin đc đúc trong quá trình tạo block mới -> sau mỗi 10p 1 block đc thêm vào blockchain -> mỗi 4 năm số tiền tạo ra từ block mới giảm 1 nửa
- Việc phát hành hữu hạn và giảm dần là để chống lạm phát

<br>

### 3. Đồng thuận phi tập trung
- Phát minh chính của satoshi là cơ chế phi tập trung cho đồng thuận nổi bật -> sự đồng thuận là giả tưởng nổi bật của sự tương tác k đồng bộ của hàng ngàn nút độc lập (theo quy tắc đơn giản)
- Sự đồng thuận phi tập trung ảnh hưởng lẫn nhau của 4 tiến trình độc lập:
    + Xác minh độc lập mỗi giao dịch bởi từng nút đầy đủ, dựa trên danh sách toàn diện các tiêu chí
    + Tổng hợp độc lập các giao dịch thành block mới bởi nút đào, kết hợp với việc tính toán proof of work
    + Xác minh độc lập block mới bởi từng nút, tổng hợp vào 1 chuỗi
    + Lựa chọn độc lập bởi từng nút, chuỗi có tính toán lũy tích nhất đc thể hiện qua PoF

<br>

### 4. Độc lập xác thực các giao dịch
- Các giao dịch sau khi tạo đầu ra -> kết quả gửi đến các nút lân cận để lan truyền trên toàn bộ mạng bitcoin
- Một nút khi nhận đc giao dịch sẽ kiểm tra -> nếu hợp lệ sẽ chuyển tiếp sang các nút lân cận, nếu không sẽ bị loại bỏ
- Các tiêu chí xác minh:
    + Cú pháp, cấu trúc giao dịch phải chính xác
    + Danh sách đầu vào, đầu ra k đc trống
    + Kích thước giao dịch (byte) nhỏ hơn MAX_BLOCK_SIZE
    + Mỗi giá trị đầu ra, cũng như tổng cộng phải nằm trong phạm vi(dưới 21 triệu BTC, nhiều hơn ngưỡng rác)
    + vv....
- Bằng cách xác minh độc lập giao dịch trc khi truyền -> nút xây dựng một vùng các giao dịch hợp lệ (chưa đc xác nhận) là vùng giao dịch, vùng nhớ (mempool)

<br>

### 5. Các nút đào
- Một số nút trên mạng bitcoin là nút đào. Vd như nút của Jing (thợ đào) -> phần cứng chuyên dụng kết nối với máy chủ chạy nút đầy đủ
- Nút của Jing chờ nhận các block mới trên mạng -> nếu nút mới xuất hiện có nghĩa là đã có ng thắng trong cuộc "đào" và cũng có nghĩa là cuộc "đào" mới đã bắt đầu

<br> 

### 6. Tổng hợp các giao dịch vào block
- Sau khi xác thực giao dịch, một nút bitcoin sẽ thêm chúng vào vùng nhớ -> nút của Jing sẽ tổng hợp các giao dịch này vào block ứng cử
- Tại thời điểm Alice mua cà phê, nút của Jing giáp một chuỗi đến block 277314, lắng nghe block 277315 -> khi thấy block 277315 thì bắt đầu đào block 277316
- Trong 10p trc, nút của Jing đang đào block 277315, khi nó nhận đc block 277315 -> so sánh các giao dịch trùng và bỏ đi, lấy các giao dịch k trùng để đào tiêp block 277316 (block ứng cử)
- Block ứng cử chỉ hợp lệ khi nó tìm ra lời giải cho bài toán PoF

<br>

### 7. Giao dịch coinbase
- Giao dịch đầu tiên trong block là giao dịch coinbase -> giao dịch này xây dựng bởi nút của Jing và chứa phần thưởng đào
- Nút của Jing tạo ra giao dịch coinbase như để trả phần thưởng đến ví của anh ấy -> tổng phần thưởng là coinbase và phí giao dịch
- Giao dịch coinbase k tiêu thụ UTXO như đầu vào -> nó có đầu vào là coinbase và đầu ra là bitcoin trả đến ví thợ đào

<br>

### 8. Phần thưởng coinbase và phí
- Để xây dựng giao dịch coinbase, nút của Jing phải tính tổng phí giao dịch
- Tiếp theo nút của Jing tính toán phần thưởng cho block mới
- Cộng tổng lại và trả ra kết quả (nếu Jing viết giao dịch coinbase trả cho mình 100 BTC sẽ đc coi là k hợp lệ bởi các nút ngang hàng)

##### 8.1 Cấu trúc giao dịch coinbase
- Tham chiếu đầu vào đc đặt bằng 0 (k có mã băm giao dịch)
- Thứ tự đầu ra đc điền bởi 4 0xFF
- Kịch bản mở khóa đc thay bằng dữ liệu coinbase

##### 8.2 Dữ liệu coinbase
- Giao dịch coinbase kịch bản mở khóa là dữ liệu coinbase (2-100 byte), ngoài vài byte đầu tiên dữ liệu coinbase đc sử dụng bởi thợ đào theo ý họ
- Hiện nay các thợ đào sử dụng dữ liệu coinbase để thêm giá trị nonce mở rộng và chuỗi nhận dạng trang trại đào

<br>

### 9. Xây dựng tiêu đề block
- Để xây dựng tiêu đề block, nút đào cần điền 6 trường: phiên bản, mã băm block kế trc, gốc merkle, nhãn thời gian, chỉ tiêu, nonce
- Sau khi tất cả các trg đc điền, tiêu đề đã hoàn chỉnh và bắt đầu đào -> mục tiêu là tìm số nonce (kết quả mã băm tiêu đề block sao cho nhỏ hơn chỉ tiêu)

<br>

### 10. Đào block
- Bây giờ 1 block ứng cử đã đc xây dựng bởi nút của Jing -> bắt đầu đào (tìm kết quả cho thuật toán PoF -> băm SHA256)
- Đào là quá trình băm tiêu đề lặp đi lặp lại khi thay đổi số nonce để tìm ra kết quả bé hơn chỉ tiêu

##### 10.1 Thuật toán PoF
- Thuật toán băm lấy đầu vào tùy ý tạo ra 1 kết quả có độ dài xác định (vân tay đầu vào) -> k có 2 đầu vào khác nhau tạo ra cùng 1 mã băm
- Với SHA256 đầu ra luôn là 256 bit -> tạo mã băm của từ "I am Satoshi Nakamoto" (đọc)
- Số nonce đc dùng để thay đổi đầu ra của hàm băm mật mã SHA256
- Chỉ tiêu là mặt chỉ tiêu số học mà mã băm nhỏ hơn -> chỉ tiêu càng nhỏ tìm mã băm càng khó
- Khi thuật toán dựa trên mã băm SHA256, đầu vào cấu thành bằng chứng rằng 1 khối lượng công việc nhất định đã thực hiện để tạo ra mã băm nhỏ hơn chỉ tiêu -> PoF

##### 10.2 Mô tả chỉ tiêu
- Ví dụ bit chỉ tiêu trong tiêu đề block là 0x1903a30c:
    + số mũ là 0x19
    + hệ số là 0x03a30c
    -> chỉ tiêu = hệ số * 2^(8 * (số mũ - 3))

##### 10.3 Đặt lại chỉ tiêu để điều chỉnh độ khó
- Chỉ tiêu là 1 tham số động đc điều chỉnh định kì để đáp ứng 10p đào đc 1 block
- Việc đặt lại chỉ tiêu diễn ra tự động, độc lập trên mọi nút -> nếu mạng lưới tìm block nhanh hơn 10p sẽ giảm chỉ tiêu, nếu chậm hơn sẽ tăng chỉ tiêu
- Mặc dù việc điều chỉnh xảy ra sau 2016 block nhưng do lỗi off-by-one dẫn đến độ khó tăng thêm 0.05% (do tính tg của 2015 block)
- Để tránh sự đột biến độ khó, việc điều chỉnh đặt lại chỉ tiêu phài nhỏ hơn 1 hệ số bằng 4 -> nếu lớn hơn nó sẽ đc đặt bằng hệ số đó
- Chỉ tiêu là độc lập so với lượng giao dịch và giá trị giao dịch
- Đào coin tiêu thụ nhiều điện năng

<br>

### 11. Đào block thành công
- Nút của Jing xây dựng block ứng cử và đào
- Gần 11 phút phần cứng của Jing tìm ra số nonce thỏa mãn chỉ tiêu và gửi lại nút đào
- Ngay lập tức nút của Jing truyền block mới lên mạng và đc các nút khác xác nhận -> các nút khác lại bắt đầu đào block mới trên block của Jing (xác nhận block Jing đào đc)

<br>

### 12. Xác thực một block mới
- Khi một block đc đào và truyền lên mạng, một nút nhận đc block sẽ xác nhận block có hợp lệ hay k -> nếu có sẽ lan truyền k thì sẽ bị loại -> thợ đào block đó sẽ k nhận đc phần thưởng nếu block k hợp lệ
- Khi nút nhận đc block, nó sẽ xác nhận block bằng các tiêu chí (k liệt kê ở đây) -> block k đáp ứng sẽ bị từ chối
- Việc xác thực block bởi các nút trên mạng làm thợ đào k thể gian lận (đồng thuận phi tập trung)

<br>

### 13. Ghép nối và lựa chọn các chuỗi block
- Bước cuối cùng là ghép nối các block thành chuỗi (block có nhiều PoF nhất)
- Các nút duy trì bộ 3 block: 
    + block đc kết nối với blockchain chính
    + block tạo ra các nhánh từ blockchain chính (block thứ cấp)
    + block k có mẹ trong những chuỗi đc biết đến (block lạc mẹ)
- Chuỗi chính ở bất kì thời điểm nào cũng là chuỗi hợp lệ (có các block có nhiều PoF nhất), thường là chuỗi có nhiều block nhất
- Chuỗi chính cũng có các nhánh là "anh, chị, em" của các block trên chuỗi nhưng k phải là 1 phần của chuỗi chính -> đc lưu để tham vấn trong tương lai
- Khi một block mới đc nhận, nút sẽ cố gắng nhét nó vào blockchain hiện có -> tìm mã băm block kế trc -> thường block kế trc đó là đỉnh của blockchain
- Đôi khi block mở rộng 1 chuỗi k phải chuỗi chính -> block gắn vào chuỗi thứ cấp kéo dài sau đó so sánh với chuỗi chính, chuỗi nào có tổng công việc nhiều hơn sẽ thành chuỗi chính, chuỗi kia thành chuỗi thứ cấp
- Nếu một block đc đào và k thấy block mẹ trong các chuỗi sẽ thành block lạc mẹ -> đc lưu vào vùng mồ côi khi tìm thấy block mẹ để đc gắn vào chuỗi -> hay xảy ra nếu 2 block đc đào trong tg ngắn và block con đc nhận trc block mẹ
- Bằng cách chọn chuỗi hợp lệ có tích lũy công việc nhiều nhất -> tất cả các nút đạt đc sự đồng thuận trên toàn mạng

##### 13.1 Phân nhánh blockchain
- Blockchain là cấu trúc dữ liệu phi tập trung -> k phải lúc nào các bản sao cũng nhất quán. Các block có thể đến các nút khác nhau vào thời điểm khác nhau -> các nút có góc nhìn khác nhau về blockchain
- Để giải quyết, nút chọn ra chuỗi dài nhất (tích lũy nhiều PoF nhất) -> mạng bitcoin sẽ nhất quán trạng thái
- Mỗi nút có góc nhìn riêng về blockchain toàn cầu. Khi nhận 1 block -> cập nhật bản sao blockchain của riêng nó (chọn chuỗi dài nhất)
- Một phân nhánh xảy ra khi có 2 block ứng cử cạnh tranh tạo blockchain dài nhất (2 thợ đào đc block trong cùng khoảng tg ngắn) -> cả 2 phát tán lên mạng -> một nút nhận đc block này, nút khác nhận block kia -> khi nhận đc 2 block ứng cử có cùng block mẹ (sẽ đưa vào nhánh thứ cấp)
- Các nút tiếp tục đào -> khi nào từ 1 block ứng cử tạo ra đc chuỗi dài nhất thì block đấy là block chiến thắng, block thua cuộc trả lại các giao dịch trong nó để phục vụ công cuộc đào tiếp theo

<br>

### 14. Cuộc đua đào và băm
- Đào bitcoin là một ngành công nghiệp cực kì cạnh tranh -> công suất băm đã tăng lên cấp số mũ trong những năm tồn tại của bitcoin
- Đọc

<br>

### 15. Giải pháp nonce bổ sung
- Kể từ năm 2012 việc đào bitcoin đã cải tiến để giải quyết giới hạn cơ bản trong tiêu đề block. Trong những ngày đầu bitcoin, một thờ đào tìm đc block thông qua các lần lặp số nonce (nếu k tìm thấy sẽ lập nhãn thời gian)
- Tuy nhiên dùng nhãn thời gian k đủ tốt -> có thể làm block trở nên k hợp lệ khi kéo dài quá xa
- Giải pháp là sử dụng coinbase như 1 nonce bổ sung (8 byte nonce bổ sung + 4 byte nonce gốc)

<br>

### 16. Trang trại đào
- Một thợ đào khó có thể tìm ra đc block mới -> chi phí điện năng cao sẽ gây lỗ -> tập hợp các thợ đào thành trang trại đào -> chia sẻ lợi nhuận
- Mỗi thợ đào nếu đào một mình thì nguy cơ lỗ rất cao -> 4 năm mới đào đc 1 block (cũng có thể đào đc hơn -> lãi nhưng cũng có thể k đào đc block nào) -> tập hợp thành trang trại đào, tuy lợi nhuận giảm nhưng thu nhập thường sẽ đều hơn
- Trang trại đào phân phối hàng trăm, nghìn thợ đào -> nút thợ đào kết nối với máy chủ trung tâm trang trại đào -> đồng bộ hóa nỗ lực đào với thợ khác -> chia sẻ lợi nhuận
- Các block thành công trả phí cho trang trại đào -> trang trại trả phí cho thợ (theo nỗ lực của nút) và lấy chiết khấu phí của thợ
- Phân chia tiền thưởng cho các thợ đào phụ thuộc vào PoF của các nút -> thường trang trại đặt chỉ tiêu cao hơn (độ khó thấp hơn) để phân chia tiền thưởng -> hàng ngàn thợ đào tìm kiếm mã băm giá trị thấp -> cuối cùng tìm thấy mã băm đáp ứng tiêu chí mạng bitcoin

##### 16.1 Trang trại đc quản trị
- Hầu hết các trang trại đều đc quản trị (1 công ty hoặc cá nhân chạy máy chủ trang trại) -> chủ sỡ hữu đc gọi là nhà điều hành trang trại -> thu lệ phí từ thợ đào
- Máy chủ trang trại chạy phần mềm chuyên dụng và giao thức đào của trang trại, điều hối hoạt động của thợ đào
- Máy chủ trang trại kết nối đến 1 hoặc nhiều nút đầy đủ (lấy bản sao đầy đủ blockchain) -> xác thực block và các giao dịch thay cho thợ đào (một thợ đào phải suy trì 1 nút đầy đủ sẽ khá phải cân nhắc)
- Các thợ đào kết nối với máy chủ bằng giao thức STM, GBT -> máy chủ trang trại tạo block ứng cử viên -> gửi tiêu đề block ứng cử cho thợ đào -> thợ đào đào block với chỉ tiêu cao hơn (dễ hơn)

##### 16.2 Trang trại đào ngang hàng (P2Pool)
- Các trang trại có quản trị tạo khả năng gian lận (định hướng tới giao dịch tiêu 2 lần hoặc vô hiệu hóa block), nó còn là điểm chết khi máy chủ bị tê liệt thì thợ k thể đào đc -> ra đời P2Pool
- P2Pool phi tập trung hóa quản trị, thực thi hệ thống như blockchain -> chuỗi chia sẻ (blockchain mức độ thấp hơn bitcoin)
- Cho phép thợ đào hợp tác bằng các phần chia đào trong chuỗi chia sẻ (30s/1 khối chia sẻ). Từng block trong khối chia sẻ ghi nhận phần thưởng chia sẻ ứng với thợ đào 
- Khi một trong các block chia sẻ đạt đc chỉ tiêu mạng bitcoin -> phần thưởng sẽ chia cho các thợ đào đóng góp
- Thợ đào kết nối phần cứng với nút P2Pool -> mô phỏng chức năng máy chủ trang trại bằng cách gửi block mẫu cho phần cứng
- Thợ đào xây dựng block ứng cử riêng nhưng cộng tác với nhau trong chuỗi chia sẻ

<br>

### 17. Tấn công đồng thuận
- Cơ chế đồng thuận của bitcoin, ít nhất về mặt lí thuyết dễ bị tấn công bởi thợ đào (trang trại đào) nếu họ cố gắng sử dụng công suất băm của họ cho mục đích không trung thực
- Tấn công đồng thuận chỉ có thể gây ảnh hưởng đến các block trong tương lai (hoặc nhiều nhất là quá khứ gần nhất). Các cuộc tấn công không ảnh hưởng đến các khóa bí mật và chữ kí ECDSA, không thể đánh cắp, tiêu bitcoin mà k có chữ kí -> chỉ có thể ảnh hưởng đến các block gần nhất và gây ra việc từ chối dịch vụ đối với việc tạo ra các block tương lai
- Tấn công quá bán là nhóm thợ đào kiểm soát (51% công suất băm toàn mạng) thông đồng tấn công bitcoin -> gây ra các phân nhánh cố ý, các giao dịch lặp chi, các cuộc tấn công từ chối dịch vụ
- Tấn công phân nhánh/lặp chi làm các block đc xác nhận trở thành k hợp lệ bằng cách phân nhánh bên dưới chúng và tái hợp trên chuỗi thay thế (có thể làm mất hiệu lực 6 block liên tiếp) -> lặp chi chỉ giao dịch của kẻ tấn công vì chỉ hắn mới có chữ kí hợp lệ
- Tấn công tiêu 2 lần có thể xảy ra theo 2 cách: 
    + Trc khi giao dịch đc xác nhận 
    + Lợi dụng phân nhánh blockchain để hoàn tác 1 vài block
- Để chống lại kiểu tấn công này, thương gia cần:
    + Chờ ít nhất 6 lần xác nhận trc khi đưa sản phẩm cho ng mua
    + Nên sử dụng tk ủy thác đa chữ kí, chờ đợi một vài xác nhận sau khi tk ủy thác nhận đc tiền
- Kịch bản cho 1 cuộc tấn công đồng thuận là từ chối dịch vụ -> bỏ qua các giao dịch cụ thể -> nếu giao dịch có trong block đc đào bởi thợ khác sẽ phân nhánh đào lại block đó và loại trừ các giao dịch đó
- Kịch bản tấn công quá bán không thực sự đòi hỏi công suất băm quá bán (51% là ở mức bảo đảm tấn công thành công)
- Một thợ đào riêng lẻ không thể tấn công -> cần 1 trang trại đào -> chủ sở hữu trang trại có thể có lợi nhuận lớn từ tấn công đồng thuận

<br>

### 18. Thay đổi các quy tắc đồng thuận
- Các quy tắc đồng thuận xác định tính hợp lệ của giao dịch và block, là cơ sở cho sự hợp tác của các nút và chịu trách nhiệm cho sự hội tụ blockchain duy nhất trên mạng lưới
- Quy tắc đồng thuận không thay đổi trong thời gian ngắn (phải nhất quán trên các nút) nhưng có thể thay đổi về lâu dài (tích hợp tính năng mới, sửa lỗi...) nhưng khó khăn

##### 18.1 Phân nhánh cứng
- Mạng lưới phân nhánh ra 2 chuỗi (sự thay đổi các quy tắc đồng thuận) -> phân nhánh cứng -> k tái hợp lại 1 chuỗi nữa (do một phần mạng lưới hoạt động quy tắc đồng thuận khác với phần còn lại)
- Các phân nhánh cứng đc sử dụng để thay đổi các quy tắc đồng thuận nhưng phải có sự phối hợp của các nút (nút k nâng cấp k thể tham gia vào cơ chế đồng thuận buộc phải vào nhánh riêng thời điểm phân nhánh cứng)
- Đọc ví dụ

##### 18.2 Phân nhánh cứng: phần mềm, mạng lưới, đào và chuỗi
- Hai tình huống dẫn đến phân nhánh cứng:
    + Một lỗi trong quy tắc đồng thuận
    + Sự sửa đổi có chủ ý của các quy tắc đồng thuận (phân nhánh mềm sẽ tới trc phân nhánh cứng)
- Phân nhánh mềm k thể tự tạo ra đc phân nhánh cứng. Để phân nhánh cứng xảy ra, bản thực thi cạnh tranh phải đc kích hoạt bởi các nút đào, ví, nút trung gian
- Các quy tắc đồng thuận có thể khác nhau theo cách rõ ràng và chi tiết (xác thực giao dịch hoặc block), khác nhau khó thấy hơn (kịch bản bitcoin, chữ kí), khác nhau theo cách k lường trc đc (ràng buộc đồng thuận tiềm ẩn của các hệ thống, bản thực thi)
- Phân nhánh cứng như sự phát triển 4 giai đoạn: phân nhánh mềm -> phân nhánh mạng -> phân nhánh đào -> phân nhánh chuỗi
- Khi bản thực thi phân nhánh triển khai trên mạng lưới -> các nút, ví, nút trung gian dùng bản thực thi này -> nếu quy tắc đồng thuận liên quan đến giao dịch, ví tạo phân nhánh mạng -> đào thành block tạo phân nhánh cứng
- Mạng lưới phân nhánh, các nút dựa trên bản thực thi ban đầu k nhận giao dịch theo quy tắc mới -> chỉ giao tiếp với các nút cũ -> phân chia thành 2 mạng lưới
- Các thợ đào theo quy tắc mới sẽ đào block mới, các thợ đào cũ sẽ đào chuỗi riêng biệt dựa trên quy tắc cũ

##### 18.3 Sự chia rẽ thợ đào và độ khó
- Thợ đào tách ra 2 chuỗi khác nhau, công suất băm chia theo 2 chuỗi (theo tỉ lệ bất kì của 2 chuỗi). Ví dụ công suất 80-20, quy tắc đồng thuận mới có 80% công suất so với lúc trc (giảm 20%) nên các block đc tìm thấy trung bình mỗi 12p
- Tốc độ phát hành block sẽ tiếp tục (trừ khi có sự thay đổi công suất băm) cho đến khi 2016 block đc đào -> độ khó đc điều chỉnh -> trở lại 10p một block (đối với quy tắc cũ thì thời gian sẽ lâu hơn)

##### 18.4 Các phân nhánh cứng gây tranh cãi
- Những phân nhánh cứng đc xem là nguy cơ vì buộc nhóm thiểu số phải nâng cấp hoặc ở lại chuỗi thiểu số -> nhiều nhà phát triển k muốn sử dụng phân nhánh cứng để nâng cấp quy tắc đồng thuận (trừ khi có sự chấp nhận gần như toàn bộ mạng lưới)
- Các nhà nghiên cứu xem nó như 1 cơ chế nên đc sử dụng hạn chế

##### 18.5 Phân nhánh mềm
- Chỉ những thay đổi đồng thuận k tương thích về sau mới gây ra phân nhánh -> nếu sửa đổi mà giao dịch hay block vẫn hợp lệ theo quy tắc trc sẽ k gây phân nhánh
- Phân nhánh mềm k phải là phân nhánh nào cả -> một thay đổi tương thích về sau với quy tắc đồng thuận cho phép phần mềm tiếp tục hoạt động vs quy tắc đồng thuận cũ
- Phân nhánh mềm đc sử dụng để hạn chế quy tắc đồng thuận chứ k phải để mở rộng nó -> block đc tạo ra theo quy tắc mới phải hợp lệ theo quy tắc cũ
- Phân nhánh mềm có thể thực hiện bằng nhiều cách -> k yêu cầu các nút phải nâng cấp hoặc buộc các nút k nâng cấp ra khỏi đồng thuận

##### 18.6 Phân nhánh mềm tái định nghĩa mã vận hành NOP
- Nhiều phân nhánh mềm đc thực hiện dựa trên định nghĩa các mã vận hành NOP (ngôn ngữ kịch bản bitcoin có 10 mã vận hành NOP1 đến NOP10) -> sự hiện diện của nó đc coi là toán tử vô hiệu (k có tác dụng)
- Phân nhánh mềm có thể thay đổi ngữ nghĩa của một mã NOP để cho nó ý nghĩa mới

##### 18.7 Những cách khác để nâng cấp phân nhánh mềm
- Đọc thêm Segwit

##### 18.8 Những lời chỉ trích đối với phân nhánh mềm
- Các phân nhánh mềm dựa trên NOP là tương đối k gây tranh cãi (cho phép nâng cấp k phá vỡ)
- Các chỉ trích:
    + Món nợ kĩ thuật: tăng chí phí bảo trì trong tương lai, tăng khả năng xảy ra lỗi, lỗ hổng bảo mật
    + Nới lỏng xác thực: những phần mềm chưa sửa đổi coi các giao dịch là hợp là mà k cần đánh giá qua các quy tắc đồng thuận đã sửa đổi
    + Nâng cấp k thể đảo ngược: các giao dịch với quy tắc mới sẽ k thể đảo ngược (nếu đảo ngược có thể gây ra mất tiền theo các quy tắc cũ)

<br>

### 19. Báo hiệu phân nhánh phần mềm bằng phiên bản block
- Đọc sách

<br>

### 20. Phát triển phần mềm đồng thuận
- Việc phát triển phần mềm đồng thuận tiếp tục tiến triển và có nhiều thảo luận về các cơ chế khác nhau để thay đổi quy tắc đồng thuận -> đặt mức cao cho sự phối hợp và đồng thuận các thay đổi
- Không có giải pháp hoàn hảo cho sự phát triển đồng thuận -> có thay đổi phân nhánh mềm tốt và ngược lại -> rất khó để thay đổi, đòi hỏi sự thỏa hiệp