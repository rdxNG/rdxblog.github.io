---
title: Microsoft Store에서 앱 설치시 로그인을 강제하는 경우
---

Microsoft Store의 앱들을 스토어 없이 설치하려면 매우 귀찮은 작업이 수반된다.

Windows를 로컬 계정으로 사용하는 경우, 갑자기 (보통 MS계정으로 로그인을 한 번 시도했다가 로그아웃한 경우) 스토어에서 앱 다운로드가 반드시 로그인을 요구할 때가 있다.

이 때 증상으로는 앱 설치 직전에 로그인을 물어볼 때 창을 닫으면 '장치 간에 사용' 다이얼로그가 뜨고 '관심 없음'버튼을 누르는 것으로 해결되나, 해당 다이얼로그가 뜨지 않고 로그인이 되지 않으면 진행이 되지 않는 상태에 이른다.

쓸 데 없는데에 친절하고 중요한 상황에 불친절한 Windows인 만큼 해당 문제의 해결방법을 여기 메모해둔다.
1. Microsoft Store를 닫는다.
2. 시작화면을 열고 `cmd`를 입력한 후 나온 '명령 프롬프트'를 우클릭하여 '관리자 권한으로 실행'
3. 검은 화면이 나오면 `whoami /user` 를 입력하고 엔터

```text
사용자 정보
----------------

사용자 이름         SID
=================== =============================================
XXXXXXXXXXXXX       S-1-5-XX-...........
```

1.  해당 화면의 SID밑에 있는 S-로 시작하는 전체를 복사한다. 해당 내용을 밑의 문장과 조합해야 한다.
2. `REG DELETE HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Appx\AppxAllUserStore\[[SID]]` [[SID]]를 위에 복사한 S-로 시작하는 내용 전체와 맞바꾼다.
3. 실행할 명령어가 `REG DELETE HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Appx\AppxAllUserStore\S-1-5-XX-.....` 형태가 되었는지 **반드시** 확인하고 엔터.
4. `WSReset.exe` 명령어를 붙여넣고 엔터
5. 스토어를 다시 실행하고 '장치 간의 사용' 다이얼로그가 나오는 것을 확인하고 '관심 없음'버튼을 누르면 로그인 없이 정상적으로 앱의 다운로드및 설치가 진행된다.
