VIRTUALIZATION WITH KVM.
====================================
KVM là viết tắt của Kernel-based Virtual Machine, là một công nghệ ảo hóa, cho
phép chúng ta có thể chạy nhiều hệ điều hành cùng lúc và trên cùng một máy vật
lý bằng cách tạo ra các virtual machines. Bạn có thể đồng thời truy cập tới chúng.

1. Các cách thức cài đặt KVM.
------------------------------------------
Theo tớ có 3 cách cài đặt một VM chính:
- Bằng đồ họa.
- Bằng dòng lệnh.
- Clone từ một cái có sẵn.

Bài viết này hướng dẫn bạn tạo VM theo cách thứ 2, nghĩa là sử dụng các command lines
và thực hiện trên ubuntu 12.04.

2. Cài đặt các package cần thiết.
--------------------------------------
```
sudo apt-get install qemu-kvm libvirt-bin bridge-utils virt-manager -y
```
3. Create a KVM Guest.
-------------------------
Để create một KVM Guest, bạn có thể sử dụng ```virt-install``` command line tool.

Bạn cần cung cấp các thông số kỹ thuật chính khi create một VM. Các thông số còn lại
của VM sẽ được apply theo default.

Thông tin về các config values bạn có thể tham khảo trong:
```
man kvm
```
Ví dụ về một command lines sẽ như sau:
```
# virt-install \
       --name testkvm \
       --ram 500 \
       --disk path=/mnt/virtual_machines/testkvm.img,size=5 \
       --network network:default \
       --accelerate \
       --vnc \
       --c /mnt/virtual_machines/ubuntu-14.04.1-server-amd64.iso
```

Sau đó quá trình install VM sẽ bắt đầu, bạn sẽ thao tác trên giao diện của VM.

Mỗi lần một VM mới được tạo, sẽ có một file XML được tạo ra trong thư mục ```/etc/libvirt/qemu/```
Đây là file định nghĩa VM của bạn với tên VMname.xml
```
thanhnt@Thanhnt:~/github/learning$ ls /etc/libvirt/qemu/
networks  testkvm.xml  VM-thanhnguyen.xml  VM-thanhnt.xml
```
4. Một số command lines thường sử dụng để quản lý VM.
----------------------------------------
**Kiểm tra các máy VM.**
```
virsh list --all
```
**Khởi chạy.**
```
virsh start VM_name
```
**Delete VM.**
```
virsh destroy guest_name
virsh undefine guest_name.**
```
**Truy cập vào máy ảo qua terminal.**
```
virt-viewer ten_may
```

Link tham khảo:
----------------
http://www-01.ibm.com/support/knowledgecenter/linuxonibm/liaai.kvminstall/liaaikvminstallinstall.htm

