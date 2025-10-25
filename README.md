# BÀI TẬP 02 - LẬP TRÌNH WEB
# LƯỜNG VĂN HẠNH - K225480106013
## NGÀY GIAO: 19/10/2025
## DEADLINE: 26/10/2025
==============================
# I. YÊU CẦU
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.

# II.NỘI DUNG BÀI TẬP
## 2.1. Cài đặt Apache web server
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server:
  Truy cập: https://www.apachelounge.com/download/
  Chọn file httpd-2.4.65-250724-win64-VS17.zip để tải về. 
- Giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf

    Sửa các chỗ:
    Define SRVROOT "E:/DataAutoBackup/LEARN-CODE/PhatTrienUngDungTrenNenWeb"
    
    ServerName localhost:80

    Include conf/extra/httpd-vhosts.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
    
    <VirtualHost *:80>
        ServerAdmin admin@luongvanhanh.com
        DocumentRoot "E:/DataAutoBackup/LEARN-CODE/PhatTrienUngDungTrenNenWeb/luongvanhanh"
        ServerName luongvanhanh.com
        ErrorLog "logs/luongvanhanh-error.log"
        CustomLog "logs/luongvanhanh-access.log" common
    
        <Directory "E:/DataAutoBackup/LEARN-CODE/PhatTrienUngDungTrenNenWeb/luongvanhanh">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>
    </VirtualHost>
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `E:\DataAutoBackup\LEARN-CODE\PhatTrienUngDungTrenNenWeb\luongvanhanh` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Lường Văn Hạnh` thì tạo website với domain là fullname ko dấu, liền nhau: `luongvanhanh.com`
- thao tác dòng lệnh trên file `E:\DataAutoBackup\LEARN-CODE\PhatTrienUngDungTrenNenWeb\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
  
 <img width="1919" height="507" alt="image" src="https://github.com/user-attachments/assets/dd6f5c4f-686d-435e-bac8-9505d2ddfe8c" />

## 2.2. Cài đặt nodejs và nodered => Dùng làm backend
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `E:\DataAutoBackup\nodejs`
   <img width="1371" height="652" alt="image" src="https://github.com/user-attachments/assets/a9ed3d61-c513-4336-9b59-c0629d439387" />

- Cài đặt nodered:
  + chạy cmd, vào thư mục `E:\DataAutoBackup\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "E:\DataAutoBackup\nodejs"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `E:\DataAutoBackup\nodejs\`
  + tạo file "E:\DataAutoBackup\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=E:\DataAutoBackup\nodejs;%PATH%
REM Run Node-RED
node "E:\DataAutoBackup\nodejs\nodered\node_modules\node-red\red.js" -u "E:\DataAutoBackup\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `E:\DataAutoBackup\NODEJS\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "E:\DataAutoBackup\NODEJS\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
    <img width="1351" height="767" alt="image" src="https://github.com/user-attachments/assets/df99c80a-75b6-4628-a6fc-8f6b0c302838" />
    
<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/b9f60f93-303b-4988-8c70-eb8aadad4b2f" />

## 2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
- Ok. Đã có sẵn csdl My_Profile với thông tin: ip=127.0.0.1, port=1433, username=sa, password=********, db_name=My_Profile,
- table_name: ThongTinCaNhan, HocVan, KyNang, SoThich, KinhNghiemLamViec.
## 2.4. Cài đặt thư viện trên nodered
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
  Ấn nút 3 gạch góc trên bên phải ==>Manager palette => chọn tab Install ==> Tìm kiếm thư viện cần tìm
  <img width="1915" height="799" alt="image" src="https://github.com/user-attachments/assets/662aca64-d8ba-4963-a514-bb2f787b8f25" />

- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },
  <img width="1525" height="362" alt="image" src="https://github.com/user-attachments/assets/cd275e15-ffb4-4628-a90e-9da986ae4b18" />
  
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  <img width="1308" height="330" alt="image" src="https://github.com/user-attachments/assets/044173c4-d88b-42e4-a9ce-7924628e36ba" />

  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
  <img width="1919" height="847" alt="image" src="https://github.com/user-attachments/assets/cfb621a0-3a1d-46a8-a472-9735ee02a163" />

## 2.5. tạo api back-end bằng nodered
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
     <img width="1919" height="825" alt="image" src="https://github.com/user-attachments/assets/95a54b0f-4c7b-436b-aa2f-c16c9106414f" />

  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=hạnh
  <img width="1919" height="773" alt="image" src="https://github.com/user-attachments/assets/aed7b748-5501-4ab5-9818-cc508d0fe85b" />

## 2.6. Tạo giao diện front-end
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
  <img width="1919" height="688" alt="image" src="https://github.com/user-attachments/assets/88a08a96-7533-4e86-9145-67fef9be8f80" />

- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
  <img width="1914" height="1079" alt="image" src="https://github.com/user-attachments/assets/ac343b61-ea61-4c7d-8d6b-3e7498270c33" />

