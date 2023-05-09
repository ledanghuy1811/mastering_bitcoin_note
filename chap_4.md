# Chương 4: Khóa, địa chỉ

### 1. Giới thiệu:
+ Quyền sở hữu bitcoin đc thiết lập thông qua các khóa số, địa chỉ bitcoin, chữ kí số: đc lưu trữ trong ví. Các khóa số trong ví độc lập với giao thức bitcoin.
+ Để đc đưa vào blockchain, các giao dịch bitcoin phải có chữ kí số hợp lệ, chỉ có thể tạo ra bằng khóa bí mật. Chữ kí số dùng để chi tiêu bitcoin đc gọi là nhân chứng (witness)
+ Các khóa đi theo cặp (khóa bí mật, công khai)
+ Trong phần thanh toán giao dịch, khóa công khai đc đại diện bằng vân tay số (địa chỉ bitcoin). Thông thường địa chỉ bitcoin đc tạo ra từ khóa công khai và tương ứng với khóa đó

##### 1.1 Mật mã khóa công khai và tiền mã hóa:
+ Mật mã khóa công khai mang tính bất khả nghịch, dựa vào đó tạo ra các bí mật số và chữ cái số không thể làm giả, bitcoin sử dụng đg cong eliptic làm nền tảng
+ Tạo ra 1 cặp khóa: khóa công khai để nhận tiền, khóa bí mật để kí chi tiêu. Có thể xác thực chữ kí khớp với khóa công khai mà k làm lộ khóa bí mật
+ Khi chi tiêu, chũ sở hữu sẽ trình ra khóa công khai và chữ kí (mỗi giao dịch chữ kí sẽ khác nhau), từ đó mọi thành viên trên bitcoin có thể xem đó là giao dịch hợp lệ hay không

##### 1.2 Khóa bí mật và khóa công khai:
+ Một ví bitcoin chứa nhiều cặp khóa, mỗi cặp gồm 1 khóa bí mật, 1 khóa công khai
+ Khóa bí mật là 1 số ngẫu nhiên -> dùng phép nhân đg cong elliptic tạo khóa công khai -> dùng hàm băm 1 chiều tạo địa chỉ bitcoin

##### 1.3 Khóa bí mật:
+ Quyền kiểm soát khóa bí mật tạo nên nguồn gốc kiểm soát lượng bitcoin gắn với địa chỉ tương ứng
+ Khóa bí mật tạo ra chữ kí để kí các giao dịch nhằm chứng minh quyền sở hữu lượng tiền
+ Cần giữ bí mật và tránh để bị mất vì k thể khôi phục -> mất tiền khóa đó bảo vệ

##### 1.4 Tạo khóa bí mật từ 1 số ngẫu nhiên:
+ Bước đầu tiên tìm nguồn entropy an toàn (sự ngẫu nhiên), phần mềm bitcoin tạo 256 bit entropy
+ Chọn số 256 bit và kiểm tra nó nhỏ hơn n-1, có thể dùng hàm băm SHA256
+ Tạo khóa bí mật bitcoin core dùng lệnh getnewaddress (chỉ hiện thị khóa công khai) -> hiển thị khóa bí mật bằng lệnh dumpprivate (hiển thị khóa bí mật bằng dịnh dạng Base58 đc mã hóa bằng checksum gọi là định dạng nhập ví WIF)

##### 1.5 Khóa công khai:
+ Đc tạo ra từ khóa bí mật bằng phép nhân đg cong elliptic: K = k*G (K là khóa công khai, k là khóa bí mật, G là điểm bất biến gọi là phần tử sinh generator point)

##### 1.6 Giải thích mật mã đg cong elliptic (đọc)

##### 1.7 Tạo khóa công khai:
+ đc tạo ra từ khóa bí mật và k thể truy ra đc khóa bí mật từ khóa công khai
+ Hầu hết các bản thực thi bitcoin đều sử dụng thư viện mật mã OpenSSL để thực hiện phép toán đg cong elliptic

<br />

### 2. Địa chỉ bitcoin:
+ Đc tạo ra từ khóa công khai gồm dãy các số và chữ, bắt đầu bằng số 1
+ Địa chỉ bitcoin xuất hiện trong các giao dịch dưới dạng ng nhận tiền. Một địa chỉ bitcoin có thể đại diện cho ng sở hữu cặp khóa bí mật/công khai hoặc đại diện cho thứ khác như kịch bản thanh toán
+ Địa chỉ bitcoin đc tạo ra từ khóa công khai và hàm băm mật mã 1 chiều (SHA256, RIPEMD160)
+ Khóa công khai K -> băm SHA256 -> băm RIPEMD160 -> mã băm (20 byte/160 bit) -> mã hóa Base58Check -> địa chỉ bitcoin
+ Mã hóa Base58 và Base58Check (đọc)

<br />

### 3. Các định dạng khóa
##### 3.1 Các định dạng khóa bí mật:
+ Biểu diễn nhiều dạng khác nhau nhưng đều là số 256 bit: dạng hexa và nhị phân dùng nội bộ, WIF dùng để nhập xuất khóa giữa các ví và mã QR -> có thể chuyển đổi định dạng

