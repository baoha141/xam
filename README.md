# Practical-Malware-Analysis-Lab

Lab 6-2
5. Are there any network-based indicators for this program? 
- Internet Explorer 7.5/pma
- http://www.practicalmalwareanalysis.com

6. What is the purpose of this malware?
Mã độc sẽ kết nối đến internet, đọc từng bit đầu file html có chứa comment, comment sẽ bắt đầu bằng <!--X, xuất ra màn hình "Success: Parsed command is %c\n" với %c là X, sau đó Sleep 60 giây.

Lab 6-3
Phân tích
-	adcapi32 cho thấy mã độc có thể sửa key register.

 

Câu hỏi
1. Compare the calls in main to Lab 6-2’s main method. What is the new function called from main? 
Hàm main ở Lab3 có thêm hàm sub_401130 so với lab2.
 

2. What parameters does this new function take? 
Hàm push vào 2 tham số, tham số đầu tiên là command X lấy từ comment đầu file html, tham số thứ 2 là argv[],
 
3. What major code construct does this function contain? 
Major code construct của hàm là bảng swich
4. What can this function do? 
Hàm dựa vào giá trị command để lựa chọn case thực thi. Có 5 switch case:
- Tạo thư mục "C:\\Temp"
- Copy file argv[] push vào hàm đến file mới "C:\\Temp\\cc.exe"
- Xóa file "C:\\Temp\\cc.exe"
- Mở key “Software\Microsoft\Windows\CurrentVersion\Run” và thêm vào “Software\Microsoft\Windows\CurrentVersion\Run\Malware” là "C:\\Temp\\cc.exe" để file  có thể thực thi mỗi khi mở máy
- Ngủ 100 giây
Nếu command không có trong các switch case thì in ra màn hình "Error 3.2: Not a valid command provided"
5. Are there any host-based indicators for this malware? 
- C:\\Temp\\cc.exe
- Software\Microsoft\Windows\CurrentVersion\Run\Malware
6. What is the purpose of this malware?
Mã độc sẽ kết nối đến internet, đọc từng bit đầu file html có chứa comment, comment sẽ bắt đầu bằng <!--X, xuất ra màn hình "Success: Parsed command is %c\n" với %c là X. 
Dựa vào command để quyết định hành động tiếp theo là: tạo thư mục, copy file, xóa file, thêm key hoặc ngủ 100 giây.


Lab 6-4
Phân tích

Câu hỏi
1. What is the difference between the calls made from the main method in Labs 6-3 and 6-4?
 
2. What new code construct has been added to main? 
Code construct mới  được thêm vào hàm main là vòng lặp for
3. What is the difference between this lab’s parse HTML function and those of the previous labs? 
Hàm parse HTML ở những lab trước cố định user-agent là Internet Explorer 7.5.
Hàm parse HTML  user-agent là "Internet Explorer 7.50/pma%d". Hàm push các tham số szAgent vào đầu buffer, format "Internet Explorer 7.50/pma%d", biến đếm vòng lặp, vậy giá trị user-agent thay đổi sau mỗi vòng lặp. 

4. How long will this program run? (Assume that it is connected to the Internet.) 
Hàm sẽ chạy 1440 phút = 24 tiếng
Điều kiện thoát vòng lặp là lớn hơn 1440. Và trong mỗi vòng lặp chương trình sẽ ngủ 60 giây
5. Are there any new network-based indicators for this malware? 
"Internet Explorer 7.50/pma%d"
6. What is the purpose of this malware?
Mã độc kết nối đến máy chủ, gửi giá trị user-agent khác nhau mỗi lần kết nối. Khi kết nối đến thì mã độc đọc ký tự bắt đầu comment ở đầu file html <!--command, mã độc lấy giá trị command, dựa vào gái trị này sẽ thực hiện hành động khác nhau: tạo thư mục, copy file, xóa file, thêm register key hoặc ngủ 100 giây. Sau đó mã độc sẽ ngủ trong 60s. Mã độc sẽ chạy trong vòng 24 tiếng (1440 vòng lặp) trước khi kết thúc.