## 2.7. Nhận xét bài làm của mình
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
  Qua quá trình làm bài tập em hiểu khá rõ về các cài đặt các phần mềm, hiểu rõ cài đặt thư viện trong nodered. Biết các cấu hình các file, chạy các dịch vụ, hiểu cách tạo ra 1 website với fake domain trên máy tính của mình.
  + Khi em cài Apache, trong file httpd.conf (file cấu hình chung cơ bản cho Apache) cần chú ý: Define SRVROOT phải có path chính xác tới vị trí đan cài đặt Apache24, cần module nào thì bỏ comment module đó đi là dùng được. Sau khi đã có active module và modified SRVROOT, tiếp theo là chỉnh các file cấu hình cụ thể trong module active, trong file httpd-vhosts.conf ta thấy ngay hướng dẫn tại những dòng đầu tiên, và đây là lý do ta phải active đúng module trong file httpd.conf trước đó:
    <img width="829" height="158" alt="image" src="https://github.com/user-attachments/assets/441ea87b-8f45-4e84-9093-86830cf45ea8" />
    Sau khi xác nhận là đã làm đúng rồi thì nó có phần hướng dẫn tạo hosts ảo cho website, thích bao nhiêu website tuỳ ý, miễn là làm đúng theo mẫu. Tuy nhiên để an toàn thì nên chuẩn bị 1 host fallback cho Apache, khi client gửi request đến 1 domain không khớp với ServerName thì Apache sẽ dùng host fallback này để hiển thị:
    
   <img width="1462" height="310" alt="image" src="https://github.com/user-attachments/assets/76c17703-485d-4c71-be0c-d0bc2aeb889d" />

       Còn dưới đây là cấu trúc thiết lập cho bất kỳ host ảo nào mong muốn:
   <img width="1503" height="446" alt="image" src="https://github.com/user-attachments/assets/06a04fdc-29bf-4752-b13f-f04e9a21f22f" />

   + Đối với nodejs,nodered: Cần chú ý download đúng bản win64(phù hợp với hệ điều hành window hiện nay). Các lưu ý em nhận ra trong khi làm: Thư mục cài đặt nodejs phải có tên là "nodejs", nếu đặt khác sẽ gây lỗi tùm lum, bằng chứng là em đã đặt tên thư mục là NODEJS và boommm...(lỗi tùm lum). Các bước cài đặt làm như trên.
   + Lưu ý chung: Khi cài đặt 1 phần mềm hay dịch vụ nào đó ta cần chú ý đến: Đường dẫn nó cài đặt, tên thư mục chứa nó, ip mở nó, port (cổng) mà nó chiễm dụng, vấn đề về port là rất hay xảy ra, điều này làm cho các phần mềm (dịch vụ) hoặc là không nghe được port hoặc là tranh nhau port, khi điều đó xảy ra thì các phần mềm sẽ không hoạt động hoặc hoạt động sai.
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
  Em đã dùng qua nodejs để tạo website trong đồ án trước và trong 1 vài dự án cá nhân (nghịch code) nhưng toàn đi copy trên chatGPT, vì nó quá khó hiểu. Giờ thì nodered đã giúp em đá đít nodejs ra chuồng gà, nó dễ chịu hơn nodejs nhiều, nhìn giao diện UI cũng đẹp mắt hơn. Trong bài trên chỉ sử dụng 4 node cơ bản khép kín, dưới đây là kiến thức em thu nạp được:
  + Node http in: Nhận request từ client với dữ liệu và method phù hợp (GET/POST/PUT/DELETE...). Khi show tab network, key+value nằm trong payload.
  + Node function: Tiền xử lý dữ liệu trước khi gửi tới db. Ở đây nó sẽ nhận được và xử lý request từ client gửi lên trước khi gửi tới db. Nó định dạng dữ liệu và định hình truy vấn phù hợp với từng loại CSDL để lấy được thông tin chính xác nhất.
  + Node MSSQL: Kết nối và truy vấn CSDL SQL Server. Đây là thư viện chuẩn dùng để kết nối và truy vấn SQL Server bằng chính truy vấn mà node function gửi lên, thư viện này cho phép sử dụng luôn code truy vấn SQL mà không cần phải dịch từ ngôn ngữ khác.
  + Node http response: Gửi dữ liệu từ NodeRed về client. Sau khi db trả về dữ liệu(hoặc lỗi) no sẽ gửi đến client, ở trình duyệt phía client mở tab network sẽ thấy dữ liệu được trả về trong response, giờ thì front-end chỉ cần đọc đúng cấu trúc của dữ liệu được trả về và view ra giao diện.
- đã hiểu cách frond-end tương tác với back-end ra sao?
  + Người ta hay ví back-end (BE) như là người đầu bếp, API như là người phục vụ, front-end (FE) như khách hàng; Trên tinh thần đó, ánh xạ sang dự án website ta có BE xử lý dữ liệu, API là cầu nối mang những yêu cầu từ phía end user đến cho BE và mang những dữ liệu mà BE trả về cho FE hiển thị ra giao diện.
  + Trong bài này, khi em đã xây dựng BE thành công, API chạy ngon lành, dữ liệu trả về chính xác thì tức là đã xong phần lớn website rồi. Phần còn lại là viết FE tạo cho website giao diện đẹp như mong muốn, khi end user thao tác vào giao diện, chẳng hạn nhập vào form, ấn nút gửi, khi này request đấy được gửi đúng tới API đã setup sẵn trong code BE và dữ liệu mong muốn sẽ được trả về FE, đúng kiểu đến là đón. Và vị trí gọi tới API sẽ nằm trong file js, render html cũng nằm luôn ở file js. Hay nói cách khác html là khung xương của trang web (nó chứa text, input, button, table, image, video,...), css là làm đẹp cho trang web (nó là các chuyển động color, border, position, keyframe, media, hover, highlight, change color, scale, rotate,...), js là các logic phía FE và giao tiếp với API, nó cũng tham gia vào các chuyển động phức tạp hơn css, render content,...
==============================
# TIÊU CHÍ CHẤM ĐIỂM
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
# GHI CHÚ
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
