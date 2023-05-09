# Chương 6: Giao dịch

### 1. Chi tiết các giao dịch:
+ Đằng sau các giao dịch:
    + Các giao dịch thường khác so với giao dịch hiện lên trên trình duyệt block
    + Dùng lệnh getrawtransaction và decoderawtransaction của giao dịch Alice với Bob sẽ k thấy các thông tin như địa chỉ của Alice, Bob hay 0.1 BTC của Alice

<br />

### 2. Đầu ra và đầu vào giao dịch:
+ Yếu tố cơ bản của giao dịch bitcoin là đầu ra giao dịch (khối tiền bitcoin k thể chia tách, đc ghi vào blockchain). Đầu ra giao dịch chưa chi tiêu (UTXO), UTXO phình to khi có nhiều đầu ra, thu nhỏ khi các đầu ra đc chi tiêu
+ Khi ví của 1 ng vừa nhận đc bitcoin có nghĩa là nó phát hiện ra một UTXO và có thể dùng khóa nó đang kiểm soát để chi tiêu UTXO này -> Số dư tk bitcoin có nghĩa là tất cả UTXO mà ví ng dùng có thể chi tiêu. Các UTXO nằm rải rác trong các giao dịch và block -> ví quét để cộng tổng UTXO của ví mà dùng khóa của nó để chi tiêu
+ Một đầu ra giao dịch là 1 số nguyên bất kì (bội số của satoshi), đầu ra của giao dịch là các dơn vị rời rạc, k thể bị chia nhỏ -> chỉ có thể chi tiêu toàn bộ đầu ra trong 1 giao dịch
+ Nếu UTXO lớn hơn giá trị muốn giao dịch sẽ tạo 2 đầu ra và trả tiền thừa -> vd: 1 UTXO 20 bitcoin giao dịch trả 1 BTC thừa 19 BTC trả lại (tiêu hết 20 BTC và trả lại 19 BTC)
+ Khi giao dịch sẽ cộng các UTXO lại để lớn hơn số tiền cần giao dịch và nhận lại tiền thừa
+ Các UTXO là đầu vào giao dịch khác và tạo ra đầu ra mới -> chuỗi giao dịch chi tiêu tạo UTXO
+ Giao dịch đặc biệt coinbase (giao dịch đầu tiên trong block). Giao dịch này đc thợ đào thắng cuộc đưa vào block và nó tạo ra lượng bitcoin mới trả cho thợ đào -> giao dịch này k chi tiêu UTXO, có đầu vào là coinbase

##### 2.1 Đầu ra giao dịch:
+ Mọi giao dịch đều tạo đầu ra và chúng đc ghi vào sổ cái bitcoin, gần như tất cả đầu ra đều tạo UTXO sẽ đc chi tiêu trong tương lai
+ Đầu ra giao dịch gồm 2 phần:
	- Một lượng bitcoin đc tính bằng mệnh gia satoshi
	- Một câu đố mật mã (kịch bản khóa, kịch bản nhân chứng, scriptPubKey) quyết định các 	điều kiện cần thiết để chi tiêu đầu ra
+ Đầu ra giao dịch của Alice là một mảng (2 đầu ra), trong mỗi phẩn tử mảng có value là giá trị bitcoin (satoshi) và scriptPubKey để xác định đk chi tiêu value này

##### 2.2 Chuỗi hóa giao dịch đầu ra:
+ Khi các giao dịch đc truyền trên mạng bitcoin hay trao đổi giữa các ứng dụng sẽ đc chuỗi hóa -> hầu hết các thư viện và nền tảng bitcoin không lưu nội bộ giao dịch dưới dạng byte mà lưu trong các cấu trúc dữ liệu (OOP)
+ Quá trình chuyển đổi từ byte sang cấu trúc dữ liệu là giải chuỗi (deserialize) hay phân tích giao dịch (transaction parsing), quá trình ngược lại là chuỗi hóa