##### 3.2 Giải mã từ Base58Check:
+ Dùng bitcoin explorer: $ bx base58check-decode mã -> kết quả trả về khóa trong payload, tiền tố phiên bản WIF 128, mã checksum

##### 3.3 Mã hóa từ hexa sang Base58Check:
+ $ bx base58check-encode mã --version WIF(128)

##### 3.4 Mã hóa từ hexa (khóa nén) sang Base58Check:
+ thêm 01 vào hậu tố r thực hiện như trên. Đinh dạng WIF nén thu về bắt đầu bằng K (ngụ ý có hậu tố 01)

##### 3.5 Các định dạng khóa công khai:
+ Thường là dạng khóa không nén (tiền tố 04) hoặc nén (tiền tố 02, 03). Biểu diễn bằng tiền tố theo sau là 2 số 256 bit (tổng 520 bit)

##### 3.6 Khóa công khai nén:
+ Đưa vào bitcoin để giảm kích cỡ giao dịch và tiết kiệm dung lượng ổ cứng các nút chứa cơ sở dữ liệu blockchain bitcoin
+ Khóa công khai trên đg cong elliptic (x, y), biết x tính đc y -> nên chỉ cần lưu x
+ y^2 mod p = (x^3 + 7) mod p, do y mũ 2 nên sẽ có 2 nghiệm âm dương, theo đg elliptic y sẽ có giá trị chẵn hoặc lẻ nên để phân biệt thì tiền tố 02 (y chẵn) và tiền tố 03 (y lẻ)
+ khóa công khai nén tương ứng vs 1 khóa bí mật, nếu đưa vào hàm băm kép RIPEMD160 và SHA256 sẽ tạo ra 1 địa chỉ khác -> nhầm lẫn 1 khóa bí mật sẽ có 2 địa chỉ bitcoin
+ Các phần mềm mới ra đời phải hỗ trợ khóa công khai nén: quét các giao dịch có khóa không nén hay nén -> để giải quyết, khi khóa bí mật xuất từ 1 ví, định dạng WIF sẽ khác đi trong ví bitcoin mới 

##### 3.7 Khóa bí mật nén:
+ Nhầm lẫn thuật ngữ: khóa bí mật chỉ nên dùng để tạo khóa công khai nén hoặc không, nên gọi định dạng xuất ra là WIF nén và WIF
+ Bản thân khóa bí mật không bị nén và không thể nén, xuất dạng WIF nén dài hơn 1 byte (hậu tố 01)
+ Trong base58, tiền tố 5 đổi sang K hoặc L nếu thêm 1 byte
+ Không thể sử dụng các định dạng này thay thế cho nhau (khi nhập ví mới, khóa bí mật xuất dạng WIF nén -> tạo khóa công khai nén, ví cũ hơn thì sẽ k nén -> báo cho ví tìm trên blockchain phải tìm địa chỉ nén hay k nén)
+ Nếu một ví bitcoin có thể tạo ra khóa công khai nén, nó sẽ dùng khóa đó trong tất cả các giao dịch, khóa bí mật sẽ đc dùng tạo ra các khóa công khai nén, các khóa công khai nén sẽ dùng để tạo ra địa chỉ bitcoin trong các giao dịch

<br />

### 4. Thực thi khóa và địa chỉ trong python (đọc)

<br />

### 5. Khóa và địa chỉ nâng cao
+ Khóa bí mật mã hóa BIP-38:
- Khóa bí mật phải đc bảo mật -> mâu thuẫn với tính sẵn sàng, phải lưu trữ các bản sao để tránh mất (lưu trong ví bằng mật khẩu, bằng usb, ví giấy) -> sự ra đời của BIP-38
- BIP-38 dùng chuẩn AES để mã hóa các khóa bí mật với một passphrase rồi mã hóa chúng bằng Base58Check
- Một sơ đồ BIP-38 nhận đầu vào là khóa bí mật (đc mã hóa WIF dưới dạng Base58Check với tiền tố 5) và passphrase -> kết quả là một khóa bí mật mã hóa Base58Check với tiền tố 6P (muốn chuyển lại khóa bí mật tiền tố 5 cần passphrase để giải mã lại)
- Đc sử dụng phổ biến trong các ví giấy, chỉ cần ng dùng chọn passphrase mạnh thì sẽ vô cùng an toàn

<br />

