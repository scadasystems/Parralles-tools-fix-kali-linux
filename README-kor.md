# Parallels Tools fix for Kali linux 2019.3 and previous version
* Kali linux 뿐만 아니라 거의 모든 운영체제에 적용할 수 있습니다.
* Parallels Tools 설치에서 에러가 난다면 이 방법을 사용해보세요. 
* 직접 해본 결과 BackBox, Kali linux, AnonymousOS, Ubuntu, Debian 등
* 만약 에러가 난다면 issues을 남겨주세요.

## Parallels tools 설치
우선 칼리 리눅스 환경을 최신버전으로 업데이트 합니다. 다음 명령을 하나씩 복사 또는 타이핑하여 실행하세요.
``` bash
apt-get clean
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
```
그리고 Reboot을 한 후, parallels tools 를 설치합니다.

## Parallels tools 패치
1. `/root/Desktop/` 에 parallels_fixed 라는 폴더를 생성합니다.
2. tool 을 설치했다면 cdrom에 파일들이 있습니다. 이 파일들을 parallels_fixed 폴더 안에 복사합니다.
3. kmods 폴더로 들어간 뒤 (`cd ~/parallels_fixed/kmods`)
4. prl_mod.tar.gz 을 압축 풀고 (`tar -xzf prl_mod.tar.gz`), prl_mod.tar.gz 를 삭제합니다. (`rm prl_mod.tar.gz`)
5. prl_fs 폴더로 들어갑니다 : `cd /root/Desktop/parallels_fixed/kmods/prl_fs/SharedFolders/Guest/Linux/prl_fs`
6. prlfs.h 파일을 열고 16번째 라인에 다음 코드를 추가합니다 : `#include <uapi/linux/mount.h>`
    파일은 이렇게 보여야 합니다. 추가 했다면 저장하고 나옵니다.

    ``` c
    ...
    #include <linux/fs.h>
    #include <uapi/linux/mount.h>   
    #include <linux/types.h>
    ...
    ``` 

1. kmods 폴더로 간 뒤 (`cd ~/parallels_fixed/kmods`) prl_mod.tar.gz 압축파일을 생성합니다 : `tar -zcvf prl_mod.tar.gz . dkms.conf Makefile.kmods`
2. installer 폴더로 이동하고 : `cd ~/parallels_fixed/installer`
3. install-cli.sh 및 기타 파일들에 대한 권한을 변경합니다 :  `sudo chmod 777 *.sh`
4. 마지막으로 해당 파일을 다음과 같이 실행합니다 : `sudo ./install-cli.sh -i --verbose`
5. 완료되면 재부팅을 하고 잘되었는지 확인합니다.

# End