##### 2.3 Đầu vào giao dịch:
+ Các đầu vào giao dịch xác định (tham chiếu) UTXO sẽ chi tiêu và cung cấp bằng chứng chủ sở hữu thông qua kịch bản mở khóa
+ Để tạo giao dịch, ví chọn UTXO đủ giá trị mà nó kiểm soát -> ví tạo đầu vào đến UTXO, mở khóa nó bằng kịch bản mở khóa
+ Hợp phần đầu vào: phần đầu tiên là con trỏ chỉ đến UTXO (thông qua mã băm giao dịch và stt trong blockchain), phần thứ hai là kịch bản mở khóa thỏa mãn điều kiện trong UTXO (thông 	thường là địa chỉ và chữ kí), phần thứ 3 là một số thứ tự
+ Đầu vào giao dịch của Alice gồm id của giao dịch (tham chiếu đến giao dịch UTXO đc chi tiêu), vout xác định UTXO (tham chiếu UTXO để lấy kịch bản khóa r mới tạo kịch bản mở khóa để chi tiêu), scriptSig là kịch bản mở khóa, một số chuỗi
+ Phải lấy về (tham chiếu) đc UTXO thì mới biết đc kịch bản khóa -> tạo kịch bản mở khóa và giá trị UTXO có đủ làm đầu vào giao dịch hay k

##### 2.4 Chuỗi hóa giao dịch - đầu vào:
+ Đầu vào đc mã hóa thành dãy byte

##### 2.5 Phí giao dịch:
+ Mọi giao dịch đều có phí để trả công cho thợ đào, đóng vai trò an ninh làm cho các ý đồ tấn công tắc nghẽn mạng lưới với lượng giao dịch ồ ạt k khả thi về mặt kinh tế
+ Phí giao dịch đc tính bằng kích thước giao dịch theo KB (đc định đoạt trên lực lượng thị trg) -> thợ đào sắp xếp thứ tư ưu tiên giao dịch trên nhiều phương diện, 1 giao dịch có phí cao có thể đc đưa vào block trc, giao dịch k có phí có thể đc đưa vào block sau hoặc k
+ minerelaytxfee (phí chuyển tiếp), giá hiện nay là 0.00001 BTC (1% milibitcoin) -> phí dưới phí này sẽ coi như k trả phí và đưa vào dĩ vãng
+ Bất cứ dịch vụ bitcoin nào tạo ra các giao dịch cũng phải trả phí linh động (qua bên thứ 3 hoặc tích hợp sẵn)
+ Các thuật toán ước tính phí sẽ tính ra mức phí phù hợp cho giao dịch. Hầu hết các dịch vụ cho phép ng dùng trả phí -> cao thì sẽ đc ưu tiên, thấp sẽ đc xác nhận lâu hơn
+ Nhiều ứng dụng tính phí bên thứ 3

##### 2.6 Thêm phí vào giao dịch
+ Cấu trúc dữ liệu giao dịch k có trường dành cho phí, phí đc tính bằng tổng đầu vào trừ tổng đầu ra
+ Nếu giao dịch phải tính kĩ, vd như đầu vào giao dịch là 20 BTC, bạn quên k thiết kế đầu ra trả lại 19 BTC thì nó sẽ đc xem là phí và thợ đào sẽ nhận nó
+ Alice mua cà phê có trị giá 0.015 BTC và phí 0.001 BTC, Alice có sẵn UTXO 0.2 BTC -> ví sẽ tạo 1 đầu ra 0.015 BTC đến quán của Bob và 0.184 BTC tiền thừa còn 0.001 BTC sẽ là phí

<br />

### 3. Kịch bản giao dịch và ngôn ngữ kịch bản
+ Ngôn ngữ kịch bản giao dịch bitcoin (script) là ngôn ngữ thực thi dựa trên ngăn xếp dạng forth dùng postfix notation
+ Kịch bản là một ngôn ngữ đơn giản, có thể chạy trên nhiều phần cứng, đòi hỏi mức độ xử lí tối thiểu và k thực hiện đc tác vụ phức tạp
+ Ngày nay hầu hết các giao dịch đều có dạng trả đến địa chỉ của Bob và dựa trên kịch bản trả đến mã băm khóa công khai P2PKH

##### 3.1 Tính turing không đầy đủ
+ Ngôn ngữ kịch bản giao dịch bitcoin chứa nhiều toán tử nhưng bị giới hạn 1 cách chủ ý: điều khiển luồng điều kiện nhưng k thể điều khiển luồng phức tạp khác
+ Không thể dùng ngôn ngữ này tạo các vòng lặp vô hạn (bom logic) để nhúng vào giao dịch r tạo cuộc tấn công từ chối dịch vụ

