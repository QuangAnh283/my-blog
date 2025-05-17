---
title: 'Định danh'
date: '2025-18-5'
updated: '2025-18-5'
categories:
  - 'Bạch Quang Anh'
  - 'markdown'
coverImage: '/images/Dinhdanh.jpg'
coverWidth: 16
coverHeight: 9
excerpt: Bài tập Distributed System notes 2.
---

# Bài Tập: DNS và Thuật Toán Chord

## Bài tập 1: Tìm hiểu quá trình duyệt web và các bước truy vấn DNS

### Bài làm :

Khi người dùng nhập URL `www.motvidu.com` vào trình duyệt và nhấn Enter, các bước sau sẽ xảy ra:

1. **Trình duyệt kiểm tra cache cục bộ**:

   - Trước tiên, trình duyệt sẽ kiểm tra bộ nhớ đệm (cache) để xem tên miền đã được phân giải gần đây chưa.
   - Nếu có, địa chỉ IP sẽ được lấy từ cache, bỏ qua bước truy vấn DNS.

2. **Hệ điều hành kiểm tra cache**:

   - Nếu không có trong cache của trình duyệt, hệ điều hành kiểm tra cache DNS cục bộ.

3. **Truy vấn đến máy chủ DNS (DNS Resolver)**:

   - Nếu địa chỉ IP chưa có, truy vấn được gửi đến máy chủ DNS resolver (thường là của ISP).

4. **DNS Resolver truy vấn theo cấp bậc**:

   - Resolver truy vấn đến máy chủ root DNS,
   - Sau đó đến máy chủ TLD (Top-Level Domain) như `.com`,
   - Cuối cùng đến máy chủ tên miền cụ thể (Authoritative DNS).

5. **Nhận địa chỉ IP từ máy chủ đích**:

   - Địa chỉ IP của `www.motvidu.com` được trả về cho trình duyệt.
   - Trình duyệt sử dụng địa chỉ IP này để gửi HTTP request đến máy chủ web.

6. **DNS caching**:
   - Các kết quả truy vấn sẽ được lưu vào cache để giảm thời gian phân giải trong tương lai.

### Tài liệu tham khảo:

- [Cloudflare - How DNS Works](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [Wikipedia - Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System)
- [Google DNS Overview](https://developers.google.com/speed/public-dns/docs/intro)
- [DigitalOcean DNS Guide](https://www.digitalocean.com/community/tutorials/an-introduction-to-dns-terminology-components-and-concepts)
- [MDN - What happens when you type a URL](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_happens_when_you_type_a_URL_into_a_browser)

## Bài tập 2: Viết thuật toán Chord

### Trình bày thuật toán:

**Thuật toán Chord** là một giao thức phân phối khóa (distributed hash table - DHT) để định tuyến và lưu trữ dữ liệu trong mạng ngang hàng (P2P).

#### 1. Mô tả ngắn gọn:

- Dữ liệu và node được gán một giá trị hash (sử dụng SHA-1 hoặc tương đương).
- Mỗi node có trách nhiệm lưu trữ các khóa dữ liệu nằm gần nhất sau hash của nó (trên vòng tròn định tuyến).
- Việc tìm kiếm khóa được thực hiện hiệu quả bằng cách sử dụng **finger table** để giảm số bước tìm kiếm xuống còn `O(log N)`.

#### 2. Ví dụ:

- Có 8 node với các ID từ 0 đến 7 (sử dụng mod 8).
- Dữ liệu có khóa 5 sẽ được lưu ở node có ID >= 5 gần nhất (nếu không có thì quay vòng về 0).

#### 3. Test case:

```python
# Giả lập các node và định tuyến
nodes = [0, 1, 3, 5]
key = 4  # Dữ liệu cần lưu
responsible_node = min([n for n in nodes if n >= key] or [min(nodes)])
print(f"Node chịu trách nhiệm lưu key {key} là node {responsible_node}")
```
