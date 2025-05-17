---
title: 'Tìm hiểu giao thức'
date: '2025-19-5'
updated: '2023-19-5'
categories:
  - 'Bạch Quang Anh'
  - 'markdown'
coverImage: '/images/Giaothuc.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Check out how heading links work with this starter in this post.
---

# Bài tập: Giao thức & OPENMPI

## Bài tập 1: Tìm hiểu sự tương quan và khác biệt giữa các giao thức

### Các giao thức cần tìm hiểu:

- HTTP
- TCP/IP
- UDP
- REST
- GraphQL
- SOAP
- AJAX
- RPC
- gRPC

### Mục đích sử dụng và sự liên quan:

- **HTTP**: Giao thức truyền tải siêu văn bản giữa client và server qua Web.
- **TCP/IP**: Bộ giao thức nền tảng cho Internet, trong đó TCP đảm bảo truyền tải tin cậy.
- **UDP**: Giao thức truyền tải không đảm bảo độ tin cậy nhưng nhanh (dùng trong video stream, game online).
- **REST**: Kiến trúc thiết kế API dựa trên HTTP, đơn giản, phổ biến.
- **GraphQL**: Ngôn ngữ truy vấn API hiện đại, cho phép client yêu cầu đúng dữ liệu mình cần.
- **SOAP**: Giao thức nhắn tin dựa trên XML, dùng trong hệ thống lớn yêu cầu bảo mật, xác thực.
- **AJAX**: Kỹ thuật kết hợp JavaScript và XML để gửi/nhận dữ liệu bất đồng bộ không cần reload trang.
- **RPC (Remote Procedure Call)**: Gọi hàm từ xa giữa các hệ thống qua mạng như thể gọi cục bộ.
- **gRPC**: Phiên bản hiện đại của RPC dùng Protocol Buffers, hiệu suất cao, phổ biến trong microservices.

### Tóm tắt mối quan hệ:

- **REST, SOAP, GraphQL** là API layer chạy **trên HTTP**.
- **AJAX** thường dùng để gọi **REST API** từ trình duyệt.
- **gRPC và RPC** thường dùng trong hệ thống **microservices** hoặc **hệ thống phân tán**.
- **TCP/IP và UDP** là nền tảng cho tất cả giao thức truyền thông mạng.

---

## Bài tập 2: Nghiên cứu thư viện OPENMPI

### Mục tiêu:

Tìm hiểu tính năng của OpenMPI và đề xuất cách dùng thư viện này để giải bài toán tính số nguyên tố.

### Giới thiệu:

**OpenMPI** là một thư viện **Message Passing Interface (MPI)** mã nguồn mở, dùng trong lập trình song song, hỗ trợ truyền thông giữa các tiến trình trên nhiều máy hoặc nhiều lõi (core).

### Bài toán:

- Tính **10,000,000 số nguyên tố đầu tiên**.
- Hệ thống có **16 máy**, mỗi máy có **2 core** (tổng cộng 32 core).
- Mục tiêu là tính **đúng và nhanh nhất**, cho kết quả **ổn định dù số core thay đổi**.

### Giải pháp đề xuất:

- Áp dụng **Sàng Eratosthenes** để tìm số nguyên tố.
- Chia đoạn số thành các phần bằng nhau và phân phối cho các tiến trình.
- Mỗi tiến trình xử lý một đoạn và gửi kết quả về tiến trình gốc (root).
- Đảm bảo khi thay đổi số tiến trình (core), việc phân chia và hợp nhất dữ liệu vẫn hoạt động tốt.

### Lưu ý:

- Cần đồng bộ hóa việc khởi tạo sàng chung.
- Sử dụng `MPI_Bcast`, `MPI_Scatter`, `MPI_Gather` để phân phối và thu thập dữ liệu.

### Tài liệu tham khảo:

- [Wikipedia - Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