### 6. Địa chỉ trả đến mã băm kịch bản (P2SH) và địa chỉ đa chữ kí:
+ Địa chỉ bitcoin truyền thống bắt đầu bằng 1, các địa chỉ trả đến mã băm kịch bản (P2SH) bắt đầu bằng 3, bị gọi nhầm là địa chỉ đa chữ kí (multisig)
+ Chúng chỉ định ng thụ hưởng của giao dịch bitcoin dưới dạng mã băm kịch bản, thay vì nêu đích danh ng đó
+ Tiền gửi đến các địa chỉ 3 đòi hỏi mã băm khóa công khai, chữ kí khóa bí mật -> đưa vào 1 kịch bản và đc xác định tại thời điểm tạo địa chỉ, tất cả đầu vào đến địa chỉ này sẽ bị chặn bởi cùng các yêu cầu đó
+ Địa chỉ P2SH dc tạo ra từ kịch bản giao dịch xác định ng có thể chi tiêu đầu ra giao dịch
+ Các địa chỉ đa chữ kí và P2SH:
+ Tính năng đa chữ kí bitcoin đc thiết kế để yêu cầu M chữ kí trong tổng số N khóa, gọi là một multisig M-trong-N (M<=N)
+ Bob có thể dùng địa chỉ đa chữ kí 1-trong-2 từ 1 khóa của mình và vợ mình nhằm đảm bảo rằng một trong 2 ng có thể kí để chi tiêu một đầu ra giao dịch đc khóa vào địa chỉ này (như tk chung ngân hàng, vk hay ck kí đều có thể lấy đc tiền)

<br />

### 7. Địa chỉ Vanity:
+ Địa chỉ vanity là địa chỉ bitcoin hợp lệ chứa các thông điệp dưới dạng con ng có thể đọc đc -> đòi hỏi phải tạo và kiểm thử hàng tỉ khóa bí mật mới tìm đc địa chỉ vanity tương ứng
+ Sau khi tìm thấy địa chỉ vanity khớp với mẫu tương ứng, chủ sở hữu có thể dùng khóa bí mật tạo ra địa chỉ đó chi tiêu bitcoin như mọi địa chỉ khác (độ an toàn như mọi địa chỉ khác)
+ Eugenia muốn gây quỹ từ thiện cho trẻ em ở Philipins -> muốn tạo địa chỉ vanity 1Kids

##### 7.1 Tạo địa chỉ vanity:
+ Eugenia k thể tạo địa chỉ vanity "1KidsCharity" một cách nhanh chóng, với mẫu dài hơn 7 kí tự phải sử dụng các phần cứng chuyên dụng, tìm địa chỉ vanity trên các hệ thống GPU nhanh hơn nhiều so với CPU đa năng
+ Một cách khác để tìm kiếm địa chỉ vanity là thuê ngoài một mỏ đào địa chỉ vanity (dịch vụ những ng có phần cứng GPU tìm địa chỉ vanity cho ng khác)
+ Việc tạo một địa chỉ vanity là một cuộc tìm kiếm vét cạn: thử một khóa ngẫu nhiên -> kiểm tra địa chỉ khóa đó có như mẫu mong muốn k

##### 7.2 Độ an ninh của địa chỉ vanity:
+ Có thể sử dụng địa chỉ vanity để tăng cường và đánh bại các biện pháp an ninh: tăng cường an ninh khi là địa chỉ đặc biệt tránh kẻ khác lợi dụng tráo địa chỉ và gửi tiền, đánh bại an ninh khi có thể tạo ra địa chỉ tương tự và đánh lừa ng khác (giống các kí tự đầu)
+ Eugenia có thể dùng địa chỉ ngẫu nhiên hoặc địa chỉ vanity: nếu dùng cố định 1 địa chỉ thì kẻ xấu có thể lén tráo địa chỉ trên website và lừa mọi ng chuyển tiền vào đấy hoặc hắn có thể tạo ra địa chỉ có các kí tự đầu giống địa chỉ của Eugenia và lừa mọi ng
+ Địa chỉ vanity an toàn nếu số kí tự đầu đủ lớn vì kẻ phá hoại muốn tạo địa chỉ đánh lừa mọi ng thì phải tạo địa chỉ vanity với số kí tự đầu nhiều hơn -> k khả thi và tốn kém

<br />

### 8. Ví giấy:
+ Ví giấy là khóa bí mật bitcoin in trên giấy, là phương thức hiệu quả để tạo sao lưu bitcoin ngoại tuyến (lưu trữ lạnh), khắc phục tai nạn như hỏng ổ cứng, bị cướp, hacker, keylogger...
+ Ví giấy có nhiều hình dạng, loại thiết kế, nhưng đơn giản nhất là khóa bí mật và địa chỉ trên 1 tờ giấy
+ Có thể tạo ví giấy thông qua bộ tạo ví giấy dạng JavaScript chạy trên máy ng dùng từ địa chỉ bitaddress.org
+ Điểm bất lợi là các khóa in ra dễ bị trộm cắp -> hệ thống lưu trữ phức tạp hơn là dùng khóa bí mật mã hóa BIP-38 và passphrase
+ Gưi tiền vào ví giấy nhiều lần nhưng nên tiêu số tiền trong ví 1 lần: nếu tiêu ít hơn 1 ví có thể tạo địa chỉ trả lại tiền thừa hoặc nếu máy tính bị phá hoại có thể lộ khóa bí mật
+ Ví giấy có nhiều thiết kế, kích cỡ, tính năng khác nhau