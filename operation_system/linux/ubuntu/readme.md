# NTFS disk(Windows용 format)를 mount하여 사용하기
```bash
# Install package: ntfs-3g
sudo apt-get update
sudo apt-get install ntfs-3g

# Create mount point
sudo mkdir /media/windows

# Check disk whether it is formatted NTFS file system
sudo fdisk -l

# 기본적으로, 해당 Package를 설치 시, 자동을 /media 하위에서 mount된 disk를 확인 가능하다

# Link to mount point
sudo ntfs-3g /dev/sda1{or other path} /media/windows

# Unmount
sudo umount /media/windows
```

# Disk mount 하기
```bash
# Check volume
sudo fdisk -l

# Create file system
sudo mkfs.ext4 /dev/sdb

# Create directory received mount
sudo mkdir /{disk_path} 

# Mount
sudo mount /dev/sdb /{disk_path}

# Check mount
df -h

# Unmount
sudo umount /{disk_path}

# Mount(Auto) - 추가 및 포멧 후 자동 마운트
sudo blkid
```


# 포트 확인, 열기 및 방화벽 확인
```bash
# 열려있는 모든 포트 표시
netstat -nap
---------------------------------
-n: host명으로 표시 안함
-a: 모든소켓 표시
-p: 프로세스ID와 프로그램명 표시 
---------------------------------

# LISTEN 중인 포트 표시
netstat -nap | grep LISTEN

# 확인하려는 포트번호 상태확인

netstat -nap | grep 포트번호

```

# tar 압축
```bash
# tar(tar.gz)로 압축하기
tar -(z)cvf [파일명.tar(.gz)] [폴더명]

# ex) abc라는 폴더를 aaa.tar로 압축하고자 한다면
tar -(z)cvf aaa.tar(.gz) abc

# tar(tar.gz)로 압축풀기
tar -(z)xvf [파일명.tar(.gz)]

# ex) aaa.tar라는 tar파일 압축을 풀고자 한다면
tar -(z)xvf aaa.tar(.gz)
```

# ACL permission이란?
- linux에서 permission의 기존 3비트 * 3비트 * 3비트(ex : 775) 권한에 추가한 확장 권한
- getfacl {directory} 로 full permission을 확인 가능
- https://dgblog.tistory.com/156

# ACL permission 지우기(권한 맨 뒤에 +)
```bash
setfacl -bn {directory}
```
# 참조 링크
[Service 등록](https://chhanz.github.io/linux/2019/01/18/linux-how-to-create-custom-systemd-service/)
[Access Control List](https://dgblog.tistory.com/156)