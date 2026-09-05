# write up task 0: Kiến thức nền
## bit/byte and endianess:
  ### a. Bit/ byte:
**bit/byte:**
-Bit: đơn vị thông tin nhỏ nhất trong máy tính, chỉ nhận 0 hoặc 1  
-Byte: 1 nhóm gồm 8 bit.  
  1 byte = 8 bit  
ví dụ: 10101010 = 1 byte  
-Nibble: 1 nhóm gồm 4 bit ( nửa byte ).  
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
- RBP: trỏ vào đáy của stack frame. Dùng làm mốc để CPU tìm vị trí các biến cục bộ.
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

## Calling convention:
- Quy luật cách các hàm truyền dữ liệu cho nhau và cách CPU dọn dẹp khi gọi hàm trên Linux x86-64.
  ### a. Truyền tham số:
- 6 tham số đầu tiên đưa trực tiếp vào thanh ghi theo thứ tự cố định:
    + Tham số 1: RDI
    + Tham số 2: RSI
    + Tham số 3: RDX
    + Tham số 4: RCX
    + Tham số 5: R8
    + Tham số 6: R9
* từ số 7 trở đi: do hết thanh ghi nên CPU đẩy vào bộ nhớ stack.
  ### b. Trả về giá trị:
- Sau Khi tính toán xong, kết quả trả về hàm luôn được cất vào thanh ghi RAX
- Hàm gọi phía sau chỉ cần đọc thanh ghi RAX là lấy được kết quả.
  ### c. Luồng chương trình chuyển dich ( call và ret )
- Lệnh call ( gọi hàm ):
  + đẩy địa chỉ của câu lệnh nằm ngay dưới lệnh call vào stack -> tên là saved rip / return address ( địa chỉ quay về ).
- Lệnh ret ( thoát hàm quay về )
  + sau khi chạy xong thì lệnh ret rút giá trị về saved rip trên đỉnh stack nạp ngược vào thanh ghi rip.
  + CPU lập tức quay trở lại chạy tiếp câu lệnh call ban đầu.
## stack: 
- Vùng nhớ tạm thời hoạt động theo cơ chế LIFO ( vào sau, ra trước ), phát triển từ địa chỉ cao đến địa chỉ thấp trong không gian bộ nhớ ảo.  
  ### a. Liên hệ của stack với thanh ghi:
- RSP: trỏ trực tiếp vào đỉnh cả stack ( nơi có địa chỉ bộ nhớ thấp nhất ).
- RBP: trỏ trực tiếp vào đáy của stack ( mốc cố định để truy cập biến cục bộ và tham số ).

[ ĐỊA CHỈ CAO ]
        +----------------------------+
        |  Saved RIP (Return Addr)   |  <- [3] Mục tiêu ghi đè của Hacker!
        +----------------------------+
        |         Saved RBP          |  <- [2] Bị ghi đè tiếp theo
RBP --->+----------------------------+
        |   buffer[16] (16 bytes)    |  <- [1] Nhập quá 16 bytes, dữ liệu tràn LÊN TRÊN
RSP --->+----------------------------+
               [ ĐỊA CHỈ THẤP ]
  ### b. Liên hệ của stack với memory layout.
- Vị trí: stack ở vùng có địa chỉ cao trong bộ nhớ
- Hhướng phình: phình từ vùng có địa chỉ cao xuống vùng có địa chỉ thấp.
- xung đột: Nếu stack phình xuống rộng chạm vào heap =>> stack overflow sẽ xảy ra ( stack overflow là khi dữ liệu ghi vào 1 mảng bị vượt quá kích thước cấp phát trên stack khiến dữ liệu tràn ra ngoài và ghi đè lên ô nhớ khác )
  ### c. Cơ chế của push và pop
- Lệnh push ( đẩy dữ liệu vào stack )
  + Push làm phình stack xuống dưới bằng 2 cách:
      1. Trừ con trỏ đỉnh stack: RSP = RSP - 8
      2. Ghi dữ liệu: chép giá trị src vào địa chỉ [ RSP ]
- Lệnh pop: dst ( rút dữ liệu ra khỏi stack )
  + pop làm thu hẹp stack lên phía trên bằng 2 cách:
      1. Đọc dữ liệu: trích xuất giá trị 8 bytes tại ô nhớ [ RSP ] gán vào thanh ghi/ ô nhớ dst.
      2. tăng con trỏ vào đỉnh stack: RSP = RSP + 8.
### Hex / binary:  
- Binary ( nhị phân ): chỉ có số 0 và 1 ( '0' là bật và '1' là tắt công tắc )
- Hex ( lục phân / cơ số 16 ): vì chuỗi 0 và 1 quá dài nên ng ta gộp 4 số nhị phân thành 1 ký tự Hex.
- Hex gồm các số từ 0 đến 9 và các chữ từ A - F. Trong code, Hex luôn có 0x ở đầu.
- 
- 
  
