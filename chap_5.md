# Chương 5: Ví
- Ví là ứng dụng giao diện ng dùng chính, kiểm soát quyền truy nhập tiền của ng dùng, quản lí các khóa và địa chỉ, theo dõi số dư, tạo và kí giao dịch
- Ví là cấu trúc dữ liệu dùng để lưu trữ, quản lí các khóa ng dùng

### 1. Tổng quan về công nghệ ví:
+ Ví bitcoin k chứa bitcoin, chỉ chứa các khóa, bitcoin đc ghi lại vào blockchain, ng dùng kiểm soát tiền bằng các khóa trong ví để kí giao dịch -> có thể hiểu ví bitcoin là 1 chùm chìa khóa
+ Có 2 loại ví chính: 
    - Ví bất định: mỗi khóa đc tạo ra độc lập từ 1 số ngẫu nhiên, các khóa không liên quan đến nhau (ví JBOK)
    - Ví tất định: tất cả các khóa đc tạo ra từ khóa chính duy nhất (master key hoặc hạt giống seed), các khóa liên quan đến nhau và có thể đc tạo lại từ master key (cách tạo là ví tất định phân cấp ví HD) -> để dễ sử dụng các hạt giống đc mã hóa thành từ tiếng anh (từ mật mã trợ nhớ)

##### 1.1 Ví bất định (ngẫu nhiên):
+ Trong ví bitcoin đầu tiên (bitcoin core), ví là tập hợp các khóa ngẫu nhiên.
+ Điểm bất lợi của khóa ngẫu nhiên là nếu tạo nhiều khóa sẽ phải giữ sao lưu của tất cả các khóa (nếu không sẽ mất tiền do khóa kiểm soát)
+ Việc dùng lại địa chỉ làm giảm đi tính bảo mật do liên kết nhiều giao dịch và địa chỉ với nhau
+ Không nên dùng ví bất định Type-0 

##### 1.2 Ví tất định (đc gieo hạt):
+ Là ví chứa các khóa bí mật đc tạo ra từ một hạt giống chung bằng hàm băm 1 chiều
+ Hạt giống này là 1 số ngẫu nhiên sau đó đc kết hợp với dữ liệu khác (một mã số hay mã chuỗi chaincode) -> tạo ra các khóa bí mật
+ Chỉ cần hạt giống có thể khôi phục lại các khóa bí mật -> chỉ cần 1 bản sao lưu, nó còn cho phép chuyển qua lại tất cả các khóa của ng dùng giữa các bản thực thi ví tham chiếu

<br />

### 2. Ví HD (BIP-32/BIP-44):
+ Là dạng ví tất định tân tiến nhất đc định nghĩa theo chuẩn BIP-32, chứa các khóa đc tạo ra từ một cấu trúc hình cây: một khóa mẹ tạo ra chuỗi khóa con vv..
+ Ví HD có 2 ưu điểm chính so với ví bất định:
    - Dùng cấu trúc hình cây để biểu đạt về mặt tổ chức: một nhánh khóa con dùng để nhận khoản thanh toán đến, một nhóm khác dùng để nhận tiền thừa trả lại
    - Ng dùng có thể tạo một chuỗi khóa công khai mà k cần truy cập khóa bí mật tương ứng: sử dụng ví HD trên các máy chủ không an toàn hay dùng để nhận tiền

##### 2.1 Hạt giống và mã hỗ trợ BIP-39:
+ Ví HD còn hữu dụng hơn khi kết hợp với phương pháp tạo hạt giống chuẩn từ 1 chuỗi tiếng anh (cụm từ trợ nhớ mnemonic đc định nghĩa bởi BIP-39)

##### 2.2 Các thông lệ hiệu quả nhất về ví:
+ Các chuẩn chung của ví bitcoin:
	- Các từ mật mã trợ nhớ, thiết lập dựa trên BIP-39
	- Các ví HD, dựa trên BIP-32
	- Cấu trúc ví HD đa mục đích, dựa trên BIP-43
	- Các ví đa tiền tệ-tài khoản, dựa trên BIP-44
+ Một ng dùng có thể xuất một cụm trợ nhớ do các ví này tạo ra và nhập nó vào ví khác -> khôi phục lại toàn bộ giao dịch, khóa, địa chỉ

