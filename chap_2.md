# Chương 2: Bitcoin hoạt động như nào?

### 1. Tổng quan về bitcoin: 
+ Các trang web bitcoin giúp tìm kiếm địa chỉ, các giao dịch, block của bitcoin

<br />

### 2. Mua cà phê bằng bitcoin: 
+ Alice mua cà phê ở tiệm của Bob bằng cách trả bitcoin thông qua ví của cô

<br />

### 3. Các giao dịch bitcoin: 
+ Một giao dịch thông báo cho mạng lưới chủ sở hữu của 1 lượng bitcoin đã chuyển giao lượng đó cho ng khác và ng nhận đó có thể chi tiêu lượng bitcoin đó bằng các giao dịch khác

##### 3.1 Đầu vào và đầu ra giao dịch:
+ Có nhiều đầu vào, đầu ra
+ Tổng đầu ra nhỏ hơn 1 chút so với đầu vào, sự chênh lệch đó là mức phí giao dịch ngầm định
+ chi tiêu là hành động kí vào giao dịch để chuyển giá trị từ chủ cũ đến chủ mới

##### 3.2 Chuỗi giao dịch:
+ khoản thanh toán Alice ở quán cà phê lấy đầu ra của giao dịch trc đó với Joe làm đầu vào
+ giao dịch với Joe tạo 1 giá trị bitcoin đc khóa bằng khóa của Alice để chứng minh cô sở hữu lượng 	bitcoin này
+ Bob tham chiếu đến giao dịch này, lấy nó làm đầu vào cho giao dịch trả tiền cà phê của Alice và 	tạo ra đầu ra mới là trả tiền và tiền thừa
+ các giao dịch này tạo thành 1 chuỗi
+ yêu cần Bob phải tạo ra chữ ký nếu muốn chi tiêu lượng bitcoin này

##### 3.3 Tạo tiền thừa trả lại:
+ nhiều giao dịch có đầu ra tham chiếu địa chỉ chủ sở hữu mới và cũ (tiền thừa trả lại)
+ địa chỉ tiền thừa trả lại k nhất thiết là địa chỉ đầu vào, vì các lí do bảo mật thường sẽ là địa 	chỉ mới từ ví chủ sở hữu
+ các giao dịch chuyển giá trị từ đầu vào đến đầu ra, đầu vào là sự tham chiếu của giao 	dịch trc 	đó, đầu ra tham chiếu đến địa chỉ chủ mới và có thể địa chỉ chủ ban đầu nếu có tiền thừa, đầu ra 	tiếp tục là đầu vào của các giao dịch mới tạo nên một chuỗi quyền sở hữu

##### 3.4 Các dạng giao dịch thường gặp:
+ giao dịch phổ biến nhất là thanh toán đơn giản từ địa chỉ này đến địa chỉ khác
+ giao dịch gộp các khoản tiền như đổi nhiều tờ tiền bé lấy đồng tiền lớn ngoài đời thực thỉnh 	thoảng ví tạo ra các giao dịch như vậy để dọn dẹp khoản tiền nhỏ ví đã nhận
+ giao dịch phân phối một đầu vào như trả lương cho nhân viên ngoài đời thực

<br />

### 4. Tạo giao dịch
##### 4.1 Chọn đầu vào phù hợp:
+ Có bản sao đầu ra từ các giao dịch trước đó mà chưa chi tiêu
+ Nếu k có bản sao đầu ra, sẽ gọi API để lấy từ nút đầy đủ
+ Nếu đầu ra chưa chi tiêu ở giao dịch trc chưa đủ thì sẽ lấy thêm như cách lọc các đồng tiền lẻ 	đến khi đủ hoặc thừa để mua 1 món đồ

##### 4.2 Tạo đầu ra:
+ Đầu ra như một kịch bản tạo rào chắn cho khoản tiền giao dịch mà chỉ có lời giải phù hợp mới lấy 	đc khoản tiền đó, vd như Alice yêu cầu khóa phù hợp với địa chỉ của Bob nên chỉ có 	Bob mới có thể 	lấy đc khoản tiền giao dịch giữa cả 2
+ Đầu ra thứ 2 nếu có tiền thừa, ví của Alice tạo 2 đầu ra là thanh toán và tiền thừa trả lại về 	cho Alice
+ Để giao dịch có thể đc mạng bitcoin xử lí, ứng dụng của Alice sẽ thêm khoản phí nhỏ đc hiểu là 	phí giao dịch

