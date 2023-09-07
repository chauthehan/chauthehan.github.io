---
layout: post
title: Vector database là gì?
subtitle: Tất tần tật từ A-F về vector database
cover-img: /assets/img/20230907_vectordatabase/database_cover.png
thumbnail-img: /assets/img/20230907_vectordatabase/thumb.gif
share-img: /assets/img/20230907_vectordatabase/database_cover.png
tags: [database, vectordatabase]
---

## Vector database

**Vector embedding** là một yếu tố quan trọng làm nên thành công của các mô hình ngôn ngữ lớn và đa số các mô hình AI hiện tại vì nó có thể chứa đựng các thông tin quan trọng giúp cho AI có thể hiểu được . Các vector embedding tạo ra bởi các mô hình AI thường là các vector có số lượng chiều rất lớn, tạo ra khó khăn trong việc lưu trữ và quản lý chúng.
Đó là lý do chúng ta cần một cơ sở dữ liệu được thiết kế chuyên biệt để xử lý kiểu dữ liệu này. Vector database cung cấp các giải pháp lưu trữ tối ưu và khả năng truy vấn cho các cho các embedding, đảm bảo về độ linh hoạt, khả năng mở rộng, và hiệu năng.

## Các tính năng của một vector database

1. Quản lý dữ liệu : Vector database cung cấp các phương pháp xử lý quen thuộc trong việc lưu trữ dữ liệu, ví dụ như chèn, xóa, hoặc cập nhật dữ liệu. 
2. Lưu trữ metadata và lọc: Vector database có thể lưu trữ metadata liên quan đến các vector đầu vào. Người dùng có thể truy vấn trong cơ sở dữ liệu sử dụng các thông tin từ metadata.
3. Khả năng mở rộng: Vector database được thiết kế để có thể mở rộng đáp ứng được khi dữ liệu tăng thêm, cung cấp các khả năng xử lý song song và phân tán.
4. Cập nhật theo thời gian thực: Vector database hỗ trợ việc cập nhật dữ liệu theo thời gian thực.
5. Sao lưu: Vector database xử lý việc sao lưu dữ liệu theo lịch trình thiết lập sẵn.
6. An toàn dữ liệu và quyền truy cập: Vector database cung cấp các tính năng về bảo mật dữ liệu và các cơ chế quản lý quyền truy cập.

## Vector database hoạt động như thế nào

Trong một database truyền thống, chúng ta thường truy vấn theo dòng sao cho giá trị truy vấn phải chính xác bằng với các giá trị trong database. Trong vector database, chúng ta sẽ sử dụng similarity metric để tìm một vector có độ tương đồng cao nhất với truy vấn của chúng ta.
Vector database sử dụng kết hợp các thuật toán trong Approximate Nearest Neighbor (ANN) search, điều này giúp cho việc truy vấn được thực hiện nhanh và chính xác. Vì vector database trả về kết quả **xấp xỉ**, chúng ta sẽ phải đánh đổi giữa độ chính xác và tốc độ xử lý. Kết quả càng chính xác thì tốc độ càng chậm và ngược lại. Tuy nhiên, một hệ thống tốt có thể cung cấp khả năng tìm kiếm cực nhanh và độ chính xác vẫn rất tốt.

Luồng hoạt động của một vector database như sau

![alt text](/assets/img/20230907_vectordatabase/vector_database_pipeline.png ).

1. Đánh thứ tự (Indexing): Vector database sử dụng các thuật toán như PQ, LSH, HNSW để đánh thứ tự cho các vector. Bước này chuyển một vector thành một cấu trúc dữ liệu giúp cho việc tìm kiếm nhanh hơn.
2. Truy vấn (Querying): Vector database so sánh vector truy vấn với các vector trong tập dữ liệu để tìm ra các thành phần có độ tương đồng cao nhất ( sử dụng các similarity metric)
3. Hậu xử lý: Trong một vài trường hợp, vector database lấy ra được các thành phần có độ tương đồng cao và hậu xử lý chúng để trả về kết quả cuối. Bước này có thể bao gồm việc re-ranking các thành phần tương đồng sử dụng một similarity metric khác.

Trong các bài tiếp theo, tôi sẽ nói rõ hơn về các thuật toán này và cách chúng góp phần tạo nên một vector database hoạt động hiệu quả.

## Reference
1. https://www.pinecone.io/learn/vector-database/