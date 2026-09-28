# Bài tập String Immutability

## 1. Phân tích lỗi

Đoạn mã ban đầu có lỗi ở hai dòng:

```javascript
rawTicketCode.trim();
rawTicketCode.toUpperCase();
```

Các phương thức `trim()` và `toUpperCase()` **không thay đổi trực tiếp chuỗi gốc**. JavaScript sử dụng cơ chế String Immutability, nghĩa là chuỗi sau khi được tạo không thể thay đổi trực tiếp.

Hai câu lệnh trên chỉ tạo ra chuỗi mới nhưng kết quả không được lưu lại. Vì vậy biến `rawTicketCode` vẫn có giá trị:

```text
"   med-card-0428-ut   "
```

Khi đó:

```javascript
rawTicketCode.startsWith("MED-");
```

trả về `false` vì chuỗi vẫn còn khoảng trắng và đang viết thường.

Ngoài ra:

```javascript
rawTicketCode.slice(4, 7);
```

chỉ lấy được `car`, trong khi mã khoa cần lấy là `CARD`. Do đó cần sử dụng:

```javascript
normalizedTicketCode.slice(4, 8);
```

## 2. Cách khắc phục

Lưu kết quả của `trim()` và `toUpperCase()` vào biến mới:

```javascript
const cleanTicketCode = rawTicketCode.trim();
const normalizedTicketCode = cleanTicketCode.toUpperCase();
```

Sau đó sử dụng `normalizedTicketCode` cho các bước kiểm tra và xử lý tiếp theo.

## 3. Test Cases

| Trường hợp kiểm thử                            | Dữ liệu đầu vào            | Kết quả sai thực tế                   | Kết quả đúng mong đợi                                  |
| ---------------------------------------------- | -------------------------- | ------------------------------------- | ------------------------------------------------------ |
| TC01 - Mã hợp lệ có khoảng trắng và chữ thường | `"   med-card-0428-ut   "` | Mã phiếu hợp lệ: `false`, khoa: `car` | Mã phiếu hợp lệ: `true`, khoa: `CARD`, ưu tiên: `true` |
| TC02 - Mã hợp lệ không có khoảng trắng         | `"MED-CARD-1234-UT"`       | Nếu dùng `slice(4,7)` thì khoa: `CAR` | Mã phiếu hợp lệ: `true`, khoa: `CARD`, ưu tiên: `true` |

## 4. Nguyên lý String Immutability

Trong JavaScript, String là kiểu dữ liệu bất biến.

Các phương thức như:

```javascript
trim();
toUpperCase();
replace();
replaceAll();
```

không thay đổi chuỗi ban đầu mà trả về một chuỗi mới.

Ví dụ:

```javascript
const code = "  med-123  ";

code.trim();

console.log(code);
```

Kết quả vẫn là:

```text
"  med-123  "
```

Muốn sử dụng chuỗi đã xử lý phải lưu kết quả:

```javascript
const cleanCode = code.trim();
```

Sau đó có thể tiếp tục:

```javascript
const normalizedCode = cleanCode.toUpperCase();
```

## 5. Kết quả chương trình

Với dữ liệu:

```text
med-card-0428-ut
```

sau khi xử lý, chương trình cho kết quả:

```text
Mã phiếu hợp lệ: true
Khoa điều trị: CARD
Ưu tiên: true
Chuỗi in phiếu: MED | CARD | 0428 | UT
```
