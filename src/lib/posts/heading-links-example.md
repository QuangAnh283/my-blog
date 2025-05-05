---
title: "Kiến thức hệ thông phân tán"
date: "2025-5-5"
updated: "2023-5-5"
categories:
  - "Bạch Quang Anh"
  - "markdown"
coverImage: "/images/HePhanTan.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Check out how heading links work with this starter in this post.
---

## 1. Kiến trúc Client-Server

**Mô hình phổ biến nhất:**

- **Client** gửi yêu cầu (request) tới **Server**.
- **Server** xử lý yêu cầu và trả lại phản hồi (response).

**Ví dụ:**

- Trình duyệt web (client) gửi yêu cầu tới máy chủ web (server) như Google, Facebook.

---

## 2. Kiến trúc Peer-to-Peer (P2P)

**Mô hình phi tập trung:**

- Mỗi node (nút) vừa là client, vừa là server.
- Các nút giao tiếp trực tiếp với nhau mà không cần máy chủ trung tâm.

**Ví dụ:**

- Mạng BitTorrent, Blockchain (Bitcoin, Ethereum).

---

## 3. Kiến trúc Multi-tier (n-tier)

**Mô hình phân lớp rõ ràng:**

- Thường có 3 tầng: Presentation (giao diện), Logic (xử lý nghiệp vụ), Data (cơ sở dữ liệu).
- Giúp dễ dàng bảo trì, mở rộng hệ thống.

**Ví dụ:**

- Một hệ thống thương mại điện tử:
  - Frontend (React/Angular)
  - Backend API (Node.js, Java)
  - Database (PostgreSQL, MongoDB)

---

## 4. Kiến trúc Microservices

**Xu hướng mới hiện nay:**

- Ứng dụng được chia thành nhiều dịch vụ nhỏ, mỗi dịch vụ đảm nhiệm một chức năng riêng biệt.
- Các dịch vụ giao tiếp với nhau qua API (REST, gRPC).

**Ưu điểm:**

- Dễ mở rộng theo từng thành phần.
- Triển khai độc lập.

**Ví dụ:**

- Netflix, Uber, Amazon đều sử dụng kiến trúc Microservices.

---

## 5. Kiến trúc Serverless

**Mô hình siêu nhẹ hiện đại:**

- Lập trình viên chỉ cần viết hàm chức năng (Function), nhà cung cấp Cloud tự động vận hành server.
- Giảm chi phí vận hành hạ tầng.

**Ví dụ:**

- AWS Lambda, Azure Functions, Google Cloud Functions.

---

# 🧠 Kết luận

Các mô hình kiến trúc phân tán ngày càng phong phú, từ mô hình truyền thống Client-Server đến Microservices và Serverless hiện đại.  
Lựa chọn mô hình phù hợp sẽ giúp hệ thống đạt hiệu năng cao, dễ mở rộng và bền vững trong dài hạn.

---

> 📚 Tham khảo:
>
> - Slide bài giảng Ứng dụng phân tán
> - AWS Architecture Center
> - Google Cloud Architecture Framework


