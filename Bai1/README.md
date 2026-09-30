1. Mục tiêu

Sửa hai lỗi trong chương trình quản lý trạm sạc:

Sử dụng sai pop() thay vì shift() khiến xe đến sau được điều phối trước.
Sử dụng điều kiện i <= array.length khiến vòng lặp truy cập vào phần tử undefined, dẫn đến tổng sản lượng điện và doanh thu bị NaN.
2. Phân tích lỗi
Lỗi 1: Sử dụng pop() sai nghiệp vụ
Mã nguồn ban đầu:
const nextVehicle = waitingQueue.pop();
pop() lấy phần tử cuối cùng của mảng.
Với:
const waitingQueue = [
    '29A-112.33',
    '30E-889.12',
    '51K-678.99'
];
pop() sẽ lấy:
51K-678.99
Điều này sai với quy tắc FIFO vì xe 29A-112.33 đến trước phải được phục vụ trước.
Cách sửa
Sử dụng:
const nextVehicle = waitingQueue.shift();
shift() lấy và xóa phần tử đầu tiên của mảng.
Kết quả:
29A-112.33

3. Lỗi 2: Vượt quá chỉ số mảng
Mã nguồn ban đầu:
for (let i = 0; i <= completedSessionsKwh.length; i++) {
    totalKwh += completedSessionsKwh[i];
}
Mảng có:
length = 4
Các index hợp lệ là:
0, 1, 2, 3
Nhưng điều kiện:
i <= completedSessionsKwh.length
cho phép i = 4.
Khi đó:
completedSessionsKwh[4]
trả về:
undefined
Sau đó:
totalKwh += undefined;
làm cho:
totalKwh = NaN
Do đó doanh thu:
totalRevenue = totalKwh * fastChargingRate;
cũng trở thành:
NaN
Cách sửa
Sử dụng:
for (let i = 0; i < completedSessionsKwh.length; i++) {
    totalKwh += completedSessionsKwh[i];
}
Vòng lặp chỉ chạy với các index hợp lệ từ 0 đến length - 1.

4. Bảng Test Cases
Trường hợp kiểm thử	Dữ liệu đầu vào	Kết quả sai thực tế	Kết quả đúng mong đợi
1. Mảng chờ có nhiều xe và mảng điện năng có nhiều phần tử	waitingQueue = ['29A-112.33', '30E-889.12', '51K-678.99'], completedSessionsKwh = [45.2, 30.5, 62.8, 28.0]	pop() lấy 51K-678.99; vòng lặp <= length làm tổng thành NaN	shift() lấy 29A-112.33; tổng điện năng = 166.5 kWh; doanh thu = 749250 VNĐ
2. Mảng điện năng rỗng	completedSessionsKwh = []	Với vòng lặp <= length, truy cập completedSessionsKwh[0] là undefined, có thể làm tổng thành NaN	Vòng lặp không thực hiện; totalKwh = 0; totalRevenue = 0 VNĐ
3. Mảng điện năng có 1 phần tử	completedSessionsKwh = [50]	Với <= length, vòng lặp truy cập thêm index 1 là undefined, tổng thành NaN	Tổng điện năng = 50 kWh; doanh thu = 225000 VNĐ