---
title: Google_Cloud_Platform
date: 2024-07-02 13:53:56
tags: Linux Game
categories:
    - Linux
    - Game
---
# 구글 클라우드 플랫폼

![](/image/xhmb4das.png)
![](/image/g18.png)

## 팰월드 서버 만들기


① 구글 클라우드(Google Cloud) 회원 가입 및 무료 이용 방법

1. 구글 클라우드(cloud.google.com) 홈페이지에 접속합니다.
2. "무료로 시작하기" 버튼을 클릭하고, 회원가입을 진행합니다.

회원가입 시 입력해야 하는 고객 정보 및 결제 수단 정보는 자동결제에 이용되지 않으며, 안내해 드린 구글 클라우드를 90일간 300달러로 이용할 수 있는 크레딧이 종료되더라도 요금이 청구되지 않습니다.

![](/image/g1.png)

3. 회원가입 후 로그인하면, 90일 동안 사용할 수 있는 300달러(원화로 표시) 크레딧이 충전되고, 만료일이 표시됩니다.

![](/image/g2.png)

② 구글 클라우드 플랫폼 VM 인스턴스 설정
1. 구글 클라우드 화면 좌측 상단에 "≡(탐색 메뉴)" 버튼을 클릭합니다.
2. "Compute Engine" 메뉴를 클릭하고, '가상 머신' 카테고리에 있는 "VM 인스턴트" 메뉴를 클릭합니다.

![](/image/g3.png)

3. 상단에 있는 "인스턴스 만들기" 버튼을 클릭합니다.

![](/image/g4.png)

4. '이름'에 팰월드 서버 이름을 정해서 입력합니다.
5. '리전'에는 "asia-northeast3(서울)" 항목을 선택하고, '영역' 값은 "asia-northeast3-a"을 선택합니다.

![](/image/g5.png)

6. 머신 구성 설정에서 가장 무난한 "N2" Series를 선택합니다.

CPU를 높게 잡고 싶은 경우에는 '컴퓨팅 최적화' 탭에서 "C2"를 선택하면, CPU Hz가 높은 옵션을 선택할 수 있습니다. 다만, 성능이 높아지는 만큼 월별 예상 가격도 함께 인상됩니다.
멀티플레이 인원수가 5~10명 정도라면, "N2" 또는 "N2D" 시리즈를 선택합니다.

![](/image/g6.png)

7. 화면을 아래로 내린 다음 '머신 유형' 설정에서 "n2d-standard-4(vCPU 4개, 코어 2개, 메모리 16GB)" 옵션을 선택합니다.

![](/image/g7.png)

8. 화면을 더 내린 다음 '부팅 디스크' 항목에서 "변경" 버튼을 클릭합니다.

![](/image/g8.png)

9. 운영체제 "Ubuntu", 버전 "Ubuntu 22.04 LTS", 부팅 디스크 유형 "균형 있는 영구 디스크", 크기(GB)는 "15"를 입력하고, "선택" 버튼을 클릭합니다.

![](/image/g9.png)

10. 화면 오른쪽에서 '월별 예상 가격'을 확인하고, 크레딧 300달러를 고려해서 약 2개월 이상 동안 팰월드 서버를 구글 클라우드 서비스를 이용해서 만들 수 있는 것을 확인할 수 있습니다.
11. 모든 내용을 입력하고, 예상 가격까지 확인한 다음 "만들기" 버튼을 클릭합니다.

③ 클라우드 방화벽 개방 설정

1. 구글 클라우드 좌측 상단에 "≡" 버튼을 클릭합니다.
2. "VPC 네트워크" > "VPC 네트워크" 메뉴를 차례대로 클릭합니다.
3. VPC 네트워크 화면에서 '이름' 항목에 "default"를 클릭합니다.

![](/image/g10.png)

4. 'VPC 네트워크 세부정보' 페이지에서 상단에 "방화벽" 메뉴를 클릭합니다.
5. "방화벽 규칙 추가" 메뉴 버튼을 클릭합니다.

