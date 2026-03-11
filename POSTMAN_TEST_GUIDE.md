# Hướng dẫn Test API User và Role bằng Postman

## I. Chuẩn bị

1. Tải Postman từ [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
2. Đảm bảo MongoDB đang chạy trên `mongodb://localhost:27017`
3. Chạy server: `npm start`
4. Server sẽ chạy trên `http://localhost:3000`

---

## II. Test Danh sách các Endpoint

### **PHẦN 1: QUẢN LÝ ROLE**

#### 1.1 - Tạo Role (CREATE)
- **Phương thức**: POST
- **URL**: `http://localhost:3000/api/v1/roles`
- **Headers**: 
  - Content-Type: application/json
- **Body (raw JSON)**:
```json
{
  "name": "Admin",
  "description": "Administrator role with full permissions"
}
```
- **Kết quả mong đợi**: Status 201, trả về role object với id

#### 1.2 - Lấy tất cả Role (READ - GET ALL)
- **Phương thức**: GET
- **URL**: `http://localhost:3000/api/v1/roles`
- **Kết quả mong đợi**: Status 200, danh sách tất cả roles

#### 1.3 - Lấy Role theo ID (READ - GET BY ID)
- **Phương thức**: GET
- **URL**: `http://localhost:3000/api/v1/roles/{role_id}`
  - Ví dụ: `http://localhost:3000/api/v1/roles/507f1f77bcf86cd799439011`
- **Kết quả mong đợi**: Status 200, thông tin role

#### 1.4 - Cập nhật Role (UPDATE)
- **Phương thức**: PUT
- **URL**: `http://localhost:3000/api/v1/roles/{role_id}`
- **Body (raw JSON)**:
```json
{
  "name": "User",
  "description": "Regular user role"
}
```
- **Kết quả mong đợi**: Status 200, role đã cập nhật

#### 1.5 - Xoá Role (DELETE - Soft Delete)
- **Phương thức**: DELETE
- **URL**: `http://localhost:3000/api/v1/roles/{role_id}`
- **Kết quả mong đợi**: Status 200, role có `isDeleted: true`

---

### **PHẦN 2: QUẢN LÝ USER**

#### 2.1 - Tạo User (CREATE)
- **Phương thức**: POST
- **URL**: `http://localhost:3000/api/v1/users`
- **Headers**: 
  - Content-Type: application/json
- **Body (raw JSON)**:
```json
{
  "username": "john_doe",
  "password": "password123",
  "email": "john@example.com",
  "fullName": "John Doe",
  "avatarUrl": "https://i.sstatic.net/l60Hf.png",
  "role": "{role_id}"
}
```
> **Lưu ý**: Thay `{role_id}` bằng ID của role được tạo ở bước 1.1

- **Kết quả mong đợi**: Status 201, user object với id

#### 2.2 - Lấy tất cả User (READ - GET ALL)
- **Phương thức**: GET
- **URL**: `http://localhost:3000/api/v1/users`
- **Kết quả mong đợi**: Status 200, danh sách tất cả users với thông tin role

#### 2.3 - Lấy User theo ID (READ - GET BY ID)
- **Phương thức**: GET
- **URL**: `http://localhost:3000/api/v1/users/{user_id}`
- **Kết quả mong đợi**: Status 200, thông tin user

#### 2.4 - Cập nhật User (UPDATE)
- **Phương thức**: PUT
- **URL**: `http://localhost:3000/api/v1/users/{user_id}`
- **Body (raw JSON)**:
```json
{
  "fullName": "John Doe Updated",
  "avatarUrl": "https://new-avatar-url.com/avatar.jpg"
}
```
- **Kết quả mong đợi**: Status 200, user đã cập nhật

#### 2.5 - Xoá User (DELETE - Soft Delete)
- **Phương thức**: DELETE
- **URL**: `http://localhost:3000/api/v1/users/{user_id}`
- **Kết quả mong đợi**: Status 200, user có `isDeleted: true`

---

### **PHẦN 3: ENABLE / DISABLE USER**

#### 3.1 - Enable User (Kích hoạt User)
- **Phương thức**: POST
- **URL**: `http://localhost:3000/api/v1/users/enable`
- **Headers**: 
  - Content-Type: application/json
- **Body (raw JSON)**:
```json
{
  "email": "john@example.com",
  "username": "john_doe"
}
```
- **Kết quả mong đợi**: Status 200, user có `status: true`

#### 3.2 - Disable User (Vô hiệu hóa User)
- **Phương thức**: POST
- **URL**: `http://localhost:3000/api/v1/users/disable`
- **Body (raw JSON)**:
```json
{
  "email": "john@example.com",
  "username": "john_doe"
}
```
- **Kết quả mong đợi**: Status 200, user có `status: false`

---

### **PHẦN 4: LẤY USER THEO ROLE**

#### 4.1 - Lấy tất cả User có Role cụ thể
- **Phương thức**: GET
- **URL**: `http://localhost:3000/api/v1/roles/{role_id}/users`
- **Kết quả mong đợi**: Status 200, danh sách users với role_id được chỉ định

---

## III. Thứ tự Test Khuyến nghị

1. **Tạo Role** (1.1)
   - Lưu lại `role_id` từ response
   
2. **Tạo User** (2.1)
   - Sử dụng `role_id` từ bước 1
   - Lưu lại `user_id` từ response
   
3. **Lấy tất cả Role** (1.2)
   
4. **Lấy Role theo ID** (1.3)
   
5. **Lấy tất cả User** (2.2)
   
6. **Lấy User theo ID** (2.3)
   
7. **Enable User** (3.1)
   - Kiểm tra `status` thay đổi thành `true`
   
8. **Disable User** (3.2)
   - Kiểm tra `status` thay đổi thành `false`
   
9. **Lấy User theo Role** (4.1)
   - Kiểm tra danh sách users có role được chỉ định
   
10. **Cập nhật User** (2.4)
   
11. **Cập nhật Role** (1.4)
   
12. **Xoá User** (2.5)
   
13. **Xoá Role** (1.5)

---

## IV. Ghi chú quan trọng

- **Soft Delete**: Khi xoá, dữ liệu không bị xóa khỏi database mà được đánh dấu `isDeleted: true`
- **Populate**: Khi lấy user, role sẽ được tự động populate (hiển thị đầy đủ thông tin role)
- **Validation**: Username và Email phải là unique (duy nhất)
- **Status mặc định**: Khi tạo user mới, `status` mặc định là `false` (user chưa kích hoạt)
- **loginCount**: Mặc định là 0, tối thiểu là 0

---

## V. Response Format

### Success Response
```json
{
  "message": "Thông báo thành công",
  "data": {
    "...": "dữ liệu"
  }
}
```

### Error Response
```json
{
  "message": "Thông báo lỗi",
  "error": "Chi tiết lỗi"
}
```

---

## VI. Mẹo sử dụng Postman

1. **Lưu request**: Click "Save" để lưu request, giúp tái sử dụng
2. **Sử dụng biến**: Lưu `role_id` và `user_id` vào biến Postman để tái sử dụng
   - Click vào biểu tượng {{ }} trên thanh URL
   - Khai báo biến và sử dụng như: `{{role_id}}`
3. **Pre-request Script**: Có thể viết script để tự động lưu ID từ response

---

Chúc bạn test thành công! 🚀
