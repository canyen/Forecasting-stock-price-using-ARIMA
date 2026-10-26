1. <a name="_toc222824536"></a><a name="_toc223084305"></a>**Kết quả mô hình ARIMA**
1. <a name="_toc222824537"></a><a name="_toc223084306"></a>**Kiểm định tính dừng (ADF test)**

Trước khi tiến hành xây dựng và đánh giá các mô hình dự báo, toàn bộ tập dữ liệu gồm 2.196 quan sát giá đóng cửa cổ phiếu BID được chia thành tập huấn luyện và tập kiểm tra theo tỷ lệ 80/20, tương ứng với 1.757 quan sát dùng để huấn luyện mô hình và 439 quan sát dùng để đánh giá hiệu suất dự báo ngoài mẫu. Việc phân chia được thực hiện theo thứ tự thời gian, đảm bảo dữ liệu tập kiểm tra luôn nằm sau tập huấn luyện về mặt thời gian, tránh hiện tượng rò rỉ thông tin tương lai vào quá trình huấn luyện. Tất cả các bước xử lý, nhận dạng tham số và ước lượng mô hình đều được thực hiện chỉ trên tập huấn luyện; tập kiểm tra được giữ nguyên và chỉ sử dụng duy nhất ở bước đánh giá cuối cùng nhằm đảm bảo tính khách quan của kết quả.

Trước khi áp dụng mô hình ARIMA, cần kiểm tra xem chuỗi thời gian có** tính dừng hay không. Một chuỗi được xem là dừng khi** giá trị trung bình, phương sai và cấu trúc tự tương quan không thay đổi theo thời gian**.** 

<img width="945" height="484" alt="image" src="https://github.com/user-attachments/assets/cd8fc6d2-2b1c-4939-9e82-4740b7595f85" />

Quan sát biểu đồ có thể thấy dữ liệu giá đóng cửa của BID là một chuỗi thời gian có xu thế tăng theo thời gian và có yếu tố bất thường và không có tính dừng. Không thể ứng dụng mô hình ARIMA trực tiếp cho chuỗi thời gian này. Để kiểm tra điều này, ta sử dụng kiểm định Augmented Dickey-Fuller (ADF).Nếu giá trị p-value lớn hơn mức ý nghĩa 0,05, thì chuỗi được xem là không dừng và cần thực hiện sai phân để đưa chuỗi về trạng thái dừng trước khi xây dựng mô hình ARIMA. Kết quả kiểm định ADF thu được các thông số:

|<a name="_hlk221894654"></a>**Thông số**|**Giá trị**|
| - | - |
|<a name="_hlk221891954"></a>**Giá trị thống kê kiểm định (Test Statistic )**|-1.451127|
|<a name="_hlk221892331"></a>**Giá trị tới hạn 1%** |-3.432735|
|**Giá trị tới hạn 5%** |-2.862594|
|**Giá trị tới hạn 10%** |-2.567331|
|**P-value** |`  `0.557535|

<a name="_toc223045650"></a>*Bảng 3. *2* Kết quả kiểm định tính dừng của chuỗi giá cổ phiếu bằng ADF*

Với giả thuyết thống kê:

- H0: Chuỗi sai phân có nghiệm đơn vị , chuỗi không dừng.
- H1: Chuỗi sai phân không có nghiệm đơn vị, chuỗi dừng.

Kết quả kiểm định cho thấy giá trị thống kê kiểm định là - 1.451127. Xét về giá trị tuyệt đối, thống kê này lớn hơn rất nhiều so với giá trị tuyệt đối của các ngưỡng tới hạn tại mức ý nghĩa 1% (-3.432735), 5% (-2.862594) và 10% (-2.567331). Đặc biệt, giá trị P-value =  0.557535 lớn hơn mức ý nghĩa 0.05 cho thấy rằng ta chưa đủ cơ sở dữ liệu để bác bỏ giả thuyết H0. Điều này khẳng định rằng chuỗi dữ liệu giá đóng cửa của cổ phiếu BID ở dạng nguyên bản là chuỗi không dừng. 

Do kết quả kiểm định ADF cho thấy chuỗi dữ liệu giá đóng cửa gốc không dừng, chưa đủ điều kiện để đưa trực tiếp vào mô hình ARIMA mà cần phải thực hiện lấy sai phân để chuyển về trạng thái dừng. Nghiên cứu tiến hành xử lý dữ liệu bằng phương pháp lấy sai phân bậc 1. Kỹ thuật này chuyển đổi chuỗi dữ liệu bằng cách tính hiệu số giữa giá trị quan sát hiện tại và giá trị quan sát ngay trước đó. Mục đích của bước này là loại bỏ các yếu tố xu thế và mùa vụ tiềm ẩn, giúp ổn định trung bình và phương sai của chuỗi dữ liệu. Chuỗi số liệu mới đại diện cho mức thay đổi giá hàng ngày của cổ phiếu BID và được kỳ vọng sẽ thỏa mãn tính dừng để phục vụ cho việc xây dựng mô hình ARIMA. Sau khi lấy sai phân bậc 1, thu được giá trị các thông số:

|**Thông số**|**Giá trị**|
| - | - |
|**Giá trị thống kê kiểm định (Test Statistic )**|-52.759109|
|**Giá trị tới hạn 1%** |-3.432736|
|**Giá trị tới hạn 5%** |-2.862594|
|**Giá trị tới hạn 10%** |-2.567331|
|**P-value** |0\.0000|

<a name="_toc222996655"></a><a name="_toc223045651"></a>*Bảng 3. *3* Kết quả kiểm định tính dừng sau khi sai phân*

<img width="945" height="504" alt="image" src="https://github.com/user-attachments/assets/22ee02cf-062d-4ebb-8e15-57057b46fe6a" />

Kết quả sau khi lấy sai phân bậc 1,  cho thấy giá trị thống kê kiểm định đạt mức -52.759109 . Xét về trị tuyệt đối, giá trị này lớn hơn rất nhiều so với các giá trị tới hạn ở mọi mức ý nghĩa thống kê. Đồng thời, giá trị xác suất P-value = 0.0000, nhỏ hơn hoàn toàn so với mức ý nghĩa 0.05.

Từ đó , ta có đủ bằng chứng thống kê vững chắc để bác bỏ giả thuyết H0. Điều này khẳng định chuỗi sai phân bậc 1 của giá cổ phiếu BID là chuỗi dừng. Các đặc tính thống kê như trung bình và phương sai của chuỗi sai phân đã ổn định theo thời gian. Do đó, nghiên cứu xác định tham số tích hợp cho mô hình ARIMA là d = 1 tương ứng với việc lấy sai phân 1 lần.

1. <a name="_toc222824538"></a><a name="_toc223084307"></a>**Nhận dạng mô hình (ACF, PACF).**

Sau khi chuỗi đã đạt được tính dừng, bước tiếp theo là xác định các tham số p (bậc tự hồi quy) và q (bậc trung bình trượt) cho mô hình ARIMA. Tham số d  đã được xác định là 1 từ bước trước. Để lựa chọn các giá trị phù hợp cho p và q, ta sử dụng đồ thị ACF (Autocorrelation Function) và PACF (Partial Autocorrelation Function).

Dựa vào việc phân tích hai đồ thị này, có thể đề xuất một số mô hình ARIMA ứng viên trước khi lựa chọn mô hình tối ưu thông qua các tiêu chí đánh giá như AIC, BIC hoặc kiểm định phần dư.

Để xác định bậc của thành phần Trung bình trượt, nghiên cứu xem xét biểu đồ hàm tự tương quan (ACF) của chuỗi sai phân giá đóng cửa.

<img width="945" height="484" alt="image" src="https://github.com/user-attachments/assets/03da2f69-3957-4495-a31c-439d323c0e3d" />

<a name="_toc223043501"></a>*Hình 3. *4* Biểu đồ ACF của chuỗi sai phân*

Kết quả hiển thị trên biểu đồ cho thấy hầu hết các hệ số tự tương quan ở các độ trễ đầu tiên (Lag 1, 2, 3...) đều có giá trị rất nhỏ và nằm lọt hoàn toàn trong khoảng tin cậy 95%. Tại các độ trễ cao hơn như lag 4, lag 29, có xuất hiện một vài thanh vượt nhẹ ra khỏi biên, tuy nhiên mức độ vượt là không đáng kể và mang tính ngẫu nhiên nhiều hơn là quy luật. Việc các hệ số ACF tại các độ trễ thấp không có ý nghĩa thống kê cho thấy chuỗi sai phân lợi suất của cổ phiếu BID mang tính chất của một quá trình "Nhiễu trắng" . Giá trị của ngày hôm nay không có sự tương quan tuyến tính rõ rệt với các sai số của những ngày trước đó. Điều này cho thấy không có một thành phần MA mạnh mẽ nào với bậc q lớn cần được đưa vào mô hình, xác định được p=0.

Bên cạnh biểu đồ ACF, nghiên cứu tiếp tục xem xét biểu đồ PACF của chuỗi sai phân để xác định bậc của thành phần tự hồi quy AR.

<img width="945" height="491" alt="image" src="https://github.com/user-attachments/assets/32f5940c-ed64-4f3e-8244-9fffa26745d6" />

<a name="_toc223043502"></a>*Hình 3. *5* Biểu đồ PACF của chuỗi sai phân*