![](/image/g11.png)

6. '방화벽 규칙 만들기' 페이지에서 '이름' 항목에 구분하기 쉽도록 서버 이름과 동일하게 입력합니다.

![](/image/g12.png)

8. 화면을 아래로 내린 다음 '대상' 항목을 "네트워크의 모든 인스턴스"로 변경 설정합니다.
9. '소스 IPv4 범위' 옵션에 모든 방화벽에 적용할 수 있는 "0.0.0.0/0"을 입력합니다.

![](/image/g13.png)

10. '프로토콜 및 포트' 카테고리에서 'TCP, UDP" 항목을 박스 체크합니다.
11. TCP 포트에 "27015, 27016, 25575", UDP 포트에 "27015, 27016, 25575, 8211"을 입력합니다.

![](/image/g14.png)

12. 화면 맨 아래 있는 "만들기" 버튼을 클릭합니다.

④ VM 접속 및 방화벽 개방 설정

1. 홈페이지 화면 좌측 상단에 ≡ 버튼 클릭 > "Compute Engine" > "VM 인스턴스" 메뉴로 이동합니다.
2. 앞서 설정한 VM 인스턴스 설정에 입력한 VM의 상태가 초록색 체크표시로 변한 것을 확인합니다.
3. '외부 IP' 항목에 있는 공인 IP주소는 서버에 접근하고, 게임에 접속하기 위한 IP로 사용되니 메모 또는 기억해 둡니다.
4. '연결' 탭에 있는 "SSH"를 클릭합니다.

![](/image/g15.png)

5. 승인 창이 생성되면, "Authorize" 버튼을 클릭합니다.
6. '브라우저에서 SSH를 통해 연결' 창이 생성되면, 아래 10가지 명령어를 차례대로 입력하고 엔터키를 눌러서 실행합니다.

```
sudo apt update
sudo apt install netfilter-persistent
sudo iptables -I INPUT -p tcp --dport 27015 -j ACCEPT
sudo iptables -I INPUT -p tcp --dport 27016 -j ACCEPT
sudo iptables -I INPUT -p tcp --dport 25575 -j ACCEPT
sudo iptables -I INPUT -p udp --dport 27015 -j ACCEPT
sudo iptables -I INPUT -p udp --dport 27016 -j ACCEPT
sudo iptables -I INPUT -p udp --dport 25575 -j ACCEPT
sudo iptables -I INPUT -p udp --dport 8211 -j ACCEPT
sudo iptables -S
```

7. 명령어를 모두 입력한 다음 방화벽 개방이 완료되었는지 아래 명령어(대소문자 구분)를 입력하고 확인합니다.

```
sudo iptables -nL
```

![](/image/g16.png)

⑤ SteamCMD 설치 및 게임엔진 설치 방법
1. Steam CMD 설치 명령어 2개를 차례대로 입력 및 실행합니다.

```
sudo add-apt-repository multiverse; sudo dpkg --add-architecture i386; sudo apt update
# 'Press [Enter] to continue or Ctrl-c to cancel'에서 엔터키를 눌러줍니다.
sudo apt install steamcmd
# 'Do you want to continue?'에서 키보드 "y" 키를 눌러줍니다.
```

2. 설치 중간에 'Configuring steamcmd' 창이 생성되면, 키보드 'Tap' 키를 눌러서 "OK" 버튼을 눌러줍니다.
3. 'STEAM LICENSE AGREEMENT' 창에서는 "I AGREE"를 선택하고, 'Tap' 키를 눌러서 "OK" 버튼을 눌러줍니다.

4. 스팀 CMD 설치를 완료한 다음 게임엔진을 설치하기 위해 아래 명령어를 입력합니다.

```
steamcmd +login anonymous +app_update 2394010 validate +quit
```

