## 12-factor
### Giới thiệu
Trong th ờibuổi hiện đại ngày nay, phần mềm thường được cung cấp như một dịch vụ: được gọi là ứng dụng web hoặc là "Phần mềm như một dịch vụ".
Ứng dụng theo mười hai yếu tố là phương pháp để xây dựng các ứng dụng phần mềm như một dịch vụ.

- Sử dụng các định dạng khai báo cho tự động hóa cài đặt, để giảm thiểu thời gian và chi phí cho các nhà phát triển mới tham gia dự án;
- Có các quy ước rõ ràng với hệ điều hành cơ bản (nền tảng), cung cấp tối đa tính di động giữa các môi trường thực thi.
- Thích hợp cho việc triển khai trên nền tảng đám mây hiện đại, giảm thiểu yêu cầu cho các máy chủ và quản trị hệ thống.
- Giảm thiểu tối đa sự khác biệt giữa giai đoạn phát triển và thực thi, tạo điều kiện cho việc phát triển liên tục một cách linh hoạt nhất.
- Và có thể mở rộng quy mô mà không có thay đổi đáng kể về công cụ, kiến trúc hoặc thực tiễn phát triển.

Phương pháp 12 yếu tố có thể được áp dụng cho các ứng dụng được viết bằng bất kỳ ngôn ngữ lập trình nào và sử dụng bất kỳ kết hợp dịch vụ sao lưu nào (cơ sở dữ liệu, hàng đợi, bộ nhớ cache, v.v.).

### Cơ sở
Những người đóng góp cho tài liệu này đã trực tiếp tham gia vào việc phát triển và triển khai hàng trăm ứng dụng,
và gián tiếp chứng kiến sự phát triển, vận hành và mở rộng hàng trăm nghìn ứng dụng thông qua công việc của chúng tôi trên nền tảng Heroku.

Tài liệu này tổng hợp tất cả các kinh nghiệm và quan sát của chúng tôi trên rất nhiều ứng dụng dạng "phần mềm như một dịch vụ". Nó bao gồm tam giác 3 yếu tố quan trọng mà bất kỳ quy trình phát triển ứng dụng l
ý tưởng nào cũng cần có, bao gồm việc tập trung vào sự phát triển linh động của một ứng dụng theo thời gian, tính linh động trong việc cộng tác giữa các nhà phát triển trên nền code của ứng dụng, và tránh các chi phí phát sinh tạo ra bởi sự hao mòn của phần mềm.

Động lực của chúng tôi là nâng cao nhận thức về một số vấn đề hệ thống mà chúng tôi đã thấy trong phát triển ứng dụng hiện đại, để cung cấp vốn từ vựng chung để thảo luận những vấn đề đó và cung cấp một loạt giải pháp khái niệm rộng cho những vấn đề kèm theo thuật ngữ.
Định dạng của tài liệu này được lấy cảm hứng từ cuốn sách của Martin Fowler, "Patterns of Enterprise Application Architecture and Refactoring".

### Ai nên đọc tài liệu này?
Bất kỳ ứng dụng xây dựng nhà phát triển nào hoạt động như một dịch vụ. Ops kỹ sư triển khai hoặc quản lý các ứng dụng như vậy.

#### 1. Codebase
###### Một codebase được theo dõi trong việc kiểm soát các thay đổi, nhiều triển khai.
Một ứng dụng tuân theo mười hai chuẩn luôn được theo dõi trong một hệ thống kiểm soát phiên bản, chẳng hạn như Git, Mercurial hoặc Subversion.
Một bản sao chéo của cơ sở dữ liệu theo dõi thay đổi được biết như là một code repository, thường được viết tắt là code repo hay chỉ là repo mà thôi. 

Một codebase có thể là bất cứ repo đơn lẻ nào (trong một hệ thống kiểm soát sửa đổi tập trung như Subverrsion), hoặc có thể lả một tập các repo chia 
sẻ chung một commit gốc(trong một hệ thống kiểm soát sửa đổi phân cấp như Git). 

