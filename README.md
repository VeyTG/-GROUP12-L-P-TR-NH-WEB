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

# 4. Result

Sau khi sửa các lỗi:

- Trang **Dashboard** hiển thị đúng số liệu
- Trang **Customers** hiển thị danh sách khách hàng hoạt động
- Trang **Checkout** tính toán tổng tiền chính xác

Project chạy ổn định trên server local.

---
