---
title: 'Đề xuất dự án giữa kì'
date: '2025-11-5'
updated: '2023-11-5'
categories:
  - 'Bạch Quang Anh'
  - 'markdown'
coverImage: '/images/de_xuat.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Check out how heading links work with this starter in this post.
---

# Tổng quan về Sonic và Kế hoạch

## 1. Mục đích, Giải pháp, Điểm mạnh, Điểm yếu, So sánh và Ứng dụng của Sonic

**Mục đích**:  
Sonic là một backend tìm kiếm nhẹ, không cần schema, thay thế đơn giản cho các hệ thống phức tạp như Elasticsearch, lưu trữ cặp văn bản-ID để tìm kiếm nhanh trong micro giây.

**Giải quyết vấn đề**:

- Tìm kiếm nhanh, nhẹ: Xử lý truy vấn trong micro giây, dùng ~30MB RAM, CPU thấp.
- Chỉ mục ID: Lưu ID để tra cứu dữ liệu từ cơ sở dữ liệu ngoài, giảm dung lượng.
- Hỗ trợ ngôn ngữ tự nhiên: Chuẩn hóa truy vấn, sửa lỗi chính tả, gợi ý từ.
- Đa ngôn ngữ: Hỗ trợ 80+ ngôn ngữ, loại bỏ stop word(như "the", "a", "an").
- Tích hợp đơn giản: Giao thức Sonic Channel dễ dùng qua các thư viện.

**Điểm mạnh**:

- Hiệu suất cao: Truy vấn micro giây, ~30MB RAM, CPU thấp.
- Nhẹ: Chỉ số nhỏ (20MB KV + 1.4MB FST cho 1 triệu tin).
- Đa ngôn ngữ: Xử lý tốt 80+ ngôn ngữ, hỗ trợ Unicode.
- Đơn giản: Không cần schema, giao thức dễ dùng.
- Mở rộng: Hỗ trợ nhiều bucket/collection.
- Dễ tích hợp: Có thư viện cho Node.js (Bởi tác giả), Rust, Python, PHP.

**Điểm yếu**:

- Giới hạn dữ liệu: IID 32-bit, ~4.2 tỷ đối tượng/bucket.
- Xử lý ngôn ngữ: Chỉ ở cấp từ, không dự đoán câu.
- Gợi ý từ: Chậm do chờ chu kỳ xây dựng FST.
- Không có API HTTP: Chỉ dùng Sonic Channel, khó với ngôn ngữ không hỗ trợ.
- Phần cứng: Chậm trên HDD, tốt nhất trên SSD.

**So sánh với các framework khác**:

| Framework         | Đặc điểm chung                    | Điểm mạnh Sonic                                              | Chi tiết khác                                                              |
| ----------------- | --------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------- |
| **Elasticsearch** | Hỗ trợ tìm kiếm ngôn ngữ tự nhiên | **Sonic nhẹ, chỉ lưu ID, phù hợp ứng dụng tài nguyên thấp.** | Elasticsearch mạnh hơn, tìm kiếm full text, nhưng nặng (GB RAM), phức tạp. |
| **Meilisearch**   | Đều nhẹ, dễ dùng                  | **Sonic chỉ lưu ID, nhẹ hơn nhưng cần DB ngoài.**            | Meilisearch lưu toàn bộ tài liệu.                                          |
| **Redis Search**  | Đều nhanh, nhẹ                    | **Sonic độc lập, tốt cho tìm kiếm văn bản đa ngôn ngữ.**     | Redis Search phụ thuộc Redis.                                              |

### **Đánh giá Sonic theo Tiêu chí Hệ thống Phân tán**

#### **Tiêu chí bắt buộc:**

1.  **Fault Tolerance (Khả năng chịu lỗi):**

    - **Hạn chế** Sonic được thiết kế để chạy như một tiến trình đơn lẻ. Nếu tiến trình Sonic đó bị lỗi hoặc máy chủ gặp sự cố, dịch vụ tìm kiếm sẽ ngừng hoạt động cho đến khi nó được khởi động lại. Sonic không có cơ chế tự động chuyển đổi dự phòng (failover) sang một nút Sonic khác trong một cụm (cluster).

2.  **Distributed Communication (Giao tiếp phân tán):**

    - **Một phần đáp ứng (ở mức client-server, không phải inter-node).** Sonic Channel là một giao thức dựa trên TCP để client (ứng dụng Express.js) giao tiếp với một server Sonic. Nó có thể hoạt động trên nhiều máy (ví dụ: ứng dụng Express.js trên một máy, Sonic server trên máy khác).
    - **Tuy nhiên, Sonic không được thiết kế để các nút Sonic giao tiếp với nhau** tạo thành một cụm phân tán (ví dụ: một nút Sonic này nói chuyện với một nút Sonic khác để chia sẻ dữ liệu hoặc phối hợp). Mỗi instance Sonic là độc lập.