Luôn luôn có sự tương quan một-một giữa codebase và ứng dụng:
- Nếu có nhiều codebases, nó không phải là một ứng dụng - đó là một hệ thống phân tán. Mỗi thành phần trong một hệ thống phân tán là một ứng dụng và mỗi thành phần có thể tuân thủ riêng với mười hai chuẩn.
- Nhiều ứng dụng chia sẻ cùng một code là vi phạm mười hai chuẩn. Giải pháp ở đây là quản lý các đoạn code được dùng chung thành các thư viện mà ta có thể sử dụng thông qua dependency manager.

Chỉ có một codebase cho mỗi ứng dụng, nhưng sẽ có nhiều triển khai ứng dụng.
Một triển khai sẽ chạy phiên bản của ứng dụng. Đây thường là một trang web sản xuất và một hoặc nhiều trang web dàn dựng. 
Thêm vào đó, mỗi người phát triển  đều có một bản sao của ứng dụng đang chạy trên môi trường developer cục bộ của họ, mỗi số chúng cũng được coi như một triển khai.

Các codebase là như nhau trên tất cả các triển khai, mặc dù các phiên bản khác nhau có thể hoạt động trong mỗi triển khai.
Ví dụ, một nhà phát triển có vài commit chưa triển khai đến staging, staging có vài commit chưa được triển khai tới production. Nhưng chúng đều dùng chung nền codebase, điều này làm chúng được phân biệt là các triển khai khác nhau của cùng một ứng dụng.

#### 2. Các phụ thuộc
###### Khai báo rõ ràng và tách biệt các phụ thuộc
Hầu hết các ngôn ngữ lập trình cho phép một hệ thống packaging phân phối các thư viện hỗ trợ, như là CPAN cho Perl hoặc Rubygems cho Ruby. Các thư viện được cài đặt thông qua hệ thống packaging có thể được cài đặt mức hệ thống ( được hiểu như là “các site package”) hoặc chỉ trọng phạm vi thư mục chứa ứng dụng ( được biết như là “vendoring” hoặc “bundling”).

Một ứng dụng theo 12 chuẩn không bao giờ phụ thuộc vào các tồn tại ngầm của các gọi mức hệ thống. Nó khai báo tất cả các phụ thuộc, hoàn thiện và chính xác, thống qua một biểu thị khai báo phụ thuộc. Hơn nữa, nó sử dụng một công cụ tách biệt phụ thuộc trong quá trình thực thi để đảm bảo rằng không có phụ thuộc ngầm nào “lọt vào”  từ các hệ thống xung quanh. Đặc tả phụ thuộc đầy đủ và rõ ràng được áp dụng thống nhất cho cả productiont và developer.

Ví dụ, Bundler cho Ruby cho phép định dạng biểu thị `Gemfile` cho việc khai báo phụ thuộc và `bundle exec` cho việc tách biệt phụ thuộc. Trong Python có 2 công cụ riêng biệt cho các bước này - Pip được sử dụng để khai báo và Virtualenv để tách biệt. Thâm chí C có Autoconf cho việc khai báo phụ thuộc và link tĩnh có thể cung cấp tách biệt các phụ thuộc. Bất kể dùng tập công cụ nào, khai báo và tách biệt phụ thuộc luôn phải được sử dụng cùng nhau - chỉ một là không đủ để thỏa mãn 12 chuẩn.

Một lợi ích của việc khai báo phụ thuộc rõ ràng là nó đơn giản hóa việc cài đặt cho những người phát triển mới của ứng dụng. Người phát triển mới có thể lấy codebase của ứng dụng về máy phát triển của họ, với điều kiện tiên quyết chỉ yêu cầu ngôn ngữ chạy và quản lý phụ thuộc được cài đặt. Họ sẽ có thể cài đặt bất cứ thứ gì cần thiết để chạy code của ứng dụng với một lệnh xây dựng xác định. Ví dụ, lấy xây dựng cho Ruby/Bundler là `bundle install`, còn với Clojureojure/Leiningen là `lein deps`.

Các ứng dụng theo 12 chuẩn cũng không phụ thuộc vào các tồn tại ngầm của bất kỳ công cụ hệ thống nào. Ví dụ bao gồm cả với ImageMagick hay curl. Trong khi nhưng công cụ này có thể tồn tại trên nhiều hay thậm chí là hầu hết các hệ thống, không có gì bảo đảm rằng chúng sẽ tồn tại trên tất cả các hệ thống mà ứng dụng có thể chạy trong tương lại, hay một phiên bản được tìm thấy trong hệ thống tương lai có thể tương thích với ứng dụng. Nếu ứng dụng cần một công cụ hệ thống, công cụ đó cần được gán vào ứng dụng.

