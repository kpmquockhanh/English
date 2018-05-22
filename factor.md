## 12-factor
### Giới thiệu
Trong th ờibuổi hiện đại ngày nay, phần mềm thường được cung cấp như một dịch vụ: được gọi là ứng dụng web hoặc là "Phần mềm như một dịch vụ".
Ứng dụng theo mười hai yếu tố là phương pháp để xây dựng các ứng dụng phần mềm như một dịch vụ.

- Sử dụng các định dạng khai báo cho tự động hóa cài đặt, để giảm thiểu thời gian và chi phí cho các nhà phát triển mới tham gia dự án;
- Có các quy ước rõ ràng với hệ điều hành bên dưới, cung cấp khả năng di chuyển tối đa giữa các môi trường thực thi.
- Thích hợp cho việc triển khai trên nền tảng đám mây hiện đại, giảm thiểu yêu cầu cho các máy chủ và quản trị hệ thống.
- Giảm thiểu tối đa sự khác biệt giữa giai đoạn phát triển và thực thi, tạo điều kiện cho việc phát triển liên tục một cách nhanh nhất.
- Và có thể mở rộng quy mô mà không có thay đổi đáng kể về công cụ, kiến ​​trúc hoặc thực tiễn phát triển.

Phương pháp 12 yếu tố có thể được áp dụng cho các ứng dụng được viết bằng bất kỳ ngôn ngữ lập trình nào và sử dụng bất kỳ kết hợp dịch vụ sao lưu nào (cơ sở dữ liệu, hàng đợi, bộ nhớ cache, v.v.).

### Cơ sở
Những người đóng góp cho tài liệu này đã trực tiếp tham gia vào việc phát triển và triển khai hàng trăm ứng dụng,
và gián tiếp chứng kiến ​​sự phát triển, vận hành và mở rộng hàng trăm nghìn ứng dụng thông qua công việc của chúng tôi trên nền tảng Heroku.

Tài liệu này tổng hợp tất cả các kinh nghiệm và quan sát của chúng tôi trên rất nhiều ứng dụng dạng "phần mềm như một dịch vụ". Nó bao gồm tam giác 3 yếu tố quan trọng mà bất kỳ quy trình phát triển ứng dụng l
ý tưởng nào cũng cần có, bao gồm việc tập trung vào sự phát triển linh động của một ứng dụng theo thời gian, tính linh động trong việc cộng tác giữa các nhà phát triển trên nền code của ứng dụng, và tránh các chi phí phát sinh tạo ra bởi sự hao mòn của phần mềm.

Động lực của chúng tôi là nâng cao nhận thức về một số vấn đề hệ thống mà chúng tôi đã thấy trong phát triển ứng dụng hiện đại, để cung cấp vốn từ vựng chung để thảo luận những vấn đề đó và cung cấp một loạt giải pháp khái niệm rộng cho những vấn đề kèm theo thuật ngữ.
Định dạng của tài liệu này được lấy cảm hứng từ cuốn sách của Martin Fowler, "Patterns of Enterprise Application Architecture and Refactoring".

### Ai nên đọc tài liệu này?
Bất kỳ ứng dụng xây dựng nhà phát triển nào hoạt động như một dịch vụ. Ops kỹ sư triển khai hoặc quản lý các ứng dụng như vậy.

#### 1. Codebase
###### Một codebase được theo dõi trong việc kiểm soát các thay đổi, nhiều triển khai.

 