5. GameEngine 설치 완료 후 SDK64를 설치하기 위해 다음 명령어를 차례대로 입력합니다.

```
mkdir -p ~/.steam/sdk64/
steamcmd +login anonymous +app_update 1007 +quit
cp ~/Steam/steamapps/common/Steamworks\ SDK\ Redist/linux64/steamclient.so ~/.steam/sdk64/
```

⑥ 서버 실행 및 서버 환경 설정 파일 수정 및 커스터마이징
1. 설치를 모두 완료한 다음 아래 2개 명령어를 입력해서 파일 생성을 위해 이니셜을 최초 1회만 실행합니다.

```
cd ~/Steam/steamapps/common/PalServer
./PalServer.sh
```

2. 서버를 실행한 다음 키보드 "Ctrl + C" 키를 동시 입력해서 서버를 내려줍니다.

3. 서버 설정 파일을 세부적으로 커스터마이징 및 수정할 수 있도록 아래 명령어를 실행합니다.

```
nano 
~/Steam/steamapps/common/PalServer/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
```

4. 아래 스크립트 내용을 복사한 다음 내용을 수정하고, 다시 수정한 내용을 복사합니다.

```
[/Script/Pal.PalGameWorldSettings]
OptionSettings=(Difficulty=None,DayTimeSpeedRate=1.000000,NightTimeSpeedRate=1.000000,ExpRate=1.000000,PalCaptureRate=1.000000,PalSpawnNumRate=1.000000,PalDamageRateAttack=1.000000,PalDamageRateDefense=1.000000,PlayerDamageRateAttack=1.000000,PlayerDamageRateDefense=1.000000,PlayerStomachDecreaceRate=1.000000,PlayerStaminaDecreaceRate=1.000000,PlayerAutoHPRegeneRate=1.000000,PlayerAutoHpRegeneRateInSleep=1.000000,PalStomachDecreaceRate=1.000000,PalStaminaDecreaceRate=1.000000,PalAutoHPRegeneRate=1.000000,PalAutoHpRegeneRateInSleep=1.000000,BuildObjectDamageRate=1.000000,BuildObjectDeteriorationDamageRate=1.000000,CollectionDropRate=1.000000,CollectionObjectHpRate=1.000000,CollectionObjectRespawnSpeedRate=1.000000,EnemyDropItemRate=1.000000,DeathPenalty=All,bEnablePlayerToPlayerDamage=False,bEnableFriendlyFire=False,bEnableInvaderEnemy=True,bActiveUNKO=False,bEnableAimAssistPad=True,bEnableAimAssistKeyboard=False,DropItemMaxNum=3000,DropItemMaxNum_UNKO=100,BaseCampMaxNum=128,BaseCampWorkerMaxNum=15,DropItemAliveMaxHours=1.000000,bAutoResetGuildNoOnlinePlayers=False,AutoResetGuildTimeNoOnlinePlayers=72.000000,GuildPlayerMaxNum=20,PalEggDefaultHatchingTime=72.000000,WorkSpeedRate=1.000000,bIsMultiplay=False,bIsPvP=False,bCanPickupOtherGuildDeathPenaltyDrop=False,bEnableNonLoginPenalty=True,bEnableFastTravel=True,bIsStartLocationSelectByMap=True,bExistPlayerAfterLogout=False,bEnableDefenseOtherGuildPlayer=False,CoopPlayerMaxNum=4,ServerPlayerMaxNum=32,ServerName="Default Palworld Server",ServerDescription="",AdminPassword="12341234",ServerPassword="",PublicPort=8211,PublicIP="",RCONEnabled=False,RCONPort=25575,Region="",bUseAuth=True,BanListURL="https://api.palworldgame.com/api/banlist.txt")
```

파일 내용을 수정할 때는 팰월드 테크 가이드 페이지의 내용을 참고해서 커스터마이징하시기 바랍니다.

https://tech.palworldgame.com/settings-and-operation/configuration/

