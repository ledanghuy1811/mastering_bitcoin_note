# Chương 3: Bitcoin core, thực thi tham chiếu

- Bitcoin là một dự án nguồn mở, bạn có thể tải về miễn phí và sử dụng, ban đầu do Satoshi Nakamoto phát triển về sau có nhiều ng hơn tham gia vào.
- Bitcoin core là bản thực thi tham chiếu của bitcoin (chính thức, có độ tin cậy cao): thực thi mọi khía cạnh của bitcoin gồm các ví, xác thực giao dịch, block, nút mạng đầy đủ

### 1. Môi trường phát triển bitcoin:
+ thiết lập một môi trg phát triển với tất cả các công cụ, thư viện và phần mềm để hỗ trợ viết các ứng dụng bitcoin

<br />

### 2. Dịch bitcoin từ mã nguồn:
+ Có thể tải mã nguồn dưới dạng file .zip trên github hoặc clone nó về
+ Clone bitcoin về và xem trong thư mục bitcoin => \$ cd bitcoin

##### 2.1 Chọn phiên bản phát hành trong bitcoin core:
+ Trước khi biên dịch, chọn một phiên bản cụ thể bằng thẻ tag => \$ git tag
+ Các phiên bản ứng cử (RC) có hậu tố "rc", các bản phát hành ổn định thì không có. Chọn phiên bản phát hành cao nhất (hiện tại là v0.11.2)
+ đông bộ mã nguồn => \$ git checkout v0.11.2, kiểm tra => \$ git status

##### 2.2 Thiết lập cấu hình xây dựng bitcoin core:
+ Đọc phần hướng dẫn xây dựng trong file doc/build-unix.md (k có) và cài các thư viện có liên quan (mình sẽ k cài). Sau khi cài đặt hết các đk, bắt đầu xây dựng bằng kịch bản autogen.sh => $ ./autogen.sh
+ autogen.sh tạo ra các kịch bản nhằm điều tra tìm ra các thiết lập chính xác, đảm bảo có đủ các thư viện cần thiết để biên dịch mã nguồn. Phần quan trọng nhất là configure, gõ => $ ./configure --help để xem thêm
+ Thay thế hành vi mặc định của configure: --prefix=$HOME, --disable-wallet, --with-imcompatible-wallet, --with-gui=no
+ Chạy $ ./configure 

##### 2.3 Xây dựng phần thực thi bitcoin core:
+ Quá trình có thể dài tùy thuộc vào CPU và bộ nhớ hệ thống
+ Nếu có lỗi gõ lệnh $ make để tiếp tục
+ Cài đặt chương trình thực thi khác: $ sudo make install
+ bitcoin ddc đặt ở /usr/local/bin, kiểm tra: $ which bitcoind (\$ which bitcoin-cli)

<br />

### 3. Vận hành một nút bitcoin core:
+ Ng vận hành các nút bitcoin có tầm nhìn trực tiếp, chính xác về blockchain bitcoin, có bản sao cục bộ tất cả các giao dịch. Khi vận hành một nút bitcoin có nghĩa đóng góp vào mạng bitcoin bằng cách làm nó vững chắc hơn
+ Vận hành nút bitcoin cần có hệ thống liên tục kết nối mạng và đủ tài nguyên. Bitcoin core mặc định giữ một bản sao đầy đủ của blockchain chứa mọi giao dịch từ 2009

##### 3.1 Tại sao bạn muốn chạy 1 nút:
+ phát triển phần mềm bitcoin, cần nút bitcoin cho các truy cập lập trình API đến mạng bitcoin và blockchain
+ xây dựng các ứng dụng phải xác thực giao dịch thông qua cơ chế đồng thuận của bitcoin
+ hỗ trợ bitcoin, vận hành một nút làm mạng chắc chắn hơn, phục vụ nhiều ví, ng dùng, giao dịch hơn
+ không muốn dựa vào bên thứ 3 để xác thực, xử lý giao dịch của mình

##### 3.2 Chạy bitcoin core lần đầu tiên:
+ Chạy lần đầu sẽ nhắc tạo file cấu hình với mk mạnh cho JSON-RPC, mk này kiểm soát quyền truy cập đến API do bitcoin core cung cấp: $ bitcoind
+ Xây dựng 1 file cấu hình với ít nhất 1 mục rpcuser, rpcpassword

##### 3.3 Cấu hình nút bitcoin core:
+ Tạo một file trong thư mục .bitcoin (.bitcoin/bitcoin.conf): 
- rpcuser=bitcoinrpc
- rpcpassword=CHANGE_THIS
+ Còn hơn 100 cấu hình khác, chạy: $ bitcoin --help để xem
+ Một số tùy chọn quan trọng: alertnotify, conf, datadir, prune, txindex, maxconnections, maxmempool, maxreceivebuffer/maxsendbuffer, minerelaxfee
+ Sau khi sửa file cấu hình, kiểm thử bitcoin với cấu hình này: \$ bitcoin -printtoconsole. Chạy bitcoin ở chế độ ngầm: \$ bitcoin -daemon. Giám sát tiến độ và trạng thái vận hành của nút bitcoin: $ bitcoin-cli getinfo

<br />

### 4. Giao diện lập trình ứng dụng bitcoin core API
+ Phần mềm bitcoin core tạo giao diện JSON-RPC có thể truy cập qua bitcoin-cli, gọi lệnh: $ bitcoin-cli help để xem thêm
+ Muốn thêm thông tin chi tiết gõ: $ bitcoin-cli help arg

##### 4.1 Lấy thông tin về trạng thái phần mềm bitcoin core
- Gõ lệnh $ bitcoin-cli getinfo: hiển thị thông tin cơ bản trạng thái của nút bitcoin, ví, cơ sở dữ liệu blockchain. Kết quả trả về dưới dạng JSON

##### 4.2 Khám phá và giải mã các giao dịch
- Lệnh getrawtransaction trả về một giao dịch đc biểu diễn dưới dạng một chuỗi các kí tự hệ hexa. Để giải mã dùng lệnh decoderawtransaction
- Khi giao dịch chưa đc xác nhận, ID giao dịch không có ý nghĩa. Việc k có mã băm của giao dịch trong blockchain k có nghĩa là giao dịch chưa đc xử lí. Sau xác nhận, txid là chính thức, k thể thay đổi

##### 4.3 Khám phá các block
- Lệnh getblockhash lấy chiều cao block làm tham số và trả về mã băm của block
- Lệnh getblock để truy vấn block

##### 4.4 Sử dụng giao diện lập trình của bitcoin
- RPC là remote procedure call, gọi hàm từ xa (trên nút bitcoin) thông qua giao thức mạng
- Nếu muốn thực hiện lệnh gọi hàm JSON-RPC, dùng thư viện http thông thường để tạo lệnh gọi hàm (hầu hết các ngôn ngữ đều có thư viện để gói API)

<br />

### 5. Các phần mềm, thư viện, bộ công cụ thay thế:
+ Trong hệ sinh thái bitcoin, có nhiều phần mềm, thư viện, bộ công cụ, bản thực thi nút đầy đủ thay thế đc viết bằng nhiều ngôn ngữ khác nhau