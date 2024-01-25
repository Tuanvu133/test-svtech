# test-svtech
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
     
- name: Install required packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - software-properties-common
    - python3-pip
    - python3-setuptools
    - python3-apt
2. **Add Sysadmin Accounts**
Sử dụng Ansible user module để thêm người dùng sysadmin và thêm vào nhóm sudo để có đặc quyền.

- name: Add sysadmin accounts
  user:
    name: sysadmin
    groups: sudo
    append: yes

3. **Set Hostname**
Sử dụng Ansible hostname module để đặt hostname của máy chủ thành "base-server".

- name: Set hostname
  hostname:
    name: base-server

4. **Install Docker**
Sử dụng Ansible apt module để cài đặt Docker.

    - name: Install required system packages
      apt:
        pkg:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
          - python3-pip
          - virtualenv
          - python3-setuptools
        state: latest
        update_cache: true

    - name: Add Docker GPG apt Key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker Repository
      apt_repository:
        repo: deb https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Update apt and install docker-ce and docker-compose
      apt:
        pkg:
          - docker-ce
          - docker-compose
        state: latest
        update_cache: true

    - name: Install Docker Module for Python
      pip:
        name: docker

5. **Chỉ định trình điều khiển lưu trữ và ghi nhật ký**
Sử dụng Ansible copy module để tạo và điền nội dung vào tệp cấu hình Docker /etc/docker/daemon.json.

- name: Specify Docker logging and storage driver
  copy:
    content: |
      {
        "log-driver": "json-file",
        "storage-driver": "overlay2"
      }
    dest: /etc/docker/daemon.json

6. **Chuẩn bị/Điều chỉnh cho khối lượng công việc có lưu lượng mạng cao**
Sử dụng Ansible sysctl module để đặt các tham số hạt nhân cho công việc mạng có tải cao.

- name: Prepare/tune for high network traffic workload
  sysctl:
    name: "{{ item.name }}"
    value: "{{ item.value }}"
  loop:
    - { name: "net.core.somaxconn", value: "65535" }
    - { name: "net.ipv4.ip_forward", value: "1" }
(Ngoài ra, vẫn còn rất nhiều các thông số cấu hình mạng nữa)

8. **Kích hoạt tính năng ghi nhật ký lệnh của người dùng**
Sử dụng Ansible lineinfile module để thêm cấu hình cho việc ghi log mỗi lệnh thực thi vào /etc/bash.bashrc.

- name: Enable user command logging
  lineinfile:
    path: /etc/bash.bashrc
    line: 'export PROMPT_COMMAND="RETRN_VAL=$?;logger -p local6.debug \"$(whoami) [$$]: $(history 1 | sed \"s/^[ ]*[0-9]\+[ ]*//\" ) [$RETRN_VAL]\""'

10. **Restart Docker Service**
Sử dụng Ansible systemd module để khởi động lại dịch vụ Docker để áp dụng các thay đổi.

- name: Restart Docker service
  systemd:
    name: docker
    state: restarted
