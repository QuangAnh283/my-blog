---
title: 'Trao đổi thông tin'
date: '2025-10-5'
updated: '2025-10-5'
categories:
  - 'Bạch Quang Anh'
  - 'markdown'
coverImage: '/images/traodoithongtin.jpg'
coverWidth: 16
coverHeight: 9
excerpt: bài tập DS_lec 3.
---

## Bài tập 1: Tìm hiểu cơ chế, chức năng và cài đặt dịch vụ truyền thông thông điệp

### Dịch vụ được chọn: RabbitMQ

**Cơ chế hoạt động:**
RabbitMQ là một hệ thống hàng đợi thông điệp sử dụng giao thức AMQP. Nó cho phép các thành phần trong hệ thống phân tán giao tiếp với nhau thông qua việc gửi và nhận thông điệp không đồng bộ.

**Chức năng chính:**

- Gửi và nhận thông điệp (message)
- Xử lý hàng đợi (queue)
- Quản lý phân phối thông điệp tới các consumer
- Đảm bảo thông điệp không bị mất (durable queues)
- Hỗ trợ cơ chế publish/subscribe (pub/sub)

**Cài đặt RabbitMQ:**

1. **Cài đặt Docker** (nếu chưa có):

   ```bash
   sudo apt install docker.io
   ```

2. **Chạy RabbitMQ bằng Docker**:

   ```bash
   docker run -d --hostname my-rabbit --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
   ```

3. **Truy cập giao diện quản trị** tại:  
   `http://localhost:15672`  
   (user: `guest`, pass: `guest`)

---

## Bài tập 2: Code hệ thống đơn giản sử dụng RabbitMQ

**Mô tả hệ thống:**

- **Producer:** gửi thông điệp "Xin chào từ Producer"
- **Consumer:** nhận và in thông điệp ra màn hình

**Ví dụ bằng Python sử dụng thư viện `pika`:**

**Producer:**

```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')
channel.basic_publish(exchange='', routing_key='hello', body='Xin chào từ Producer!')

print("Đã gửi thông điệp.")
connection.close()
```

**Consumer:**

```python
import pika

def callback(ch, method, properties, body):
    print(f"Đã nhận: {body.decode()}")

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()
channel.queue_declare(queue='hello')

channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

print('Đang chờ thông điệp. Nhấn CTRL+C để thoát.')
channel.start_consuming()
```

---

## Bài tập 3: RPC và JSON

### Thư viện đã biết: `xmlrpc.client`

### Tìm hiểu thêm: **gRPC** (Google Remote Procedure Call)

**Mô tả:**
gRPC là một framework RPC hiệu năng cao của Google. Nó sử dụng giao thức HTTP/2 và định dạng dữ liệu là **Protocol Buffers (protobuf)**. Tuy nhiên, ta có thể tích hợp để hỗ trợ định dạng **JSON**.

**Ví dụ sử dụng gRPC với JSON trong Python:**

1. Khởi tạo server dùng gRPC
2. Sử dụng thư viện `google.protobuf.json_format` để chuyển đổi giữa JSON và Protobuf

**Cài đặt:**

```bash
pip install grpcio grpcio-tools
```

**Tài liệu tham khảo:**

- [https://grpc.io/docs/](https://grpc.io/docs/)
- [https://github.com/grpc/grpc](https://github.com/grpc/grpc)
