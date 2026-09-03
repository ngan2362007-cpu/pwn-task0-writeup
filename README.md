# write up task 0: Kiến thức nền
## bit/byte and endianess:
  ### a. Bit/ byte:
**bit/byte:**
-Bit: đơn vị thông tin nhỏ nhất trong máy tính, chỉ nhận 0 hoặc 1  
-Byte: 1 nhóm gồm 8 bit.  
  1 byte = 8 bit  
ví dụ: 10101010 = 1 byte  
-Nubble: 1 nhóm gồm 4 bit ( nửa byte ).  
**LSB/MSB**
-LSB: bit/byte có GTNN (nằm ngoài cùng phía bên phải )  
-MSB: bit/byte có GTLN (nằm ngoài cùng phía bên trái )  
Ví dụ: 10000002 có LSB = 2 và MSB = 1.  
  ### b.Endianess ( thứ tự lưu trữ byte trong bộ nhớ )  
**Khái niệm:** là sắp xếp byte vào bộ nhớ.  
- Big endianess: byte cao nhất sẽ xếp vào vị trí thấp nhất.  
- Little endianess: byte thấp nhất sếp vào vị trí thấp nhất.
ví dụ: 012345678 có MSB = 12 và LSB = 78.  
Big endianess: '12 34 56 78'
Little endianess: '78 56 34 12'
## Các thanh ghi x86-64 phổ biến:  
- Thanh ghi là các ổ nhớ siêu nhỏ nhưng có tốc độ truy xuất nhanh nhất ( nằm trong cpu ).
- Có kích thước 64 bits ( 8 bytes )
  ### a. Thanh ghi mục đích chung:
- Lưu dữ liệu tạm thời, thực hiện tính toán hoặc truyền tham số khi gọi hàm.
- RAX:
  + Lưu giá trị trả về của hàm khi thực thi xong.
  + Lưu mã ID của syscall khi thực hiện systemcall ( '0' là read, '1' là write, '60' là exit )
- RDI, RSI, RDX, RCX, R8, R9: bộ 6 thanh ghi dùng để truyền 6 tham số đầu tiên vào 1 hàm.
- RBX: lưu trữ dữ liệu chung.
  ### b. Thanh ghi quản lý bộ nhớ và nguồn thực thi.
- RSP: luôn trỏ vào đỉnh hiện tại của stack ( nơi có địa chỉ bộ nhớ nhỏ nhất trong stack frame ).
- RBP: trỏ vào đý của stack frame. Dùng làm mốc để CPU tìm vị trí các biến cục bộ.
- RIP: lưu địa chỉ bộ nhớ của câu lệnh assembly tiếp theo mà CPU thực hiện.
## Memory space:  
| KERMEL SPACE |   |  
| :--- | :--- | 
| stack | -> Phình từ cao xuống thấp   
|        |  -> Chứa các biến cục bộ bên trong hàm,, các tham số dư và địa chỉ trả về |
| heap | -> phình từ thấp lên cao 
|      | -> vùng cấp phát bộ nhớ động chạy bằng các hàm như malloc(), free () |  
| .bss | -> chứa các biến toàn cục/static chưa được gán giá trị ( nghĩa là nó luôn luôn = 0 ) |  
| .data | -> chứa các biến toàn cục ( global ) hoặc biến tĩnh được gán giá trị |
| .text | -> chứa code. Vùng này có quyền đọc và thực thi |  

