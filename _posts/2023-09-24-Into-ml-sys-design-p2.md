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

###### Lựa chọn hàm loss
Hàm loss là phép đo độ chính xác của model. Hàm loss cho phép hàm tối ưu cập nhập tham số cho model trong suốt quá trình training để đưa loss về nhỏ nhất.
Cần phải hiểu và lựa chọn hàm loss từ các hàm loss phổ biến và thỉnh thoảng bạn sẽ cần chỉnh sửa hàm loss cho phù hợp với vấn đề.

###### Training từ đầu hay là fine-tune
Fine-tune là việc tiếp tục huấn luyện mô hình dựa trên mô hình có sẵn khác so với training mô hình từ đầu. Suy nghĩ để lựa chọn phương pháp phù hợp.

###### Distributed training
Model có thể phát triển lớn hơn theo thời gian và dữ liệu cũng sẽ càng nhiều hơn. Distributed training là phương pháp tạo ra nhiều worker để training đồng thời model để tăng tốc quá trình training. Có 2 cách làm đó là data parallelism và model parallelism

##### Evaluation

Bước tiếp theo trong quá trình phát triển mô hình đó là evaluation, sử dụng một metric nào đó để đánh giá performance của model. Có 2 phương pháp evaluation đó là offline và online.

###### Offline evaluation
Phương pháp này sẽ đánh giá model trong quá trình phát triển. Dưới đây là các metric thường được sử dụng cho các task khác nhau.

|**Task**| **Offline metrics**|
|--------|--------------------|
|Classification|Precision, recall, F1,  accuracy, ROC-AUC, confusion matrix  |
|Regression| MSE, MAE, RMSE   |
|Ranking| Precision@k, recall@k, MRR, mAP, nDCG |
|Image generation|FID, Inception score |
|NLP|BLEU, METEOR, ROUGE, CIDEr, SPICE |

###### Online evaluation
Đây là quá trình đánh giá model performance trên môi trường production. Các phép đo này có liên quan chặt chẽ đến đến mục tiêu business. Dưới đây là các metric cho các vấn đề khác nhau

|**Problem**| **Online metrics**|
|--------|--------------------|
|Ad click prediction|Click-through rate, revenue lift, etc.|
|Harmful content detection|Prevalence, valid appeals, etc.|
|Video recommendation|Click-through rate, total watch time, number of completed videos, etc.|
|Friend recommendation|Number of requests sent per day, number of requests accepted per day, etc.|