##### 3.2 Xác minh phi trạng thái
- Ngôn ngữ giao dịch bitcoin mang tính phi trạng thái (k có trạng thái nào trc khi thực thi cũng như sau thực thi) -> toàn bộ thông tin chứa trong kịch bản
- Nếu bạn xác minh một kịch bản thì các máy khác cũng thế -> giao dịch là hợp lệ với tất cả mọi ng

##### 3.3 Xây dựng kịch bản (khóa + mở khóa)
- Cỗ máy xác thực giao dịch bitcoin dựa vào 2 kịch bản: khóa và mở khóa
- Kịch bản khóa là 1 điều kiện đặt trong đầu ra: đk để đc chi tiêu đầu ra. Thường đc gọi là scriptPubKey (thường là khóa công khai hoặc địa chỉ)
- Kịch bản mở khóa là kịch bản thỏa mãn các đk do kịch bản khóa đặt ở đầu ra và cho phép chi tiêu đầu ra. Thông thường chúng chứa chữ kí số do ví ng dùng tạo từ khóa bí mật (scriptSig)
- Mỗi nút xác thực bitcoin sẽ xác thực giao dịch bằng cách thực thi kịch bản khóa và mở khóa cùng nhau
- Các UTXO đc lưu trên blockchain vĩnh viễn -> k bị ảnh hưởng bởi các chi tiêu k thành công -> khi thành công mới bị loại khỏi tập UTXO

##### 3.4 Ngăn xếp thực thi kịch bản
- Ngôn ngữ kịch bản bitcoin dựa trên ngăn xếp vì cấu trúc dữ liệu đc sử dụng là ngăn xếp (stack)
- Ngôn ngữ kịch bản thực thi kịch từng đối tượng từ trái qua phải

##### 3.5 Một kịch bản đơn giản
- Hầu hết các kịch bản khóa đều tham chiếu đến mã băm khóa công khai (địa chỉ) từ đó yêu cầu bằng chứng sở hữu để chi tiêu tiền
- Bất cứ sự kết hợp kịch bản khóa và mở khóa trả về true đều hợp lệ

##### 3.6 Thực thi riêng kịch bản khóa và mở khóa
- Trong phần mềm bitcoin nguyên thủy, kịch bản mở khóa và khóa đc nối với nhau r thực hiện tuần tự -> nếu kịch bản mở khóa độc hại sẽ đẩy dữ liệu vào ngăn xếp làm hỏng kịch bản khóa
- Trong bản thực thi hiện tại, các kịch bản đc thực hiện riêng rẽ và ngăn xếp đc di chuyển qua lại giữa 2 lần thực thi
- Đầu tiên dùng trình thực thi ngăn xếp thực thi kịch bản mở khóa -> nếu k lỗi ngăn xếp đc sao chép và thực thi kịch bản khóa -> nếu true thì đc chi tiêu UTXO, false thì k

##### 3.7 Trả-đến-mã-băm-khóa-công-khai (P2PKH)
- Đa số các giao dịch chi tiêu đầu ra bị khóa bằng kịch bản trả-đến-mã-băm-khóa-công-khai "P2PKH" (địa chỉ bitcoin)
- Có thể dùng kịch bản P2PKH để mở khóa đầu ra bằng cách trình ra 1 khóa công khai và chữ kí

<br />

### 4. Chữ kí số (ECDSA)
- Thuật toán chữ kí số đc dùng trong bitcoin là thuật toán chữ kí số đg cong elliptic (ECDSA), dựa trên cặp khóa bí mật/công khai, đc dùng bởi các hàm kịch bản OP_CHECKSIG, OP_CHECKSIGVERIFY....
- Trong bitcoin, chữ kí số phục vụ 3 mục đích:
    + Chứng minh chủ sỡ hữu khóa bí mật (chủ sỡ hữu tiền), cho phép chi tiêu số tiền này
    + Bằng chứng của sự cho phép này là k thể phủ nhận (k thể chối bỏ)
    + Chứng minh rằng giao dịch này chưa bị và k thể bị thay đổi bởi bất kì ai sau khi đc kí
- Mỗi đầu vào giao dịch đc kí độc lập