##### 2.3 Sử dụng ví bitcoin:
+ Gabriel sử dụng ví HD phần cứng Trezor để quản lí bitcoin: gồm 2 nút bấm để lưu khóa và kí giao dịch
+ Khi Gabriel sử dụng lần đầu -> nó tạo ra một cụm trợ nhớ và hạt giống -> ghi lại và đánh số từng từ -> cụm trợ nhớ dùng để khôi phục lại ví phần cứng, phần mềm...
+ Với bản thực thi đầu tiên trên website, Gabriel dùng 1 địa chỉ bitcoin duy nhất -> có nhược điểm và ví HD có thể khắc phục

<br />

### 3. Chi tiết về công nghệ ví
##### 3.1 Cụm từ mã trợ nhớ (BIP-39):
+ Là chuỗi từ tượng trưng cho (mã hóa) một số ngẫu nhiên đc dùng làm hạt giống tạo ví tất định: chỉ cần chuỗi này có thể tạo lại hạt giống r tạo lại các khóa, thường từ 12 đến 24 từ
+ Các mã trợ nhớ đc định nghĩa trong BIP-39

##### 3.2 Tạo các từ trợ nhớ:
+ Ví tự động tạo ra các từ trợ nhớ thông qua quá trình chuẩn hóa đc định nghĩa trong BIP-39: 
	- Tạo 1 chuỗi ngẫu nhiên (1 entropy) 128 đến 256 bit
	- Tạo 1 checksum của chuỗi trên bằng cách lấy các bit đầu tiên (độ dài entropy 	chia 32) của mã băm SHA256 của nó
	- Thêm checksum vào cuối chuỗi ngẫu nhiên
	- Chia chuỗi thành các đoạn 11 bit
	- Ánh xạ từng giá trị 11 bit thành 1 từ trong từ điển 2048 từ cho trc
	- Mã trợ nhớ là chuỗi này

