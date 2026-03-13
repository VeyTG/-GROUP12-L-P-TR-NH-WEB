# Campus Bookstore Lab – Bug Fix Report

## 1. Introduction

Campus Bookstore là một mini project PHP mô phỏng trang quản trị nhà sách trong trường học.

Mục tiêu của bài lab:

- Chạy project trong môi trường local
- Tìm lỗi trong source code
- Phân loại lỗi thành **syntax errors** và **logic errors**
- Sửa lỗi để các trang hoạt động đúng

Thành viên
- 23521840 Vũ Thị Tường Vy
- 22520298 Lê Nguyễn Thùy Dương

---

## 2. Files Checked

Trong báo cáo này đã kiểm tra và sửa lỗi ở các file:

```
pages/checkout.php
pages/customers.php
pages/dashboard.php
```

---

# 3. Bugs Found and Fixed

## 3.1 checkout.php

### Bug 1 – Discount calculation incorrect

Buggy code:

```php
$discountValue = $subtotal * $discountPercent;
```

Problem:

Discount 10% nhưng code nhân trực tiếp với 10.

Example:

```
subtotal = 100
discount = 100 * 10 = 1000
```

Fix:

```php
$discountValue = $subtotal * ($discountPercent / 100);
```

Type:

```
Logic Error
```

---

### Bug 2 – VAT calculation incorrect

Buggy code:

```php
$vat = $subtotal * 0.1;
```

VAT phải tính sau khi trừ discount.

Fix:

```php
$vat = ($subtotal - $discountValue) * 0.1;
```

Type:

```
Logic Error
```

---

## 3.2 customers.php

### Bug 3 – Missing semicolon

Buggy code:

```php
$activeCustomers = []
```

PHP yêu cầu dấu `;` kết thúc câu lệnh.

Fix:

```php
$activeCustomers = [];
```

Type:

```
Syntax Error
```

---

## 3.3 dashboard.php

### Bug 4 – Revenue calculation incorrect

Buggy code:

```php
$totalRevenue += $item['qty'];
```

Revenue phải được tính bằng:

```
price × quantity
```

Fix:

```php
$totalRevenue += $products[$item['sku']]['price'] * $item['qty'];
```

Type:

```
Logic Error
```

---

### Bug 5 – Low stock condition incorrect

Buggy code:

```php
if ($product['stock'] > 5)
```

Điều kiện này hiển thị sản phẩm còn nhiều hàng thay vì sắp hết.

Fix:

```php
if ($product['stock'] <= 5)
```

Type:

```
Logic Error
```

---

## 3.4 settings.php

### Bug 6 – Sai cú pháp mảng cấu hình (thiếu dấu phẩy)

Buggy code:

```php
$config = [
    'currency' => 'USD'
    'timezone' => 'Asia/Ho_Chi_Minh',
    'language' => 'en',
];
```

Fix:

```php
$config = [
    'currency' => 'USD',
    'timezone' => 'Asia/Ho_Chi_Minh',
    'language' => 'en',
];
```

Type:

```
Syntax Error
```

---

### Bug 7 – Thiếu dấu đóng mảng `]` trước `;`

Buggy code:

```php
$config = [
    'currency' => 'USD',
    'timezone' => 'Asia/Ho_Chi_Minh',
    'language' => 'en';
```

Fix:

```php
$config = [
    'currency' => 'USD',
    'timezone' => 'Asia/Ho_Chi_Minh',
    'language' => 'en',
];
```

Type:

```
Syntax Error
```

---

### Bug 8 – Tiêu đề trang hiển thị sai

Buggy code:

```php
<h3>Bookstore settings</h3>
```

Fix:

```php
<h3>Store settings</h3>
```

Type:

```
Logic Error (UI)
```

---

## 3.5 reports.php

### Bug 9 – Thiếu dấu `;` sau khi khởi tạo mảng

Buggy code:

```php
$reportRows = []
$totalsByCategory = [];
```

Fix:

```php
$reportRows = [];
$totalsByCategory = [];
```

Type:

```
Syntax Error
```

---

### Bug 10 – Gom nhóm theo `name` thay vì `category`

Buggy code:

```php
foreach ($products as $product) {
    $category = $product['name'];

    if (!isset($totalsByCategory[$category])) {
        $totalsByCategory[$category] = 0;
    }

    $totalsByCategory[$category] += $product['stock'] * $product['price'];
}
```

Fix:

```php
foreach ($products as $product) {
    $category = $product['category'];

    if (!isset($totalsByCategory[$category])) {
        $totalsByCategory[$category] = 0;
    }

    $totalsByCategory[$category] += $product['stock'] * $product['price'];
}
```

Type:

```
Logic Error
```

---

### Bug 11 – Thiếu dấu ngoặc `)` trong `foreach`

Buggy code:

```php
foreach ($products as $product {
    // ...
}
```

Fix:

```php
foreach ($products as $product) {
    // ...
}
```

Type:

```
Syntax Error
```

---

## 3.6 orders.php

### Bug 12 – Điều kiện lọc trạng thái đơn hàng sai

Buggy code:

```php
$pendingOnly = [];

foreach ($orders as $order) {
    if ($order['status'] === 'completed') {
        $pendingOnly[] = $order;
    }
}
```

Fix:

```php
$pendingOnly = [];

foreach ($orders as $order) {
    if ($order['status'] !== 'completed') {
        $pendingOnly[] = $order;
    }
}
```

Type:

```
Logic Error
```

---

### Bug 13 – Sắp xếp sai thứ tự trong `usort`

Buggy code:

```php
usort($pendingOnly, function (array $left, array $right): int {
    return $left['id'] <=> $right['id'];
});
```

Fix:

```php
usort($pendingOnly, function (array $left, array $right): int {
    return $right['id'] <=> $left['id'];
});
```

Type:

```
Logic Error
```

# 4. Result

Sau khi sửa các lỗi:

- Trang **Dashboard** hiển thị đúng số liệu
- Trang **Customers** hiển thị danh sách khách hàng hoạt động
- Trang **Checkout** tính toán tổng tiền chính xác
- Trang **Settings** hiển thị đúng cấu hình cửa hàng
- Trang **Reports** thống kê doanh thu theo từng danh mục chính xác
- Trang **Orders** chỉ hiển thị đơn đang chờ xử lý và sắp xếp đơn mới nhất lên trước

Project chạy ổn định trên server local.

---
