![image](https://github.com/Tuanvu133/test-svtech/assets/95236990/63b9006f-02dc-41c7-b0e1-158e86b40c13)# test-svtech
01 - Build a script to install base servers for a production environment, and use any automation tools that suit you (ansible*, salt stack, terraform, bash...). The solution should set up the following components, and make any changes as you see fit so that we can assess your consideration for a production-grade system.
"Sysadmin" accounts with sudo privilege; hostname (dns); cli commands you use often.
Install docker daemon, specify logging driver + storage driver of your choice.
The server should be prepared/tuned for a high network traffic workload.
Logging Every Command Executed by Users and saving in a specific file.

02 - Describe your idea to monitor the resource utilization of a Tech Stack. Draw a model that is Scalable, Resilient, and Responsive to the most traffic you have ever deployed. 

03 - When you receive an alert that a service is down or slow, what would you do to check that service and resolve the issue? Describe your steps to resolve and prevent the problem (you can describe a real situation that you have encountered).


PART 1:
- Ansible Playbook được thiết kế để cài đặt và cấu hình các thành phần cơ bản trên một máy chủ. Nó bao gồm các bước như tạo người dùng sysadmin, thiết lập hostname, cài đặt Docker, và thực hiện các tùy chỉnh hệ thống.
  
1. **Install Required Packages**
   - Sử dụng Ansible `apt` module để cài đặt các gói phần mềm cần thiết như `software-properties-common`, `python3-pip`, `python3-setuptools`, và `python3-apt`.
    
2. **Add Sysadmin Accounts**
Sử dụng Ansible user module để thêm người dùng sysadmin và thêm vào nhóm sudo để có đặc quyền.

3. **Set Hostname**
Sử dụng Ansible hostname module để đặt hostname của máy chủ thành "base-server".

4. **Install Docker**
Sử dụng Ansible apt module để cài đặt Docker.

5. **Chỉ định trình điều khiển lưu trữ và ghi nhật ký**
Sử dụng Ansible copy module để tạo và điền nội dung vào tệp cấu hình Docker /etc/docker/daemon.json.

6. **Chuẩn bị/Điều chỉnh cho khối lượng công việc có lưu lượng mạng cao**
Sử dụng Ansible sysctl module để đặt các tham số hạt nhân cho công việc mạng có tải cao.

8. **Kích hoạt tính năng ghi nhật ký lệnh của người dùng**
Sử dụng Ansible lineinfile module để thêm cấu hình cho việc ghi log mỗi lệnh thực thi vào /etc/bash.bashrc.

10. **Restart Docker Service**
Sử dụng Ansible systemd module để khởi động lại dịch vụ Docker để áp dụng các thay đổi.

