---
layout: post
title: Giới thiệu về thiết kế một hệ thống machine learning
subtitle: Thiết kế hệ thống machine learning
cover-img: /assets/img/20230907_vectordatabase/database_cover.png
thumbnail-img: /assets/img/20230907_vectordatabase/thumb.gif
share-img: /assets/img/20230907_vectordatabase/database_cover.png
tags: [database, vectordatabase, filtering]
---

## Giới thiệu
Nhiều người mới làm về ML nghĩ rằng các mô hình ML hay neural network là toàn bộ một hệ thống ML. Tuy nhiên trong thực tế, một hệ thống ML liên quan đến nhiều thức phức tạp, bao gồm các thành phần như quản lý data, hệ thống máy chủ, evaluation pipeline để đánh giá hệ thống, monitoring để đảm bảo rằng độ chính xác của hệ thống ML không bị giảm theo thời gian.

![alt text](/assets/img/2023199_Intro-ml-sys-design/ml-component.webp)

Để xây dựng một hệ thống ML cần trả lời một loại câu hỏi đã được thiết kế sẵn. Dưới đây là các thành phần mà tôi đề xuất và sẽ được dùng xuyên suốt loạt bài viết này:
1. Làm rõ yêu cầu
2. Đặt vấn đề thành một bài toán ML
3. Chuẩn bị dữ liệu
4. Phát triển mô hình ML
5. Đánh giá mô hình
6. Triển khai
7. Monitoring

#### Làm rõ yêu cầu

Chúng ta cần làm rõ các yêu cầu khi thiết kế một hệ thống ML. Dưới đây là danh sách các câu hỏi cần được làm rõ:
- **Mục tiêu kinh doanh**: Nếu chúng ta được yêu cầu thiết kế một hệ thống đề xuất các nhà nghỉ du lịch, thì chúng ta thường sẽ có 2 mục tiêu là tăng số lượng booking và tăng doanh thu
- **Các tính năng của hệ thống**: Hệ thống cần có những chức năng gì mà có thể ảnh hưởng đến thiết kế của hệ thống ML. Ví dụ, với một hệ thống đề xuất video, chúng ta có thể cần biết răng người dùng có thể ấn like hoặc dislike các video được đề xuất ko, vì các tương tác này có thể được dùng để gán nhãn cho dữ liệu huấn luyện
- **Dữ liệu**: Nguồn dữ liệu là gì? Lớn đến mức nào và đã được gán nhãn chưa?
- **Constraints**: Năng lực tính toán như thế nào? Đây có phải là hệ thống trên cloud hay hệ thống có thể hoạt động trên thiết bị? Mô hình có phải cần cải thiện tự động theo thời gian không? 
- **Scale**: chúng ta có bao nhiêu user? Có bao nhiêu đối tượng, ví dụ như video, mà chúng ta sẽ phải làm việc? Mức độ tăng trưởng của các số liệu này như thế nào?
- **Hiệu năng**: một dự đoán cần thực hiện trong tối đa bao lâu? Có cần phải là giải pháp theo thời gian thực không? ưu tiên độ chính xác hay là độ trễ?

Danh sách này chưa phải hoàn hảo nhưng có thể dùng nó để làm một điểm bắt đầu. Các chủ đề khác như an toàn dữ liệu cũng nên được cân nhắc. 


#### Đặt vấn đề thành một bài toán ML

 Trong thực tế, ban đầu chúng ta cần quyết định rằng phương pháp ML có cần để giải quyết vấn đề không. Đối với một bài toán ML, chúng ta có thể coi vấn đề thành một bài toán ML theo các bước:
 - Định nghĩa mục tiêu của bài toán
 - Xác định đầu vào, đầu ra của hệ thống
 - Xác định thuộc dạng bài toán ML nào

###### Định nghĩa mục tiêu bài toán
Mục tiêu của kinh doanh có thể là tăng doanh số thêm 20%, nhưng mà chúng ta không thể huấn luyện một mô hình bằng cách nói với nó rằng "tăng doanh số thêm 20%" :) . Để giải quyết bài toán, chúng ta cần chuyển mục tiêu kinh doanh sang bài toán ML một cách rõ ràng. Ví dụ:

| Application             | Business objective| ML objective|
| ----------------------- | -------------- |----------------------
| Ứng dụng bán vé sự kiện | tăng doanh số bán vé| Tối đa số lượng đăng ký dự sự kiện
| Ứng dụng video streaming| tăng tương tác người dùng| Tối đa thời gian xem của người dùng
| Hệ thống dự đoán click vào quảng cáo| Tăng số lượng click| Tối đa tỉ lệ click-through
|Phát hiện nội dung độc hại trong một nền tảng media| Cải thiện tính an toàn của nền tảng | Dự đoán nội dung có phải là độc hại|
|Đề xuất kết bạn| Tăng tỉ lệ người dùng gia tăng network của họ| Tối đa số lượng kết bạn|

###### Xác định đầu vào, đầu ra của hệ thống

Khi ta đã xác định được ML objective, chúng ta cần định nghĩa input và output của hệ thống. Ví dụ, một hệ thống phát hiện nội dung độc hại trên một nền tảng mạng xã hội, đầu vào là một post, đầu ra là post đó có phải là độc hại không.

Trong một vài trường hợp, hệ thống có thể bao gồm nhiều mô hình ML. Khi đó, chúng ta cần xác định đầu vào và đầu ra cho mỗi mô hình. Ví dụ trong hệ thống phát hiện nội dung độc hại, chúng ta có thể sử dụng một mô hình để phát hiện bạo lực và một mô hình khác để phát hiện ảnh 18+. 

Một cân nhắc quan trọng nữa đó là có thể có nhiều cách để xác định đầu vào và đầu ra cho mô hình. Ví dụ:

![alt text](/assets/img/2023199_Intro-ml-sys-design/model_input_output.webp)

###### Xác định thuộc dạng bài toán ML nào

Các dạng bài toán ML quen thuộc như hình dưới đây:

![alt text](/assets/img/2023199_Intro-ml-sys-design/ML_category.webp)

#### Dữ liệu