##### 3.3 Từ cụm trợ nhớ đến hạt giống:
+ Các từ trợ nhớ 128 đến 256 bit -> dùng entropy này tạo hạt giống dài hơn 512 bit bằng hàm kéo dài khóa PBKDF2 -> dùng hạt giống tạo ví tất định rồi tạo khóa
+ Hàm kéo dài khóa có 2 tham số: cụm trợ nhớ và hạt muối (hạt muối dùng để gây khó khăn khi tìm kiếm vét cạn, trong BIP-39 nó còn dùng để tạo passphrase để tăng cường bảo vệ hạt giống
+ Quá trình:
	- Tham số đầu tiên cho hàm PBKDF2 là cụm trợ nhớ
	- Tham số thứ 2 cho hàm PBKDF2 là hạt muối (1 hằng số dạng chuỗi mnemonic nối với 	passphrase)
	- PBKDF2 băm 2048 lần với thuật toán HMAC-SHA512 tạo ra hạt giống 512 bit

##### 3.4 Passphrase tùy chọn trong BIP-39:
+ Cho phép sử dụng một passphrase tùy chọn trong tạo hạt giống. Trên thực tế, với cụm trợ nhớ cho trc, mỗi passphrase tiềm năng sẽ tạo ra hạt giống khác nhau
+ Có 2 tính năng quan trọng:
	- Một lớp bảo mật thứ 2 (một thứ đc nhớ): khiến cụm trợ nhớ trở nên vô dụng nếu đứng 1 mình
	- Một hình thức từ chối hợp lí (ví cưỡng ép): dùng passphrase đến một ví ít tiền
+ Rủi ro:
	- Nếu chủ sở hữu mất và không ai biết passphrase, hạt giống sẽ trở nên vô dụng và toàn bộ tiền trong ví sẽ mất
	- Nếu chủ sở hữu cất bản sao lưu passphrase cùng chỗ hạt giống sẽ làm mất tính bảo mật của lớp bảo vệ thứ 2

##### 3.5 Làm việc với mã trợ nhớ:
+ BIP-39 đc cài đặt như một thư viện trong nhiều ngôn ngữ lập trình khác nhau

##### 3.6 Tạo ví HD từ hạt giống:
+ Các ví HD đc tạo ra từ một hạt giống gốc đơn lẻ: một số ngãu nhiên 128, 256, 512 bit hoặc từ một cụm trợ nhớ
+ Mọi khóa trong ví HD đều đc tạo ra từ hạt giống -> dễ dàng sao lưu khi chuyển ví
+ Hạt giống gốc là đầu vào hàm băm HMAC-SHA512, kết quả là khóa bí mật chính (m) và mã chuỗi chính (c): khóa bí mật chính tạo khóa công khai chính M, mã chuỗi chính dùng để tạo entropy trong hàm tạo khóa con từ khóa mẹ

##### 3.7 Tạo khóa bí mật con:
+ Ví HD dùng hàm tạo khóa con CKD, dựa trên hàm băm 1 chiều kết hợp với:
	- 1 khóa bí mật hoặc công khai mẹ (khóa ECDSA k nén)
	- 1 hạt giống (mã chuỗi 256 bit)
	- 1 số thứ tự (32 bit)
+ Mã chuỗi đc dùng để đưa dữ liệu ngẫu nhiên tất định vào quá trình này -> biết số thứ tự và 1 khóa con k thể suy ra khóa con khác
+ Kết hợp khóa bí mật mẹ, mã chuỗi, số thứ tự -> băm HMAC-SHA512 -> mã băm 512 bit -> nửa phải 256 bit là mã chuỗi khóa con, nửa trái 256 bit thêm vào khóa mẹ tạo khóa con

##### 3.8 Sử dụng các khóa con đc tạo ra:
+ Khóa bí mật con và khóa bất định (ngẫu nhiên) k có gì khác nhau
+ Không thể dùng khóa con để tìm ra khóa mẹ hay khóa anh em, k thể tạo ra khóa cháu nếu k có mã chuỗi con
+ Một mình khóa bí mật con tạo ra khóa công khai và địa chỉ -> dùng địa chỉ đó để chi tiêu, giao dịch như địa chỉ bình thường

##### 3.9 Khóa mở rộng:
+ Là kết hợp của khóa và mã chuỗi, đc lưu bằng cách ghép 256 bit khóa với 256 bit mã chuỗi thành 512 bit
+ Có 2 loại khóa mở rộng: khóa bí mật mở rộng (khóa bí mật + mã chuỗi), khóa công khai mở rộng (khóa công khai + mã chuỗi)
+ Khóa bí mật mở rộng có thể tạo ra cây HD hoàn chỉnh, khóa công khai mở rộng chỉ tạo ra nhánh khóa công khai
+ Khóa mở rộng đc mã hóa bằng Base58Check có tiền tố xprv hoặc xpub

##### 3.10 Tạo khóa công khai con:
+ Có thể tạo khóa công khai con từ khóa công khai mẹ hoặc khóa bí mật con
+ Có thể tạo ra vô số khóa công khai và địa chỉ bitcoin trên máy chủ k an toàn, k thể chi tiêu số tiền gửi đến địa chỉ đó nhưng trên 1 máy chủ an toàn có thể tạo các khóa bí mật tương ứng để chi tiêu khoản tiền đó
+ Các ứng dụng phổ biến là: cài đặt khóa công khai mở rộng trên máy chủ nền web, lưu trữ lạnh và các ví phần cứng

##### 3.11 Sử dụng khóa công khai mở rộng trên các website bán hàng:
+ Gabriel lấy địa chỉ bitcoin đầu tiên của Trezor làm địa chỉ bitcoin duy nhất cho website, mọi giao dịch đều đến địa chỉ đó -> khối lượng vài đơn mỗi tuần thì nó ổn
+ Khi có quá nhiều đơn sẽ trở nên quá tải -> tạo khóa công khai mở rộng với mỗi đơn hàng sẽ có địa chỉ riêng -> Gabriel chi tiêu tiền từ thiết bị Trezor

##### 3.12 Tạo khóa con gia cố:
+ Tạo khóa công khai từ xpub rất hữu dụng nhưng nếu biết 1 khóa bí mật con và mã chuỗi sẽ rất nguy hiếm -> biết đc các khóa bí mật con khác hoặc khóa bí mật mẹ
+ Dùng hàm tạo khóa gia cố (thay khóa công khai mẹ bằng khóa bí mật mẹ tạo ra mã chuỗi con)

##### 3.13 Số thứ tự để tạo khóa gia cố và khóa thường:
+ Là số nguyên 32 bit, từ 0 -> 2^31-1 dùng để tạo khóa thông thường, từ 2^31 đến 2^32-1 dùng để tạo khóa gia cố (số thứ tự của khóa gia cố sẽ có dấu ' trên đầu)

##### 3.14 Định danh (đg dẫn) khóa ví HD:
+ Quy ước đặt tên theo kiểu đg dẫn: khóa bí mật đầu tiên là m, khóa công khai đầu là M
+ m/0/1 nghĩa là khóa m có khóa bí mật con đầu tiên và có khóa bí mật cháu thứ 2

##### 3.15 Duyệt cấu trúc hình cây của ví HD:
+ BIP-44 quy định cấu trúc của cây:
+ m -> mục đích -> loại coin -> tài khoản -> tiền thừa -> thứ tự địa chỉ