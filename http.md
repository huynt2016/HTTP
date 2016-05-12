#HTTP
##I.Khái niệm
- Hypertext Transfer Protocol (HTTP) là giao thức tầng ứng dụng, dùng để trao đổi, truyền tin siêu văn bản. HTTP là nền tảng của truyền thông dữ liệu cho World Wide Web.

##II.Thành phần
- HTTP hoạt động theo mô hình Client - Server.
- Ví dụ một máy tính được thiết lập để lưu trữ các tài liệu Hypertext (Cấu trúc văn bản sử dụng các liên kết logic - Siêu văn bản) được gọi là Web Server. Phía người sử dụng có một máy tính cùng với phần mềm hiểu được các tài liệu Hypertext, giao tiếp được với Web Server gọi là Web Browser (trình duyệt Web) hay Web Client. Hai phía Client và Server giao tiếp với nhau thông qua giao thức HTTP.

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/0T0HjMe.png">

##III.Cơ chế hoạt động
- HTTP chạy trên tầng ứng dụng trong mô hình OSI và chạy dưới nền giao thức TCP ở tầng vận chuyển.
- HTTP sử dụng port 80 (mặc định nhưng có thể thay đổi) để giao tiếp với các client và sử dụng bản tin response để đáp ứng lại bản tin request của client.
- Các bước giao tiếp giữa web-client và web-serverl:

###Bước 1: Thiết lập kết nối
- Kết nối được thiết lập theo phương thức bắt tay 3 bước.

<img src="http://kenhgiaiphap.vn/Images/kgplab/DDOS/kgp_DDoS01.JPG">

  <ul>
  <li>Quá trình 1: Web-client sẽ gửi một bản tin SYN đến cổng port 80 của máy chủ web để yêu cầu kết nối với các trường bản tin như seq, ack, windown size, len, stt.</li>
  <li>Quá trình 2: Khi máy chủ nhật được bản tin SYN của client sẽ gửi lại bản tin (SYN+ACK)trong đó bản tin ACK là để xác nhận đã nhận được bản tin SYN của clien và bản tin SYN của server để yêu câu khởi tạo với client với các trường bản tin tương tự như bản tin SYN của clien</li>
  <li>Quá trình 3: Cient gửi lại bản tin ACK cho máy server và xác nhận phiên kết nối đã được khởi tạo</li>
  </ul>

###Bước 2: Client gửi bản tin request
- Sau khi đã thiết lập phiên kết nối với máy chủ web-client sẽ gửi bản tin request đến server để yêu cầu lấy dữ liệu từ server

###Bước 3: Server gửi bản tin respose
- Khi nhận được bản tin resquest từ client. Server gửi lại bản tin ACK cho client xác nhận đã nhận được bản tin request của client và ngay sau đó sẽ gửi cho client gói tin có chưa dữ liệu mà client yêu cầu.

##IV.Chi tiết bản tin HTTP
###1.Bản tin request
- Là tin nhắn yêu cầu từ client gửi đến server. Có cấu trúc như sau:

	Request       = Request-Line              
                     *(( general-header        
						| request-header         
						| entity-header ) CRLF)  
                     CRLF
                     [ message-body ]     
					 
####a. Request-Line  
- Cấu trúc:

	`Request-Line   = Method SP Request-URI SP HTTP-Version CRLF`
	
- Ví dụ:

    `GET http://www.example.org/pub/WWW/TheProject.html HTTP/1.1`
	
- Danh sách Method:
	
	Method         = "OPTIONS"        
                   | "GET"              
                   | "HEAD"             
                   | "POST"           
                   | "DELETE"              
                   | "TRACE"          
                   | "CONNECT"             
                   | extension-method
    extension-method = token

- Request-URI:

    Request-URI    = "*" 
                   | absoluteURI 
                   | abs_path [ "?" query ] 
                   | authority
				   
####b. Request-header
- Cấu trúc:

    request-header = Accept                   
                   | Accept-Charset       
                   | Accept-Encoding          
                   | Accept-Language        
                   | Authorization            
                   | Host                
                   | If-Match               
                   | If-Modified-Since        
                   | If-None-Match          
                   | If-Range               
                   | Max-Forwards            
                   | Proxy-Authorization      
                   | Range                
                   | Referer               
                   | TE                       
                   | User-Agent       

###2.Bản tin Response
- Là bản tin phản hồi lại của Server sau khi nhận được yêu cầu của Client
- Cấu trúc:

    Response      = Status-Line              
                    *(( general-header        
                     | response-header        
                     | entity-header ) CRLF)  
                    CRLF
                    [ message-body ]        
			
- Status-Line

	Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF		

- Các chữ số đầu tiên của Status-Code định nghĩa lớp của phản ứng. Hai chữ số cuối cùng không có bất kỳ vai trò phân loại. Có 5 giá trị cho các chữ số đầu tiên:
<ul>
<li>1xx: Informational - Yêu cầu nhận được, quá trình tiếp tục</li>
<li>2xx: Success - Các hành động đã được nhận thành công, hiểu và chấp nhận</li>
<li>3xx: Redirection - hành động tiếp theo phải được thực hiện để hoàn thành theo yêu cầu</li>
<li>4xx: Client Error - Các yêu cầu chứa cú pháp xấu hoặc không thể thực hiện</li>
<li>5xx: Server Error - Các máy chủ không thực hiện một yêu cầu rõ ràng hợp lệ</li>
</ul>

- Chi tiết Status-Code

	       | "100": Tiếp tục 
	       | "101": Switching Protocols 
	       | "200": OK 
	       | "201": Tạo 
	       | "202": Được chấp nhận 
	       | "203": Thông tin chưa được 
	       | "204": Không có nội dung 
	       | "205": Đặt lại nội dung 
	       | "206": Nội dung Một phần 
	       | "300": Có nhiều lựa chọn 
	       | "301": Moved Permanently 
	       | "302": Tìm thấy 
	       | "303": Xem Khác 
	       | "304": Không thay đổi 
	       | "305": Sử dụng Proxy 
	       | "307": Temporary Redirect 
	       | "400": Bad Request 
	       | "401": trái phép 
	       | "402": Yêu cầu thanh toán 
	       | "403": Cấm 
	       | "404": Không tìm thấy 
	       | "405": Phương thức không được phép 
	       | "406": Không được chấp nhận 
	       | "407": Proxy Yêu cầu Chứng thực 
	       | "408": Thời gian yêu cầu ra 
	       | "409": Xung đột 
	       | "410": Cuốn 
	       | "411": Bắt buộc Chiều dài 
	       | "412": Điều kiện tiên quyết không 
	       | "413": Yêu cầu Entity Too Large 
	       | "414": Yêu cầu-URI Too Large 
	       | "415": không được hỗ trợ loại truyền thông 
	       | "416": Yêu cầu dao không phù 
	       | "417": Kỳ vọng Không 
	       | "500": Lỗi máy chủ nội bộ 
	       | "501": Không thực hiện 
	       | "502": Bad Cổng 
	       | "503": Service Unavailable 
	       | "504": Gateway Time-out 
	       | "505": HTTP Version không được hỗ trợ 
	   
##V. Tham khảo
- https://github.com/NguyenHoaiNam/HTTP--Hypertext-transfer-protocol
- https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Server_response
- https://www.w3.org/Protocols/HTTP/1.1/rfc2616bis/draft-lafon-rfc2616bis-03.html#request.header.fields
