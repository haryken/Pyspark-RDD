# Pyspark-RDD
## RDD (Tập dữ liệu phân tán có khả năng phục hồi) là gì?

RDD (Tập dữ liệu phân tán có khả năng phục hồi) là một khối xây dựng cơ bản của PySpark, là bộ sưu tập đối tượng phân tán không thay đổi, chịu được lỗi. Ý nghĩa bất biến khi bạn tạo RDD, bạn không thể thay đổi nó. Mỗi bản ghi trong RDD được chia thành các phân vùng logic, có thể được tính toán trên các nút khác nhau của cụm. 

Nói cách khác, RDD là một tập hợp các đối tượng tương tự như danh sách trong Python, với sự khác biệt là RDD được tính toán trên một số quy trình nằm rải rác trên nhiều máy chủ vật lý còn được gọi là các nút trong một cụm trong khi tập hợp Python tồn tại và xử lý chỉ trong một quy trình.

Ngoài ra, RDD cung cấp sự trừu tượng hóa dữ liệu của việc phân vùng và phân phối dữ liệu được thiết kế để chạy tính toán song song trên một số nút, trong khi thực hiện các phép biến đổi trên RDD, chúng ta không phải lo lắng về tính song song như PySpark cung cấp theo mặc định.

Hướng dẫn Apache PySpark RDD này mô tả các thao tác cơ bản có sẵn trên RDDs, chẳng hạn như  map(), filter()và  persist()và nhiều hơn nữa. Ngoài ra, hướng dẫn này cũng giải thích các hàm RDD Ghép nối hoạt động trên RDD của các cặp khóa-giá trị như  groupByKey() và join()v.v.
Lưu ý: RDD có thể có tên và số nhận dạng duy nhất (id)

## Lợi ích của PySpark RDD

PySpark được thích nghi rộng rãi trong cộng đồng Học máy và Khoa học dữ liệu do những ưu điểm của nó so với lập trình python truyền thống.

##Xử lý trong bộ nhớ

PySpark tải dữ liệu từ đĩa và xử lý trong bộ nhớ và giữ dữ liệu trong bộ nhớ, đây là điểm khác biệt chính giữa PySpark và Mapreduce (I/O chuyên sâu). Giữa các lần biến đổi, chúng ta cũng có thể lưu cache / duy trì RDD trong bộ nhớ để sử dụng lại các tính toán trước đó.

## Tạo RDD

RDD được tạo ra chủ yếu theo hai cách khác nhau,song song hóa một bộ sưu tập hiện có vàtham khảo một tập dữ liệu trong một hệ thống bên ngoài lưu trữ ( HDFS, S3và nhiều hơn nữa). 
Trước khi chúng ta xem xét các ví dụ, trước tiên hãy khởi tạo SparkSession bằng cách sử dụng phương thức mẫu xây dựng được định nghĩa trong lớp SparkSession. Trong khi khởi tạo, chúng ta cần cung cấp tên chính và ứng dụng như hình bên dưới. Trong ứng dụng thời gian thực, bạn sẽ vượt qua master từ spark-submit thay vì hardcoding trên ứng dụng Spark

![image](https://user-images.githubusercontent.com/64195026/109417729-7151c780-79f7-11eb-9037-862adddd2d75.png)

master() - Nếu bạn đang chạy nó trên cụm, bạn cần sử dụng tên chính của mình làm đối số cho chủ (). thông thường, nó sẽ là một trong hai  yarn (Yet Another Resource Negotiator)hoặc  mesos tùy thuộc vào thiết lập cụm của bạn.

Sử dụng local[x]khi chạy ở chế độ Độc lập. x phải là một giá trị nguyên và phải lớn hơn 0; điều này thể hiện số lượng phân vùng nó sẽ tạo khi sử dụng RDD, DataFrame và Dataset. Tốt nhất, giá trị x phải là số lõi CPU bạn có.

appName() - Được sử dụng để đặt tên ứng dụng của bạn.

getOrCreate() - Điều này trả về một đối tượng SparkSession nếu đã tồn tại, tạo một đối tượng mới nếu chưa tồn tại.

### Tạo RDD bằng sparkContext.parallelize ()
Bằng cách sử dụng parallelize()hàm của SparkContext ( sparkContext.parallelize () ), bạn có thể tạo RDD. Hàm này tải bộ sưu tập hiện có từ chương trình trình điều khiển của bạn vào song song hóa RDD. Đây là phương pháp cơ bản để tạo RDD và được sử dụng khi bạn đã có dữ liệu trong bộ nhớ được tải từ tệp hoặc từ cơ sở dữ liệu. và nó yêu cầu tất cả dữ liệu phải có trên chương trình trình điều khiển trước khi tạo RDD.

![image](https://user-images.githubusercontent.com/64195026/109417742-77e03f00-79f7-11eb-9e33-cba5b84f9f93.png)

### Tạo RDD bằng sparkContext.textFile ()

Sử dụng phương thức textFile (), chúng ta có thể đọc tệp văn bản (.txt) vào RDD.

![image](https://user-images.githubusercontent.com/64195026/109417744-7adb2f80-79f7-11eb-9142-b5ae0aeb8d97.png)

### Tạo RDD bằng sparkContext.wholeTextFiles ()

Hàm wholeTextFiles () trả về một PairRDD với khóa là đường dẫn tệp và giá trị là nội dung tệp.

![image](https://user-images.githubusercontent.com/64195026/109417748-7d3d8980-79f7-11eb-827f-1a98bf186292.png)

Bên cạnh việc sử dụng các tệp văn bản, chúng ta cũng có thể tạo RDD từ tệp CSV , JSON và nhiều định dạng khác.

### Tạo RDD trống bằng sparkContext.emptyRDD

Sử dụng emptyRDD()phương thức trên sparkContext, chúng ta có thể  tạo một RDD không có dữ liệu . Phương pháp này tạo ra một RDD trống không có phân vùng

![image](https://user-images.githubusercontent.com/64195026/109417754-80387a00-79f7-11eb-9aca-cfae13aa39b2.png)

### Tạo RDD trống với phân vùng

Đôi khi, chúng ta có thể cần ghi RDD trống vào các tệp theo phân vùng, Trong trường hợp này, bạn nên tạo RDD trống có phân vùng.

![image](https://user-images.githubusercontent.com/64195026/109417759-862e5b00-79f7-11eb-8045-eb2eba31b8cc.png)

### RDD Song song hóa

Khi chúng tôi sử dụng parallelize()hoặc textFile()hoặc  wholeTextFiles()các phương thức của SparkContxt để khởi tạo RDD, nó sẽ tự động chia dữ liệu thành các phân vùng dựa trên tính khả dụng của tài nguyên. khi bạn chạy nó trên máy tính xách tay, nó sẽ tạo các phân vùng có cùng số lượng lõi có sẵn trên hệ thống của bạn.

getNumPartitions () - Đây là một hàm RDD trả về một số phân vùng mà tập dữ liệu của chúng tôi được chia thành.

![image](https://user-images.githubusercontent.com/64195026/109417927-4451e480-79f8-11eb-9d43-bb99fc44eb9c.png)

Đặt song song theo cách thủ công - Chúng ta cũng có thể đặt một số phân vùng theo cách thủ công, tất cả những gì chúng ta cần là chuyển một số phân vùng làm tham số thứ hai cho các hàm này chẳng hạn   sparkContext.parallelize([1,2,3,4,56,7,8,9,12,3], 10).

