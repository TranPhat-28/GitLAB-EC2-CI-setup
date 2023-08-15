# GitLAB-EC2-CI-setup
Setup CI pipeline for automatic deployment from gitLab repo to AWS EC2

1. Khởi tạo instance EC2 và cấu hình rule kết nối
●	Khởi tạo một instance EC2 mới trên AWS
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/e1a881a7-d611-4e4f-8a86-fd2c5b441e98)
 
●	Đặt tên cho instance
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/1cf3b199-5a7c-4a82-a4f9-65f3f3a79389)
 
●	Chọn loại instance là Amazon Linux
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/bb13f9ac-64e5-41d1-9143-14e6ebe839c8)
 
●	Ở mục Key pair chọn Create new key pair
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/5252c0b7-fd19-4269-9c60-49b96d6d91cc)
 
●	Đặt tên cho file key. Các option còn lại giữ nguyên mặc định
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/c3cf0d2f-3485-4984-bd88-9a2b05c79541)
 
●	Nhấn Create key pair. File key có đuôi .pem sẽ được tải xuống
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/7fb442b7-2d70-42a1-8ff8-f9cbff235f82)
 
●	Giữ nguyên tất cả các tùy chọn khác và chọn Launch instance
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/51bfff49-4c85-4ef3-bf6f-8aef1e57ddee)
 
●	Quay lại Dashboard, chọn vào instance vừa tạo xong
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/af1d5f11-e970-4c07-8d64-b90d29ab7788)

●	Note lại địa chỉ IPv4 public để truy cập vào instance.
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/b5fc44c6-26e7-4999-a8b7-98a39b8ed95d)
 
●	Ở phía dưới, chọn tab Security, chọn vào option ở mục Security groups
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/b047faea-8043-4fb6-9cd9-be6ee340292b)
 
●	Chọn Inbound rules, chọn Edit inbound rules
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/f2e2a20f-1e1d-4b46-af61-7fdeaa369a41)
 
●	Chọn Add rule:
  ○	Type chọn Custom TCP
  ○	Port range nhập 80 (dùng để kết nối đến server nginx sau này)
  ○	Source chọn Anywhere IPv4
  ○	Chọn Save rules
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/533771ad-bcc4-49e7-80fc-55132b5dec62)
 
●	Instance hiển thị Running là ok
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/0f4cf646-e2c9-45f9-87a2-f39537be64c3)
 
2. Setup gitlab-runner
●	Ở Dashboard click vào tên instance và chọn connect
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/a4c47d8b-9aa9-4c8b-8e66-15871511af23)
 
●	Chọn Connection Type là Instance Connect và nhấn Connect
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/2b419625-fe05-406c-855f-5e0c45b61d75)

●	Một tab mới sẽ được mở với terminal kết nối đến instance
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/5224d410-9eaf-4446-ae03-e0ff35b1e928)

●	Đầu tiên, cài đặt git, node và npm cho instance

```
$ sudo yum update -y
$ sudo yum install -y git
$ sudo yum install -y nginx
$ sudo yum install -y nodejs
$ sudo yum install -y npm
```

●	Tiếp theo, cài đặt gitlab-runner

```
$ sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
$ sudo chmod +x /usr/local/bin/gitlab-runner
$ sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
$ sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
$ gitlab-runner register
```

●	Đến đây giữ nguyên tab EC2 Instance Connect, quay lại repo Gitlab, vào Settings → CI/CD
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/ea1c0e34-ed89-4985-a372-584f7d444b26)
 
●	Chọn Expand để mở phần Runner
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/62cf3e44-20a3-460a-9a9d-d0aa39660279)
 
●	Copy URL và registration token ở option 2 (Set up a specific runner for a project) và dán vào terminal:
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/c675f955-6c8c-40cc-91c0-8d4ed9c47572)
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/33e46b04-a8cf-43fc-8ba4-f34074cff203)

●	Tiếp theo, đặt description và tag cho runner:
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/77b04068-f64c-45ee-948d-cd2f77f99e2d)
 
●	Optional maintenance để trống. Executor gõ shell và Enter
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/3cb095a6-f5dc-4c8b-90fd-9b37d06d3b07)
 
●	Cuối cùng, gõ gitlab-runner run để khởi chạy runner
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/f998e403-1a85-4b55-b67c-41eae9c723bd)

●	Quay lại trang Settings của repo, refresh lại, nếu thấy có runner với đúng description và tag đã đặt trước đó là đã oke
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/6e9e35cb-be14-416b-9054-9013d4cc096e)
 
●	Để kiểm tra runner đã hoạt động chưa, tạo một file .gitlab-ci.yml trong root của repo (có thể tạo trực tiếp online luôn cho nhanh). tags ở đây là tag của runner mà lúc nãy bạn đặt
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/54b560e1-8091-436c-82ca-4c671245d195)
 
●	Vào CI/CI → Jobs → Chọn vào job
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/08dbfcf5-33c7-4da9-a474-df07fb035ece)

●	Output Job succeeded như sau nghĩa là job đã thành công
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/16450c7d-2540-4652-86b5-e87065775e11)
 
3. Clone và build React App
●	Quay lại tab Instance Connect. Gõ CTRL + C để ngừng chạy gitlab-runner.
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/7b5af71e-74e7-487d-bd32-25255f778767)
 
●	Gõ pwd để lấy đường dẫn đến vị trí hiện tại (thông thường là /home/ec2-user )
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/d90fadd6-2971-40f3-893a-ba7c4ec3f417)
 
●	Dùng git để clone repo từ Gitlab về và tiến hành build

