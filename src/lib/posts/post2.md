---
title: 'Tiến trình và luồng'
date: '2025-10-5'
updated: '2025-10-5'
categories:
  - 'Bạch Quang Anh'
  - 'markdown'
coverImage: '/images/tien-trinh-va-luong-1.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Bài tập Distributed System notes 2.
---

## 1. Kiểm tra hiệu năng máy tính

**Thiết bị:** Dell Latitude E5570

- **CPU:** Intel Core i5-6300HQ (4 cores, 4 threads)
- **GPU:** Intel HD Graphics 530 (onboard)
- **RAM:** 8GB DDR4

**Đánh giá hiệu năng:**  
Máy tính có thể xử lý tốt các tác vụ cơ bản như lướt web, lập trình, xử lý văn bản, học tập và chạy ứng dụng nhẹ. Với CPU 4 nhân và RAM 8GB, máy có thể chạy đa luồng mức vừa phải, tuy nhiên sẽ gặp khó khăn khi xử lý các tác vụ nặng như huấn luyện AI, xử lý đồ họa 3D, hoặc chạy các phần mềm yêu cầu tài nguyên cao.

---

## 2. 12 bài toán CNTT phổ biến sử dụng đa luồng / đa tiến trình

Dưới đây là danh sách 12 bài toán hoặc ứng dụng phổ biến trong ngành Công nghệ thông tin có sử dụng đa luồng hoặc đa tiến trình:

1. **Trình duyệt web**: Mỗi tab thường là một tiến trình riêng biệt, trong khi các hoạt động trong tab (render, JavaScript) sử dụng đa luồng.
2. **Trò chơi (game)**: Game hiện đại sử dụng nhiều luồng cho AI, vật lý, render và âm thanh để hoạt động mượt mà.
3. **Hệ điều hành**: Chạy hàng trăm tiến trình và mỗi tiến trình có thể chứa nhiều luồng xử lý.
4. **Biên dịch mã nguồn**: Trình biên dịch như GCC hoặc Clang hỗ trợ biên dịch song song bằng cách dùng nhiều tiến trình.
5. **Máy chủ web (Web server)**: Apache hoặc Nginx có thể xử lý nhiều kết nối bằng cách tạo nhiều tiến trình hoặc luồng.
6. **Xử lý ảnh hàng loạt**: Khi xử lý hàng trăm bức ảnh, sử dụng đa luồng giúp rút ngắn thời gian xử lý.
7. **Ứng dụng chat / nhắn tin**: Mỗi cuộc trò chuyện hoặc kết nối có thể xử lý bằng một luồng riêng.
8. **Phân tích dữ liệu lớn (Big Data)**: Dùng các framework như Hadoop, Spark – dựa trên hệ thống phân tán, mỗi nút có nhiều tiến trình/luồng.
9. **Cơ sở dữ liệu (Database)**: Hệ quản trị như MySQL hoặc PostgreSQL sử dụng đa tiến trình để phục vụ truy vấn đồng thời.
10. **Chương trình nén/giải nén tệp tin**: Sử dụng nhiều luồng để nén nhanh hơn các phần khác nhau của tệp tin.
11. **IDE (Visual Studio Code)**: Dùng nhiều luồng để biên dịch, gợi ý mã, linting… mà không làm đơ giao diện.
12. **AI Training**: Mô hình như ChatGPT được huấn luyện bằng cách phân chia mô hình và dữ liệu ra hàng ngàn GPU chạy song song – sử dụng cả đa luồng và hệ thống phân tán.

## 3. Khi nào dùng Thread, khi nào dùng Process?

![ảnh chép tay](/images/THREADandProcess.jpg)

### ✅ Khi nào nên dùng **Thread**

- Khi cần xử lý song song các tác vụ nhẹ trong cùng một ứng dụng.
- Khi các tác vụ cần chia sẻ dữ liệu hoặc bộ nhớ với nhau — vì các thread chia sẻ không gian bộ nhớ.
- Khi muốn giảm thiểu tài nguyên hệ thống, do tạo thread nhẹ hơn tạo process.
- Khi yêu cầu phản hồi nhanh, như trong giao diện người dùng hoặc server xử lý nhiều kết nối một lúc.

Ví dụ:

- Một ứng dụng giao diện người dùng (UI) có một thread chính cho giao diện, và các thread phụ để xử lý tác vụ nền.
- Một web server dùng thread để xử lý nhiều request từ người dùng cùng lúc.

---

### ✅ Khi nào nên dùng **Process**

- Khi cần cách ly hoàn toàn giữa các tác vụ để đảm bảo tính ổn định (nếu một process bị lỗi, các process khác vẫn chạy).
- Khi các tác vụ thực thi nặng, không cần chia sẻ dữ liệu.
- Khi muốn tận dụng đa nhân CPU để chạy các tiến trình riêng biệt.
- Khi ứng dụng có khả năng mở rộng quy mô (scale) tốt hơn bằng cách chạy nhiều process độc lập.

Ví dụ:

- Mỗi tab trong trình duyệt như Chrome là một process riêng để tránh treo toàn bộ trình duyệt khi một tab bị lỗi.
- Chạy song song các tiến trình xử lý video hoặc phân tích dữ liệu.

---

### ✅ Khi nào nên kết hợp **Process và Thread**

- Khi ứng dụng phức tạp, vừa cần cách ly các thành phần lớn (dùng process), vừa cần xử lý song song nhẹ bên trong (dùng thread).
- Khi mỗi phần của hệ thống có thể hoạt động độc lập, nhưng bên trong phần đó lại cần các tác vụ đa luồng để tăng hiệu năng.

Ví dụ:

- Trình duyệt web: mỗi tab là một process, bên trong có nhiều thread để vẽ giao diện, xử lý JavaScript, âm thanh,...
- IDE như Visual Studio Code: mỗi phần như extension, gợi ý code,... là process, bên trong có nhiều thread cho từng thao tác cụ thể.
- Hệ thống xử lý AI hoặc học máy: mỗi mô hình là một process, các thread xử lý dữ liệu song song bên trong để tận dụng GPU/CPU.

---

Tóm lại, chọn dùng **thread**, **process** hay kết hợp cả hai phụ thuộc vào:

- Mức độ độc lập giữa các tác vụ
- Yêu cầu chia sẻ dữ liệu
- Hiệu năng mong muốn
- Khả năng cách ly lỗi trong hệ thống

## 4. Report: Huấn luyện ChatGPT bằng hệ thống phân tán

ChatGPT là một mô hình ngôn ngữ lớn (LLM) với hàng trăm tỷ tham số. Để huấn luyện được mô hình như vậy, người ta phải sử dụng hệ thống máy tính **phân tán** rất lớn:

- **Data Parallelism**: mỗi GPU xử lý một phần của tập dữ liệu.
- **Model Parallelism**: chia mô hình thành các phần nhỏ để huấn luyện song song.
- **Pipeline Parallelism**: tổ chức luồng dữ liệu theo từng giai đoạn xử lý.
- **GPU clusters**: hàng ngàn GPU kết nối bằng mạng tốc độ cao như NVLink, InfiniBand.
- **Lưu trữ phân tán**: SSD tốc độ cao và cơ chế phân phối dữ liệu hiệu quả.

**Tài liệu tham khảo:**

- https://openai.com/research
- https://arxiv.org/
- https://deepmind.com/blog

---