5. 복사한 내용을 편집기로 돌아간 다음 검은 화면 위에 마우스 오른쪽 버튼을 클릭하고, "붙여 넣기"를 클릭합니다.
6. 모든 작업을 완료한 다음 키보드 "Ctrl + X" 키를 입력해서 종료합니다.
7. 'Save modifield buffer?' 창에서 키보드 "Y"키를 입력하고, 엔터키를 눌러줍니다.

⑦ 서버 구동 방법
1. '브라우저에서 SSH를 통해 연결' 창에서 게임을 구동하기 전에 스크린 유틸리티를 구동하기 위해 아래 명령어를 입력합니다.(터미널 창을 닫더라도 서버는 계속 운영되도록 Screen Utility를 실행합니다.)

```
screen
```

2. 서버 구동 및 게임을 실행할 수 있는 명령어를 입력 및 실행합니다.

```
cd ~/Steam/steamapps/common/PalServer
```

3. 개발사에서 안내하는 성능 최적화를 위한 멀티스레딩 옵션을 추가할 수 있는 명령어를 입력 및 실행합니다.

```
./PalServer.sh -useperfthreads -NoAsyncLoadingThread -UseMultithreadForDS
```

3. 실행되어 있는 터미널을 백그라운드로 돌리고, 종료하기 위해 키보드 "Ctrl(컨트롤)" 키를 누른 상태로, 키보드 A(ㅁ) 키와 D(ㅇ) 키를 차례대로 눌러줍니다.

모든 설정을 완료하고, 터미널 창(브라우저에서 SSH를 통해 연결)을 닫더라도 서버는 24시간 계속 운영됩니다.

이상으로 구글 클라우드 서비스를 이용해서 팰월드 서버 만들기를 완료했습니다. 구글 클라우드 VM 인스턴스 설정 페이지에서 "외부 IP" 탭에 적혀있는 IP 주소를 내가 만든 팰월드 서버에서 함께 멀티플레이할 친구, 지인, 동료들에게 공유해서 접속하도록 합니다.

구글 클라우드를 이용한 팰월드 서버 접속 방법
마지막으로 구글 클라우드를 이용해서 만든 팰월드 서버에 접속하는 방법을 설명해 드리겠습니다. 구글 클라우드 홈페이지 VM 인스턴스 설정 페이지에서 "외부 IP" 탭에 적혀있는 공인 IP 주소를 사용해서 접속할 수 있습니다. 또한 공인 IP 주소를 직접 만든 팰월드 서버에서 함께 멀티플레이할 친구, 지인, 동료들에게 공유하고, 접속하도록 할 수 있습니다. 서버 접속 방법은 다음과 같습니다.

1. 팰월드(PALWORLD) 게임을 실행합니다.
2. 팰월드 첫 화면에서 "멀티 플레이 참가하기(전용 서버)" 메뉴를 클릭합니다.
3. "커뮤니티 서버" 탭을 클릭한 다음 하단에 "연결" 버튼 왼쪽에 "공인 IP 주소:8211"를 입력하고, "연결" 버튼을 클릭합니다.

- 공인 IP 주소 입력 방법 : xx.xx.xx.xxx:8211

연결 버튼을 클릭하면, 구글 클라우드 서비스를 이용해서 만든 팰월드 전용 서버에 정상적으로 접속되는 것을 확인할 수 있습니다.

이상으로 구글 클라우드 서비스를 이용해서 팰월드 서버 만들기 방법에 대해 모두 설명해 드렸습니다. 게임 화면에서 키보드 "Esc" 키를 입력하고, "옵션" 탭에서 서버 인원수 등의 서버 정보를 확인할 수 있습니다.

![](/image/g17.png)

출처 : https://copyking.tistory.com/entry/%ED%8C%B0%EC%9B%94%EB%93%9C-PalWorld-%EC%84%9C%EB%B2%84-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EA%B5%AC%EA%B8%80-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C