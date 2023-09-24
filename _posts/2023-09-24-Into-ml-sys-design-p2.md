---
layout: post
title: Giới thiệu về thiết kế một hệ thống machine learning (P2)
subtitle: Thiết kế hệ thống machine learning 
cover-img: /assets/img/2023199_Intro-ml-sys-design/information-system-scaled.webp
thumbnail-img: /assets/img/2023199_Intro-ml-sys-design/thumbb.webp
share-img: /assets/img/2023199_Intro-ml-sys-design/information-system-scaled.webp
tags: [machine learning system, system, system design]
---

### Model Development

Quá trình phát triển một mô hình đi từ việc lựa chọn một mô hình ML phù hợp cho đến huấn luyện để giải quyết vấn đề.

##### Model selection

Lựa chọn thuật toán ML và kiến trúc tốt nhất cho bài toán thường sẽ có các bước sau
- **Thiết lập một baseline đơn giản** Ví dụ với bài toán hệ thống đề xuất video, baseline có thể là đề xuất những video nổi tiếng nhất.
- **Thử nghiệm với các mô hình đơn giản** Sau khi đã có được baseline, chúng ta nên thử nghiệm các mô hình ML có thể cho ra kết quả train nhanh chóng như logistic regression.
- **Thử nghiệm với các mô hình phức tạp hơn** nếu các mô hình đơn giản không thể đưa ra kết quả tốt, chúng ta có thể sử dụng đến các mô hình phức tạp hơn như mạng neuron.
- **Sử dụng ensemble models** để cải thiện chất lượng của dự đoán có thể cân nhắc đến dùng ensemble models với các phương pháp bagging, boosting, stacking.

Có nhiều lựa chọn mô hình có thể sử dụng và cần biết được ưu nhược điểm của chúng. Một vài mô hình điển hình như Logistic regression, Linear regression, Decision trees, Gradient boosted decision trees and random forests, Support vector machines, Naive Bayes, Factorization Machines (FM), Neural networks (chi tiết về ưu nhược điểm sẽ có ở các bài tiếp theo).
Khi lựa chọn một mô hình ML cần xem xét đến nhiều khía cạch của mô hình. Ví dụ:
-  Số lượng dữ liệu mà mô hình cần để huấn luyện
- Tốc độ huấn luyện
- Bộ siêu tham số để train và tune.
- Khả năng tính toán của phần cứng hiện tại (mô hình có cần GPU không,..)
- Khả năng diễn giải kết quả. Một mô hình phức tạp có thể cho ra kết quả tốt nhưng kết quả đó khó để diễn giải vì sao có được.

Không có một mô hình nào có thể giải quyết mọi vấn đề. Chúng ta cần có hiểu biết về các mô hình ML khác nhau cũng như ưu nhược điểm của chúng để có thể chọn ra mô hình phù hợp. Các phần sau này sẽ bao gồm nhiều ví dụ về việc lựa chọn mô hình.

##### Model training
Trong quá trình training mô hình sẽ có nhiều thứ cần được xét tới:
- Tạo dataset
- Lựa chọn hàm loss
- Training từ đầu hay là fine-tuning
- Distributed training

###### Tạo dataset
Có 5 bước cho quá trình tạo data.
![alt text](/assets/img/20230924_Intro_ml_sys_design_p2/data.webp)