<br />

### 5. Thêm giao dịch vào sổ cái
+ Truyền giao dịch
##### 5.1 Phát tán:
+ khi một nút bitcoin nhận đc 1 giao dịch mà nó chưa thấy trc đây, nó sẽ chuyển tiếp giao dịch đến 	các nút kết nối với nó (flooding)

##### 5.2 Góc nhìn của Bob:
+ Nếu ví của Bob kết nối trực tiếp với ví Alice thì có thể ví của Bob sẽ nhận đc giao dịch này đầu 	tiên, dù k phải thì Bob vẫn nhận đc giao dịch này
+ Ví của Bob sẽ xác định đc đây là giao dịch thanh toán gửi đến mình, cũng có thể độc lập kiểm tra 	tính đúng đắn của giao dịch

<br />

### 6. Đào bitcoin
+ Giao dịch của Alice đc phát tán trên mạng bitcoin nhưng phải đến khi đc xác nhận và đưa vào block 	thông qua quá trình đào (mining) thì mới trở thành 1 phần của blockchain
+ Các giao dịch đc nhóm lại thành các block, quá trình đòi hỏi một lượng tính toán khổng lồ để chứng 	minh nhưng chỉ cần một lượng tính toán nhỏ để xác thực
+ Quá trình đào phục vụ 2 mục đích:
	- Các nút đào xác thực giao dịch bằng cách tham chiếu đến các quy tắc đồng thuận của bitcoin
	- Hoạt động đào tạo ra bitcoin mới trong các block
+ Đào bitcoin thành công sẽ đc thưởng là lượng bitcoin mới và phí giao dịch
+ Có thể hình dung việc đào coin như giải bài toán sodoku khổng lồ, câu đố trong bitcoin dựa trên 	hàm băm mật mã (khó giải nhưng dễ kiểm tra), hàm băm mật mã SHA256
+ việc tìm ra lời giải trong đào coin (pow) đòi hỏi hàng triệu phép tính trên mạng bitcoin

<br />

### 7. Đào giao dịch trong block:
+ Các giao dịch mới từ phía ng dùng hoặc các ứng dụng khác liên tục đổ vào mạng bitcoin
+ Khi tạo block mới, các thợ đào thêm vào các giao dịch chưa đc xác thực rồi chứng minh tính xác 	thực block mới bằng PoW
+ Các giao dịch đc đưa vào block theo thứ tự ưu tiên giao dịch có phí cao nhất trc và một số tiên 	phí khác
+ Mỗi thợ đào sẽ đưa vào block của mình một giao dịch đặc biệt
+ Giao dịch của Alice đc mạng blockchain ghi nhận và đưa vào vùng chứa giao dịch chưa đc xác 	nhận, sau khi đc xác nhận sẽ đc đưa vào 1 block
+ Block thắng cuộc trở thành 1 phần của blockchain, mỗi block đc đào trên nền block chứa giao dịch 	của Alice đc tính thêm 1 lượt xác nhận cho giao dịch đó
+ Một block có từ 6 lượt xác nhận trở lên đc coi là không thể thay đổi

<br />

### 8. Chi tiêu giao dịch:
+ Bây giờ giao dịch của Alice đã đc nhúng vào blockchain, trở thành một phần của sổ cái phân tán
+ Các phần mềm nút đầy đủ có thể truy nguồn từ lúc bitcoin đc tạo ra lần đầu tăng dần lên từ giao 	dịch này đến giao dịch khác
+ Các phần mềm nút rút gọn có thể gọi xác minh thanh toán giản lược bằng cách nhận biết giao dịch đã 	có trong blockchain và đã có vài block đc đào sau nó
+ Lúc này Bob đã có thể chi tiêu đầu ra từ giao dịch của anh và Alice, mở rộng thêm cho chuỗi giao 	dịch ban đầu