##### 4.1 Chữ kí số hoạt động như nào
- Là mô hình toán học gồm 2 phần:
    + Thuật toán dùng khóa bí mật để tạo chữ kí từ 1 tin nhắn
    + Thuật toán cho phép bất cứ ai cũng xác minh chữ kí này nếu biết tin nhắn và khóa công khai

- Tạo chữ kí số:
    + Trong ECDSA, tin nhắn là giao dịch (mã băm của 1 tập con cụ thể của dữ liệu trong giao dịch), khóa để kí là khóa bí mật của ng dùng: Sig = F_sig(F_hash(m), d_A)
    + Hàm F_sig tạo ra 1 Sig có 2 giá trị R, S -> đc chuỗi hóa thành 1 dãy byte bằng mô hình chuẩn hóa DER

- Chuỗi hóa chữ kí (DER) -> đọc

##### 4.2 Xác minh chữ kí
- Để xác minh cần có: chữ kí (R và S), giao dịch chuỗi hóa, khóa công khai (ứng với khóa bí mật tạo chữ kí)
- Thuật toán lấy chữ kí từ tin nhắn, tin nhắn và khóa công khai -> TRUE nếu chữ kí hợp lệ

##### 4.3 Các kiểu băm chữ kí (SIGHASH)
- Chữ kí đc áp dụng trong các giao dịch bitcoin, ngầm định một cam kết của ng kí đối với giao dịch cụ thể
- Các chữ kí bitcoin có phương thức chỉ ra phần nào đc đưa vào mã băm kí theo SIGHASH (là byte lẻ đc thêm vào cuối chữ kí). Mọi chữ kí đều có cờ hiệu SIGHASH
- Một giao dịch nhiều đầu vào có nhiều chữ kí với SIGHASH khác nhau
- Có 3 cờ hiệu SIGHASH:
    + ALL: áp dụng cho cả tất cả đầu vào và ra
    + NONE: áp dụng cho tất cả đầu vào, k áp dụng cho đầu ra
    + SINGLE: áp dụng tất cả đầu vào và 1 đầu ra với số thứ tự tương ứng đầu vào
    + ANYONECANPAY: kết hợp với các cờ hiệu trên, chỉ có 1 đầu vào đc kí
- Áp dụng SIGHASH: tạo bản sao giao dịch -> chuỗi hóa bản sao -> thêm SIGHASH và cuối và băm -> mã băm là tin nhắn đc kí
- Đọc thêm các loại SIGHASH

##### 4.4 Toán ECDSA (đọc)

##### 4.5 Tầm quan trọng của tính ngẫu nhiên trong chữ kí
- Trong phần toán ECDSA, khóa ngẫu nhiên k làm nền tảng cho khóa bí mật/công khai tạm thời, nếu dùng k tạo 2 chữ kí trên giao dịch khác nhau sẽ làm lộ khóa bí mật
- Để tránh nguy cơ này, k nên tạo k bằng bộ tạo số ngẫu nhiên đc gieo giống bằng entropy mà sử dụng quá trình ngẫu nhiên tất định đc gieo giống bằng dữ liệu giao dịch -> mỗi giao dịch sẽ có k khác

<br />

### 5. Địa chỉ bitcoin, số dư tài khoản, các khái niệm trừu tượng khác
- Các giao dịch k chứa địa chỉ bitcoin mà hoạt động thông qua kịch bản khóa và mở khóa
- Ví dụ về giao dịch mua cà phê của Alice:
    + Hiển thị địa chỉ ng gửi là Alice, trên thực tế, trình duyệt blockchain lấy về giao dịch này cũng như giao dịch trc đó, lấy tham chiếu UTXO có kịch bản khóa là mã băm khóa công khai của Alice (địa chỉ bitcoin) và trích xuất nó dưới dạng Base58Check để hiển thị
    + Hiển thị ng nhận là Alice và Bob, thực chất là trích xuất lấy kịch bản khóa của đầu ra giao dịch (P2PKH) và hiển thị lên trình duyệt
- Ví dụ về số dư tk Bob:
    + Để hiển thị tổng nhận, trình duyệt giải mã Base58Check để lấy mã băm khóa công khai -> tìm trong cơ sở dữ liệu giao dịch đầu ra có kịch bản khóa P2PKH nếu đúng địa chỉ sẽ tính tổng đầu ra
    + Hiển thị số dư bằng cách theo dõi tập UTXO chưa chi tiêu trên cơ sở dữ liệu