```
$ git clone https://<username>:<access_token>@gitlab.***/***/***.git
$ cd <repo_name>
$ npm i
$ npm run build
$ cd ./build
```

●	Có thể dùng ls để xem nội dung của thư mục build vừa được tạo. Đường dẫn hiện tại của thư mục này là /home/ec2-user/<repo_name>/build
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/843a211f-3741-4238-9b3f-1e70c80564d4)
 
4. Setup Nginx
●	Khi build một React app sẽ thu được các file html, css, js. Các file này cần phải được đặt vào một web server để có thể hoạt động. Vì vậy cần phải cài đặt nginx để deploy app.
●	Đầu tiên cần phải có Nodejs và nginx (đã cài đặt ở bước trước). Chạy các lệnh sau để kiểm tra version:
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/ec74f5cf-96a0-41b2-bc86-898eccf9ba29)

●	Tiếp theo chạy các lệnh sau để tạo thư mục /var/www/<tên thư mục> và copy toàn bộ file từ thư mục build của app của chúng ta vào thư mục này. Ở đây mình sẽ đặt tên thư mục là /myapp:

```
$ sudo mkdir /var/www
$ sudo mkdir /var/www/myapp
$ sudo cp -r /home/ec2-user/sample-pipeline-app/build/* /var/www/myapp
```

●	Chạy ls /var/www/myapp để kiểm tra lại lần nữa. Nếu thư mục này đã chứa toàn bộ nội dung từ thư mục build của app chúng ta là ok.
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/94d659c3-fc50-41f2-bfb4-53580a7978a0)

●	Tiếp theo cần set quyền đọc file
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/7b5da7f1-cf06-45f8-ab69-cbef9be5b474)
 
●	Tiếp theo tạo file config cho nginx
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/8d0a3f65-33d6-4b8a-b0b6-395c74ae575b)
 
●	Dán nội dung file như sau, giải thích:
  ○	nginx lắng nghe ở cổng 80
  ○	Thư mục root là /var/www/myapp, ta đã copy toàn bộ file build vào đây
  ○	Tìm file index là index, index.html hoặc index.htm
  ○	Server name là tên server, ở đây là instance ec2 của chúng ta. Thay địa chỉ ip của ec2 vào đây

```
server {
  listen 80;
  listen [::]:80;
  root /var/www/myapp/;
  index index.html index.htm;
  server_name <ec2_public_ip>;
    location / {
      try_files $uri $uri/ =404;
    }
}
```
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/1b8751d8-12c2-462a-9ae6-a2bf20bcf2bf)

●	Gõ CTRL + O rồi Enter để lưu lại (sau đó CTRL + X để thoát ra ngoài):
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/99b7be47-8869-4aa6-ba90-a3f352d21ba6)
 
●	Gõ lệnh sau để khởi chạy nginx:
```$ sudo systemctl start nginx```
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/9a6af339-6294-4cfb-8b21-4b61b91eba86)
 
●	Như vậy là server đã được setup xong. Mở tab mới và gõ địa chỉ ip public ec2 của chúng ta vào là sẽ truy cập được app:
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/607660f7-1830-47bf-bd3a-d662421e19a5)

5. Chỉnh sửa file YML để hoàn tất CI pipeline
●	Server nginx của chúng ta đã chạy. Bây giờ cần khởi chạy gitlab-runner lại và cài đặt file yml, để mỗi khi có commit mới lên repo, runner sẽ tự động thực hiện hết các tác vụ từ clone về, build, copy file và khởi chạy lại nginx.
●	Như vậy mỗi khi có commit mới, source code của ta sẽ được tự động cập nhật và deploy lên ec2 tự động và nhanh chóng nhất.
●	Đầu tiên, khởi chạy lại gitlab-runner
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/a13b76f4-d345-4c19-8c09-eafc6659ee8a)

●	Quay lại repo của chúng ta và chỉnh sửa file .gitlab-ci.yml như sau. Giải thích:
  ○	Pipeline có 2 job sẽ được thực hiện:
    ■	Build-job:
      ●	Di chuyển vào thư mục app (thay sample-pipeline-app thành tên thư mục trên ec2 của bạn)
      ●	checkout sang nhánh master. Ở đây chỉ deploy các thay đổi lên nhánh master
      ●	pull origin về, cài đặt lại các thư viện mới và build lại
    ■	Deploy-job:
      ●	Copy đè lại các file build mới vào thư mục root để nginx đọc được
      ●	Restart lại nginx
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/510a0109-316c-4e8c-9ebe-824886e06217)

●	Lưu và commit lại các thay đổi. Một pipeline mới sẽ tự động khởi chạy. Kết quả như sau là đã hoàn thành
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/d788d8cd-242c-4e5c-a1f7-f0f15d5183ab)
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/f57a1274-bc23-49d3-a088-1e8cab4a1223)
 
6. Kết thúc
●	Như vậy là pipeline CI đã được cài đặt xong. Từ giờ, mọi thay đổi lên nhánh master sẽ được tự động deploy lên instance ec2 của chúng ta một cách nhanh chóng.
●	Thử thay đổi nội dung trong App.js và commit:
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/d4fd088b-979d-4f86-bd1f-5bbc332926ae)
 
●	Pipeline đã pass
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/953349c4-cbc4-4504-ad3a-78a2a071157c)
 
●	Truy cập vào lại trang web là sẽ thấy nội dung mới ngay
![image](https://github.com/TranPhat-28/GitLAB-EC2-CI-setup/assets/62002249/bcf1de11-f3eb-409a-930e-b4950f8f2a72)
 
GoodLuck 🎉🎉🎉