Quan sát biểu đồ PACF cho thấy các thanh biểu diễn hệ số tự tương quan riêng phần tại các độ trễ Lag 1, Lag 2, Lag 3... đều có biên độ dao động nhỏ. Tất cả các hệ số này đều nằm trọn trong vùng giới hạn tin cậy 95%. Không có bất kỳ điểm nào vượt ra khỏi ngưỡng ý nghĩa thống kê một cách rõ rệt. Việc không có hệ số PACF nào có ý nghĩa thống kê tại các độ trễ đầu tiên chỉ ra rằng không tồn tại mối quan hệ tự hồi quy tuyến tính đáng kể giữa giá trị hiện tại và các giá trị trong quá khứ sau khi đã loại bỏ ảnh hưởng của các độ trễ trung gian. Ta nhận thấy không có một thành phần AR mạnh mẽ nào với bậc p lớn cần được đưa vào mô hình,từ đó xác định được tham số q=0.

1. <a name="_toc222824539"></a><a name="_toc223084308"></a>**Ước lượng tham số và kiểm định phần dư**

Sau khi nhận dạng được mô hình, tiếp tục tiến hành ước lượng các tham số của mô hình ARIMA. Mặc dù phân tích ACF–PACF cung cấp gợi ý ban đầu về bậc của các thành phần tự hồi quy và trung bình trượt, đây chỉ là bước định hướng sơ bộ trong quy trình [Box–Jenkins](https://www.geeksforgeeks.org/machine-learning/box-jenkins-methodology-for-arima-models/). 

Do đó, Để lựa chọn bộ tham số tối ưu (p, d, q), nghiên cứu thực hiện tìm kiếm toàn diện (grid search) trên không gian tham số với p, q ∈ {0, 1, 2, ..., 7} và d = 1 (bậc sai phân được xác định từ kiểm định ADF ở bước trước) . Tổng cộng 64 cấu hình ARIMA được ước lượng bằng phương pháp hợp lý cực đại có điều kiện , trong đó mỗi mô hình được đánh giá đồng thời theo hai tiêu chí: tiêu chí thông tin AIC và BIC nhằm cân bằng giữa độ khớp và độ phức tạp của mô hình, và kiểm định Ljung-Box tại bậc trễ 10 nhằm đảm bảo phần dư không còn tự tương quan có ý nghĩa thống kê (p-value > 0,05). Chỉ các cấu hình thỏa mãn đồng thời cả hai điều kiện mới được đưa vào danh sách ứng viên cuối cùng, từ đó mô hình có BIC thấp nhất được chọn làm mô hình tối ưu.

Kết quả ước lượng cho thấy mô hình ARIMA(0,1,0) đạt giá trị AIC và BIC (lần lượt là 5299.833 và 5305.527) thấp nhất trong số các mô hình được xem xét, đồng thời đạt Ljung-Box p = 0.585 tại bậc trễ 10, cho thấy phần dư không còn tự tương quan có ý nghĩa thống kê. Vì vậy, mô hình ARIMA(0,1,0) được lựa chọn làm mô hình chính thức để phục vụ cho bước kiểm định phần dư và dự báo trong các phần tiếp theo của nghiên cứu. Điều này phù hợp với lý thuyết thị trường hiệu quả dạng yếu, cho rằng biến động giá cổ phiếu là ngẫu nhiên và khó dự báo dựa trên dữ liệu quá khứ.

Mô hình được ước lượng bằng phương pháp cực đại hóa hàm hợp lý (MLE), sử dụng thuật toán BHHH. Quá trình tối ưu hóa hội tụ nhanh, cho thấy cấu trúc mô hình đơn giản và ổn định.

<img width="852" height="444" alt="image" src="https://github.com/user-attachments/assets/b6e0994e-5310-41e5-8e7e-d930f55f64b5" />

<a name="_toc223043503"></a>*Hình 3. *6* Kết quả ước lượng mô hình ARIMA(0,1,0)*

Kết quả ước lượng mô hình ARIMA (0,1,0) cho thấy kiểm định Ljung-Box tại bậc trễ 1 cho p-value = 0,51 > 0,05, cho phép chấp nhận giả thuyết H₀ rằng phần dư không có tự tương quan bậc 1. Tuy nhiên, mô hình bộc lộ hai hạn chế đáng lưu ý. Thứ nhất, kiểm định phương sai không đồng đều (Heteroskedasticity) cho p-value = 0,00, bác bỏ giả thuyết phương sai không đổi và xác nhận hiện tượng volatility clustering đã quan sát được trong chuỗi phần dư. Thứ hai, kiểm định Jarque-Bera cho p-value = 0,00 cùng với hệ số kurtosis = 11,43 và độ lệch = -0,82 cho thấy phân phối phần dư lệch trái và có đuôi dày, phản ánh sự xuất hiện của các cú sốc giá cực đoan trong giai đoạn nghiên cứu. Những hạn chế này là đặc trưng phổ biến của dữ liệu tài chính và cho thấy ARIMA(0,1,0) chưa mô hình hóa đầy đủ cấu trúc phương sai có điều kiện của chuỗi giá.

Kết quả định dạng mô hình là ARIMA(0,1,0), tương đương với mô hình bước đi ngẫu nhiên (Random Walk). Đặc trưng thống kê của mô hình này là phương sai có xu hướng tăng dần theo thời, hàm ý rằng chuỗi dữ liệu gốc chứa nghiệm đơn vị và ở trạng thái không dừng , dẫn đến sự thiếu ổn định về mức. Tuy nhiên, sau khi thực hiện sai phân bậc 1, chuỗi dữ liệu đã chuyển thành nhiễu trắng ,qua đó đảm bảo tính dừng và thỏa mãn điều kiện ổn định cần thiết của mô hình phân tích.

1. <a name="_toc222824541"></a><a name="_toc223084310"></a>**Kết quả dự báo**

<img width="945" height="524" alt="image" src="https://github.com/user-attachments/assets/a3a05b5e-776a-43f9-99f1-fb645123c046" />

Kết quả cho thấy đường dự báo hội tụ về một giá trị gần như cố định xấp xỉ 40 nghìn đồng trong suốt giai đoạn dự báo, trong khi giá thực tế dao động đáng kể trong khoảng từ 32 đến 54 nghìn đồng. Đây là hành vi đặc trưng của mô hình ARIMA(0,1,0) - quá trình random walk, khi thực hiện dự báo nhiều bước: giá trị dự báo nhanh chóng hội tụ về mức trung bình dài hạn và mất khả năng theo dõi biến động thực tế của chuỗi. Kết quả này phù hợp với lý thuyết thị trường hiệu quả dạng yếu [(Fama, 1970),](https://datagolf.com/static/blogs/fl_bias/fama_1970.pdf) theo đó giá cổ phiếu không thể dự báo một cách hệ thống dựa trên thông tin giá lịch sử. Mặc dù mô hình đáp ứng tiêu chí thống kê về tính dừng của phần dư, khả năng dự báo thực tế bị hạn chế đáng kể, cho thấy sự cần thiết phải bổ sung thêm các biến ngoại sinh hoặc kết hợp với các phương pháp học máy nhằm nâng cao độ chính xác dự báo.

Khi tiến hành tính toán các chỉ số đánh giá độ chính xác của mô hình, nghiên cứu thu được các giá trị:

|Chỉ số|Giá trị|
| - | - |
|R-square|-0.3361|
|MAE|4\.8432|
|RMSE|6\.0464|
|MAPE(%)|10\.60%|
|MSE|36\.5596|

<a name="_toc222996657"></a><a name="_toc223045653"></a>*Bảng 3. *5* Các chỉ số đánh giá hiệu quả dự báo của mô hình*

Kết quả đánh giá hiệu suất dự báo của mô hình ARIMA(0,1,0) trên tập kiểm tra được trình bày trong Bảng 3.5. Chỉ số MAE = 4,84 cho thấy sai số tuyệt đối trung bình giữa giá dự báo và giá thực tế là 4,84 nghìn đồng. Chỉ số MAPE = 10,60% cho thấy trung bình mô hình dự báo lệch khoảng 10,6% so với giá thực tế, vượt ngưỡng 10% thường được coi là giới hạn chấp nhận được trong dự báo tài chính.

Chỉ số RMSE = 6,05 cao hơn đáng kể so với MAE = 4,84, phản ánh sự chênh lệch lớn giữa hai chỉ số này và cho thấy mô hình chịu ảnh hưởng mạnh từ các sai số cực đoan. Kết quả này nhất quán với quan sát về biến động giá lớn trong giai đoạn kiểm tra, đặc biệt khi giá cổ phiếu MWG trải qua các cú sốc mạnh xuống vùng 32 nghìn đồng và lên vùng 54 nghìn đồng."

Đáng chú ý nhất, hệ số R² = -0,336 là chỉ số phản ánh rõ nhất hạn chế của mô hình. Giá trị R² âm cho thấy ARIMA(0,1,0) có khả năng giải thích phương sai của chuỗi giá kém hơn cả mô hình dự báo đơn giản bằng giá trị trung bình, tức là mô hình không có giá trị dự báo thực sự trên tập kiểm tra."

Nhìn chung, các chỉ số đánh giá xác nhận rằng ARIMA(0,1,0) không đủ khả năng nắm bắt động thái biến động của giá cổ phiếu BID trong giai đoạn kiểm tra, phù hợp với phân tích trực quan khi đường dự báo hội tụ về mức cố định trong khi giá thực tế dao động mạnh. Kết quả này đặt ra yêu cầu cấp thiết phải xem xét các mô hình có khả năng mô hình hóa tính phi tuyến và phương sai thay đổi của chuỗi giá.

