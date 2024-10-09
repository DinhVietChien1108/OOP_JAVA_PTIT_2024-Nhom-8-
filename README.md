# OOP_JAVA_PTIT_2024-Nhom-8-
1. Xác định yêu cầu
A. Phát biểu bài toán và các chức năng dự định phát triển
1. Bài toán
Xây dựng một ứng dụng quản lý quán cà phê nhằm tối ưu hóa quá trình quản lý, giúp chủ quán và nhân viên thực hiện công việc một cách hiệu quả và thuận lợi. Ứng dụng này sẽ cung cấp các chức năng cơ bản để quản lý các hoạt động hàng ngày của quán cà phê.
2. Các chức năng ứng dụng
Hệ thống đăng nhập
•	Cho phép nhân viên và quản lý đăng nhập vào hệ thống
Trang chủ
Danh mục
•	Quản lý danh sách danh mục trong quán cà phê
•	Cho phép thêm mới, sửa đổi, xóa, tìm kiếm các danh mục đối với quản lý, nhân viên chỉ có thể tìm kiếm.
Sản phẩm
•	Quản lý thông tin chi tiết về sản phẩm: tên sản phẩm, thuộc danh mục nào, giá và trạng thái.
•	Cho phép thêm mới, sửa đổi, xóa, tìm kiếm sản phẩm đối với quản lý, nhân viên chỉ có thể tìm kiếm.
Khu vực & Bàn ăn
•	Cho phép quản lý các khu vực và bàn ăn trong quán cà phê đối với quản lý.
•	Cho phép theo dõi trạng thái và đặt bàn đối với quản lý và nhân viên.	
Hóa đơn
•	Quản lý hóa đơn bán hàng
•	Cho phép xem, xóa, tìm kiếm và xuất hóa đơn đối với quản lý, nhân viên không có chức năng xoá.
Người dùng
•	Cho phép quản lý thông tin về người dùng đối với quản lý
2. Phân tích thiết kế ra lớp
1. Lớp User (Người dùng)
Chức năng:
Người dùng (nhân viên hoặc quản lý) có thể đăng nhập và thực hiện các chức năng dựa trên vai trò.
Thuộc tính:
id: Mã định danh người dùng.
name: Tên người dùng.
email: Email người dùng.
password: Mật khẩu đăng nhập.
role: Vai trò của người dùng (nhân viên hoặc quản lý).
Phương thức:
dangNhap(email: String, password: String): Xác thực đăng nhập.
capNhatThongTin(name: String, email: String, password: String): Cập nhật thông tin cá nhân.
2. Lớp Category (Danh mục sản phẩm)
Chức năng:
Quản lý các danh mục sản phẩm trong quán cà phê.
Thuộc tính:
id: Mã định danh danh mục.
name: Tên danh mục.
status: Trạng thái của danh mục (hiện đang sử dụng hoặc ngừng).
Phương thức:
themDanhMuc(name: String): Thêm danh mục mới.
xoaDanhMuc(id: int): Xóa danh mục.
timKiemDanhMuc(name: String): Tìm kiếm danh mục theo tên.
capNhatDanhMuc(id: int, name: String): Cập nhật danh mục.
3. Lớp Product (Sản phẩm)
Chức năng:
Quản lý các sản phẩm trong quán cà phê (thuộc một danh mục nhất định).
Thuộc tính:
id: Mã định danh sản phẩm.
category: Danh mục của sản phẩm (quan hệ với lớp Category).
name: Tên sản phẩm.
price: Giá sản phẩm.
status: Trạng thái của sản phẩm (còn bán hay đã ngừng bán).
Phương thức:
themSanPham(categoryId: int, name: String, price: Double): Thêm sản phẩm mới.
xoaSanPham(id: int): Xóa sản phẩm.
capNhatSanPham(id: int, name: String, price: Double, status: Boolean): Cập nhật thông tin sản phẩm.
timKiemSanPham(name: String): Tìm kiếm sản phẩm.
4. Lớp Table (Bàn)
Chức năng:
Quản lý khu vực và bàn trong quán cà phê.
Thuộc tính:
id: Mã định danh bàn.
area: Khu vực của bàn (quan hệ với lớp Area).
name: Tên bàn (ví dụ: Bàn số 1).
Phương thức:
themBan(areaId: int, name: String): Thêm bàn mới.
capNhatBan(id: int, name: String): Cập nhật thông tin bàn.
timKiemBan(name: String): Tìm kiếm bàn.
xoaBan(id: int): Xóa bàn.
5. Lớp Area (Khu vực)
Chức năng:
Quản lý khu vực của quán (một khu vực có thể có nhiều bàn).
Thuộc tính:
id: Mã định danh khu vực.
name: Tên khu vực.
Phương thức:
themKhuVuc(name: String): Thêm khu vực mới.
capNhatKhuVuc(id: int, name: String): Cập nhật thông tin khu vực.
xoaKhuVuc(id: int): Xóa khu vực.
6. Lớp Bill (Hóa đơn)
Chức năng:
Quản lý hóa đơn bán hàng.
Thuộc tính:
id: Mã định danh hóa đơn.
user: Nhân viên tạo hóa đơn (quan hệ với lớp User).
table: Bàn được sử dụng trong hóa đơn (quan hệ với lớp Table).
total_price: Tổng số tiền của hóa đơn.
status: Trạng thái của hóa đơn (đã thanh toán hoặc chưa thanh toán).
created_at: Thời gian tạo hóa đơn.
table_name: Tên bàn sử dụng.
Phương thức:
themHoaDon(userId: int, tableId: int, totalPrice: Double): Thêm hóa đơn mới.
xoaHoaDon(id: int): Xóa hóa đơn.
timKiemHoaDon(date: Date): Tìm kiếm hóa đơn theo ngày.
capNhatHoaDon(id: int, status: Boolean): Cập nhật trạng thái hóa đơn (đã thanh toán).
7. Lớp BillDetail (Chi tiết hóa đơn)
Chức năng:
Quản lý chi tiết các sản phẩm được mua trong một hóa đơn.
Thuộc tính:
bill: Hóa đơn liên kết (quan hệ với lớp Bill).
product: Sản phẩm liên kết (quan hệ với lớp Product).
amount: Số lượng sản phẩm đã mua.
Phương thức:
themChiTietHoaDon(billId: int, productId: int, amount: int): Thêm chi tiết hóa đơn.
xoaChiTietHoaDon(billId: int, productId: int): Xóa chi tiết hóa đơn.
Tóm tắt quan hệ giữa các lớp:
User: Người dùng (quản lý và nhân viên) có thể quản lý các hoạt động liên quan đến danh mục, sản phẩm, hóa đơn, khu vực và bàn.
Category: Liên quan đến sản phẩm thông qua quan hệ 1-nhiều.
Product: Sản phẩm thuộc về một danh mục và được thêm vào hóa đơn.
Table: Bàn thuộc về một khu vực, liên quan đến hóa đơn.
Bill: Hóa đơn được tạo bởi nhân viên, bao gồm nhiều chi tiết hóa đơn (sản phẩm).
BillDetail: Liên kết sản phẩm với hóa đơn.
3. Xây dựng biểu đồ lớp (UML)
![image](https://github.com/user-attachments/assets/f37406e2-9e82-4a49-9382-41a8973be5ed)
