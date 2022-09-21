Định dạng PDF là tiêu chuẩn thực tế trong việc trao đổi tài liệu trực tuyến. Tuy nhiên, sự phổ biến như vậy cũng đã thu hút bọn tội phạm mạng phát tán phần mềm độc hại đến những người dùng không nghi ngờ. Khả năng tạo các tệp pdf độc hại để phân phối phần mềm độc hại là một chức năng đã được tích hợp trong nhiều bộ công cụ khai thác. Do người dùng ít thận trọng hơn trong việc mở tệp PDF, tệp PDF độc hại đã trở thành một vectơ tấn công khá thành công. Lưu lượng mạng được ghi lại trong lala.pcap chứa lưu lượng mạng liên quan đến một cuộc tấn công tệp PDF độc hại điển hình, trong đó người dùng không nghi ngờ sẽ mở một trang web bị xâm phạm, chuyển hướng trình duyệt web của người dùng đến URL của tệp PDF độc hại. Khi trình cắm PDF của trình duyệt mở PDF, phiên bản chưa vá lỗi của Adobe Acrobat Reader được khai th

# TOOLs:
- [De4js](https://lelinhtinh.github.io/de4js/)
- [pdfid](https://github.com/Rafiot/pdfid)
- [pdfparter](https://github.com/smalot/pdfparser)
- [peepdf](https://github.com/jesparza/peepdf)
- Wireshark
- Network Minner
- Detail chall: [Here](https://cyberdefenders.org/blueteam-ctf-challenges/47)

Thử thách này mình đã vượt qua khoảng 80% và để hoàn thiện thử thách mình vừa làm và vừa tìm hiểu thêm về các kỹ thuật và cài đặt 1 số tool Forensic PDF files, có 1 số câu khá dễ dàng để submit, nhưng đó chỉ là bề nổi của tảng băng chìm- điều cơ bản của một chuyên viên Forensic cần biết. Sau đây là cách mà cá nhân mình đã vượt qua thử thách này!!!

- Vì đây là một thử thách kết hợp với gói tin cho nên đầu tiên chúng ta phải thực hiện phân tích gói tin để tìm kiếm các thông tin có liên quan đến chủ đề này. 
- Chúng ta có thể sử dụng một số công cụ khác nhau để thu thập những thông tin cần thiết. Ví dụ mình có thể thấy có 3 địa chỉ IP tham gia cuộc hội thoại này.

![image](https://user-images.githubusercontent.com/42565778/191223643-f7123561-8e9b-4932-8fcd-ad90e3e70617.png)

- Tiếp tới là các file hoặc đường link có thể export trong quá trình thu thập, có khoảng 7 truy vấn

![image](https://user-images.githubusercontent.com/42565778/191223772-c9cb006d-d8a5-462b-8ba3-bfc06f10433c.png)

- Ngoài ra có thể thu thập các thông tin khác liên quan, thông tin trên virustotal cũng là một hình thức và điều này mình sẽ sử dụng hashsum nhé!!!

![image](https://user-images.githubusercontent.com/42565778/191224015-97660419-ae0d-4e73-bf1b-928b659a7d12.png)

- Nếu các bạn có cách thu thập thông tin được nhanh hơn và hữu ích hơn thì góp ý để mình rút kinh nghiệm. CÒn bây giờ đi vào thử thách này thôi nào.

## 1. How many URL path(s) are involved in this incident?

- Dựa vào kết quả đã thu thập ở bên trên thì có khoảng 7 packet tham gia vào giao tiếp trong đó có 1 packet được lặp lại. Như thế kết quả sẽ là 6 hoặc 7 thôi.

![image](https://user-images.githubusercontent.com/42565778/191224298-cb136f71-a984-44ad-98b4-d673e9e4bce3.png)

_submit: 6_

## 2. What is the URL which contains the JS code?

- Dựa vào việc kết nối từ máy bạn nhân được lưu thông tin lại thì mình đã tìm được ngay thông tin này 

![image](https://user-images.githubusercontent.com/42565778/191226022-56aeb7e0-12d3-4ded-ab01-e6c0ef966e91.png)

_sumit: http://user- ........._

## 3. What is the URL which contains the JS code?

- Sau khi đã lọc hết các JS có thể tham gia thì còn lại là câu lệnh gọi tới tải file getpdf.php chính vì thế mà tôi submit kết quả và thành công :v tuy hên xui nhưng có vẻ chúng ta cần phải làm rõ điều này...

_sumit: http://user- ........._

## 4. What is the MD5 hash of the PDF file contained in the packet?

- Sau khi sử dụng chức năng export cơ bản của wireshark thì cuối cùng đã thu thập được file PDF và chỉ cần hash và submit kết quả thôi nào

![image](https://user-images.githubusercontent.com/42565778/191225029-1a31744d-78c3-4d48-9141-5de30569310e.png)

_submit: 659c............_

## 5. How many object(s) are contained inside the PDF file?

- Có tất cả 18 Object hoạt động bình thường và 1 Object bị lỗi, do đó kết quả tổng sẽ là 19.

![image](https://user-images.githubusercontent.com/42565778/191435607-82ff6fe4-5736-463d-b2b7-3ecc8fe1d6dc.png)

_submit: 19_

## 6. How many filtering schemes are used for the object streams?

- Sử dụng các chức năng lọc Filters cơ bản của  peepdf và chúng ta thu được kết quả là 4 filters

![image](https://user-images.githubusercontent.com/42565778/191435848-1396e9f3-7caf-4a41-a796-fb45d6f8a8df.png)

_submit: 4_

## 7. What is the number of the 'object stream' that might contain malicious JS code?

- Trong quá trình thực hiện thì tôi dò khá nhiều object từ 1 tới 20 để kiểm tra, cho tới object 5 thì thu được kể quả 

![image](https://user-images.githubusercontent.com/42565778/191436126-e47308fb-01e8-4b57-a2ab-3c83c76efc2b.png)

_submit: 5_

## 8. Analyzing the PDF file. What 'object-streams' contain the JS code responsible for executing the shellcodes? The JS code is divided into two streams. Format: two numbers separated with ','. Put the numbers in ascending order

- Tương tự như câu thứ 7 tôi cũng ngồi dò từng object thì tới 7, 8, 9 thì nhận các kết quả là các payload hoặc là các đoạn mã hóa nào đó ví dụ như này

![image](https://user-images.githubusercontent.com/42565778/191436498-8469e93b-a042-4e97-bc62-1387cd437e6a.png)

- Nhưng mà khả năng phân tích còn kém tôi đã mở hint để tìm kiếm cách làm khác thông minh hơn và tư duy trong thực tế tốt hơn, nhưng dường như chưa tìm được cách nào khác ngoài việc ngồi mò mẫm

_submit: 7..._

## 9. The JS code responsible for executing the exploit contains shellcodes that drop malicious executable files. What is the full path of malicious executable files after being dropped by the malware on the victim machine?

- Câu 9 do chưa hiểu cách thức thực hiện và khả năng RE còn hạn chế do đó việc tìm kết quả với tôi khá khó khăn và tôi đã bỏ cuộc để thực hiện các thử thách khác.

_submit: c:\WINDOWS\system32\a.exe_

## 10. The PDF file contains another exploit related to CVE-2010-0188. What is the URL of the malicious executable that the shellcode associated with this exploit drop?

- Dựa vào việc thu thập thông tin từ CVE này, nhưng dựa vào việc lọc gói tin thì nhanh chóng mình đã tìm ra tệp tin đã được tải về

![image](https://user-images.githubusercontent.com/42565778/191229224-c90585c0-7ea8-4168-81cb-15e7e3734954.png)

_submit: the_real........._

## 11. How many CVEs are included in the PDF file?

- Dựa vào việc liệt kê các chú thích và các đoạn đoạn JS đã bị ẩn đi mà peepdf đã liệt kê được tôi dự đoán rằng các lỗ hổng đang được khai thác dựa trên object 5. Chính vì thế tôi đã tìm kiếm các thông tin có liên quan đến Object 5 là 7, 9, 10, 11, và phán đoán rằng chắc hẳn các trường thông tin này là các truy vấn được gọi đến để khai thác lỗ hổng. bên cạnh đó là các thông tin theo như Sanbox và virustotal cho ra thì tôi đã đoán lỗ hổng có 4 hoặc 5 hoặc 6. Thử submit đáp án nhé.

![image](https://user-images.githubusercontent.com/42565778/191432020-d4c4fb78-dcaa-417f-905b-af1e81ed50dd.png)

- Có thể đây là cách làm không hữu ích nhưng nó có thể áp dụng 1 số kinh nghiệm nhất định để kiểm tra.

_submit:5_