#### III. Cấu hình
###### Lưu cấu hình trong môi trường
Cấu hình của một ứng dụng là bất cứ thứ gì giống như là sự thay đổi giữa các triển khai (môi trường staging, production, developer, vân vân). Nó bao gồm:

- Nguồn xử lý cơ sở dữ liệu, Memcaches, và các dịch vụ hỗ trợ khác.
- Thông tin đăng nhập cho các dịch vụ bên ngoài như Amazon S3 hoặc Twitter
- Các giá trị của mỗi-triển-khai như là tên máy chủ của triển khai

Các ứng dụng đôi khi lưu trữ cấu hình như các hằng số trong codeode. Điều này xung đột với 12 chuẩn, khi nó yêu cầu `tính tách biệt 1 cách nghiêm ngặt trong cấu hình ở coe`. Cấu hình thay đổi  giữa các triển khai, còn code thì không.

Một kiểm thử litmus kiểm tra ứng dụng có tất cả cấu hình chính xác với code là khi codebae có thể tạo mã nguồn mở bất cứ khi nào, mà không ảnh hưởng bất cứ thông tin xác thực nào.

Lưu ý rằng định nghĩa `cấu hình` không bao gồm các cấu hình nội bộ ứng dụng, ví dụ như là `config/routes.rb` trong Rails, hoặc cách các module được kết nối trong Spring. Loại cấu hình này không thay đổi giữa các triển khai, và ví thể hoàn thiện nhất trong code.

Một cách tiếp cận khác với cấu hình là sử dụng các file cấu hình không được kiểm soát trong kiểm soát thay đổi, như là `config/database.yml` trong Rails. Đây là 1 cải thiện lớn so với việc sử dụng các hằng số được kiểm soát trong code repo, nhưng vẫn còn những nhược điểm: nó rất dễ kiểm soát lỗi một file cấu hình trong repo, có một xu hướng là các file cấu hình sẽ nằm rải rác ở những chỗ khác nhau và trong những định dạng khác nhau, khiên nó khó để thấy và kiểm soát tất cả các cấu hình trong 1 chỗ. Hơn nữa những định dạng này có xu hướng riêng biệt với từng ngôn ngữ hay framework.

Ứng dụng theo 12 chuẩn lưu trữ cấu trình trong các biến môi trường ( thường đọc tắt là env vars hoặc env).Các biến ENV thường dễ thay đổi giữa các triển khai mà không phải thay đổi code, không giống như các file cấu hình, chỉ 1 chút thay đổi của chúng được kiểm soát phụ thuocj bởi code repo; và không giống các file cấu hình thông thường,  các cơ chế cấu hình khác như Java System Properties, chúng là các chuẩn ngôn ngữ và OS-agnostic.

Một khía cạnh khác của quản lý cấu hình là gom nhóm. Đôi khi các ứng dụng gom cấu hình thành các tên nhóm ( thường gọi là “các môi trường”) được đặt tên sau các triển khai cụ thể, như là môi trường `development`, `test`, và `production` trong Rails. Phương pháp này không được gọn cho lắm: vì sẽ có nhiều triển khai của ứng dụng được tạo ra, các tên môi trường mới sẽ cần thiết, như là `staging` hoặc `qa`. Vì dự án sẽ càng phát triển hơn, các người phát triển có thể sẽ thêm các môi trường đặc biệt riêng của họ như `joes-staging`, kết quả của việc kết hợp đống cấu hình này sẽ khiến việc quản lý triển khải của ứng dụng trở nên mong manh.

Trong các ứng dụng 12 chuẩn, các biến env là các điều khiển chi tiết, mỗi chúng trực giao đầy đủ với các env vars khác. Chúng không vào giờ được nhóm lại với nhau như làà "các môi trường", nhưng thay vào đó chúng quản lý độc lập cho từng triển khai. Đây là một mô hình nâng cao sự mượt mà, ứng dụng sự mở rộng một cách tự nhiên đến nhiều triển khai hơn trong suốt vòng đời của nó.