3.  **Sharding (Phân mảnh) hoặc Replication (Sao chép):**

    - **Hạn chế:** Sonic không có cơ chế sharding hay replication dữ liệu tích hợp sẵn giữa nhiều nút Sonic. Dữ liệu của mỗi instance Sonic được lưu trữ cục bộ trong RocksDB (cho KV store) và các file FST.
    - **Sharding:** Đang nghiên cứu thêm.
    - **Replication:** Đang nghiên cứu thêm.

4.  **Simple Monitoring Logging (Giám sát và log đơn giản):**

    - **Đáp ứng.** Sonic có hệ thống logging (`log_level` trong `config.cfg`). Lệnh `INFO` trong `control` mode của Sonic Channel cung cấp một số thông tin thống kê cơ bản về thời gian hoạt động, số client kết nối, tổng số lệnh, độ trễ lệnh, số lượng KV/FST store đang mở. Điều này đáp ứng yêu cầu về giám sát đơn giản. (Nhưng chỉ hiện ở sonic instance, cần điều hướng log sang server gốc để hiện trên admin board)

5.  **Basic Stress Test (Kiểm tra tải cơ bản):** Đang nghiên cứu thêm

**Tiêu chí tùy chọn (Chọn 1–2):**

1.  **System Recovery (Rejoin after Failure - Nút tham gia lại hệ thống sau lỗi):**

    - **( Single instance )** Nếu một instance Sonic bị lỗi và được khởi động lại, nó sẽ khôi phục trạng thái từ dữ liệu đã lưu trên đĩa (RocksDB và FST).

2.  **Load Balancing (Cân bằng tải):**

    - **Hạn chế:** Sonic không có load balancer tích hợp.
    - Nếu chạy nhiều instance Sonic, sẽ cần một load balancer bên ngoài như `Nginx` để phân phối yêu cầu từ client đến các instance Sonic.

3.  **Consistency Guarantees (Đảm bảo tính nhất quán giữa các nút):**

    - **Hạn chế:** Vì Sonic không phải là một hệ thống phân tán có nhiều nút đồng bộ dữ liệu với nhau, nên khái niệm nhất quán dữ liệu giữa các nút Sonic không được Sonic xử lý. Nếu tự xây dựng replication, sẽ phải tự giải quyết bài toán nhất quán.
    - Mỗi instance Sonic đảm bảo tính nhất quán cho dữ liệu của chính nó (ví dụ: RocksDB có WAL).

4.  **Leader Election (Bầu chọn Leader):**

    - **Hạn chế:** Sonic không hoạt động theo mô hình leader-follower hay yêu cầu cơ chế bầu chọn leader. Nó là một dịch vụ độc lập.

5.  **Security Features (Tính năng bảo mật cho giao tiếp giữa các nút):**

    - **Sonic Channel có xác thực bằng mật khẩu (`auth_password`)** cho kết nối client-server.
    - Nếu xây dựng một hệ thống nhiều nút Sonic, sẽ cần phải thêm 1 application layer để xử lý bảo mật cho giao tiếp giữa các node Sonic.

6.  **Deployment Automation (Tự động triển khai):**
    - Sonic có thể được tự động hóa triển khai bằng Docker, Docker Compose, Kubernetes qua `Dockerfile` và `docker-compose.yml`

#### **Tổng kết về Sonic cho các tiêu chí hệ thống phân tán:**

Sonic, như được thiết kế và thể hiện trong mã nguồn, là một **search backend đơn lẻ, hiệu năng cao, và nhẹ**. Nó **không phải** là một hệ thống phân tán "out-of-the-box" có sẵn các tính năng như fault tolerance tự động, sharding/replication dữ liệu tích hợp, hay giao tiếp inter-node phức tạp.

- **Sonic đáp ứng tốt:** Logging/Monitoring đơn giản, có thể tự động hóa triển khai.
- **Sonic không đáp ứng hoặc chỉ đáp ứng một phần (cần xây dựng thêm bên ngoài):** Fault Tolerance, Distributed Communication (ở mức inter-node), Sharding/Replication, System Recovery (trong cụm), Load Balancing, Consistency (giữa các nút), Leader Election.

## 2. Kế hoạch Dự kiến Bài Giữa Kỳ

**Bài toán**: Xây dựng một website Ecommerce nơi người dùng có thể xem sản phẩm và sử dụng chức năng tìm kiếm nhanh chóng giữa hàng triệu sản phẩm. Hệ thống cần được thiết kế với các yếu tố của một hệ thống phân tán để đảm bảo tính sẵn sàng và khả năng mở rộng cơ bản.

**Mục tiêu chính**:

1. Sonic làm backend tìm kiếm cho sản phẩm.
2. Tích hợp Sonic với MongoDB (nguồn dữ liệu chính cho sản phẩm).
3. Đáp ứng các tiêu chí hệ thống phân tán bắt buộc và 1-2 tiêu chí tùy chọn.
4. Xây dựng một trang admin đơn giản để giám sát trạng thái cơ bản của hệ thống.
5. Xây dựng một ứng dụng frontend đơn giản để người dùng có thể tìm kiếm sản phẩm.
