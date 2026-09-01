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
