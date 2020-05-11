# day 1. login

## XingAPI 사용 방식, XASession
xing api는 크게 세개의 클래스로 구성되어 있다.
~~~
XASession	서버연결, 로그인	XA_Session.dll
XAQuery	조회TR	XA_DataSet.dll
XAReal	실시간TR	XA_DataSet.dll
~~~
  
먼저 XASession을 사용해서 로그인을 해보자.  
사용방법은 간단하다.  
win32com패키지의 DispatchWithEvents매서드를 통해 XASession클래스 인스턴스를 생성하고  
클래스의 메서드들을 호출하는 방식이다.  
  
XA_Session 라이브러리(dll)의 XASession클래스 인스턴스를 COM을 이용해서 생성하고,  
이때 XASessionEvents클래스를 전달하는데, login이 완료되면 XASessionEvents클래스의 OnLogin콜백이 불리게 된다.  
생성된 인스턴스의 메서드를 호출해서 Server에 연결, Login을 한다.  
Login()메서드를 호출하면 서버에서 처리를 완료 후에 OnLogin()함수를 불러준다.  
(비동기 방식을 사용하고 있기 때문에 함수에 대한 결과를 바로 return받는 것이 아니라  
서버가 처리를 완료하면 미리 등록해둔 콜백함수가 불리는 방식이다.)  
~~~python
# 이벤트 처리용 클래스
class XASessionEvents:
    def OnLogin(self, code, msg):
        if code == "0000":
            print("로그인 성공")
        else:
            print("로그인 실패 ", msg)

id = "ID"
passwd = "PASSWORD"
cert = "CERT"

session = win32com.client.DispatchWithEvents("XA_Session.XASession", XASessionEvents)
session.ConnectServer("hts.ebestsec.co.kr", 20001)
session.Login(id, passwd, cert, 0, False)
~~~
  
## 패키지 설치
~~~sh
# com을 사용하기위해 필요하다.
pip install pypiwin32
# virtualenv 설치
pip install virtualenv
~~~
  
## xing api 사용시 발생하는 에러
COM을 사용해서 explorer 같은 어플리케이션이 열리는 것을 확인했었고  
COM 버전의 xing api를 사용할 때만 아래 url과 동일한 에러가 발생했다.  
https://sogentle.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-win32com-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0  
아나콘다 32비트를 사용하니 해결됬다고 한다. 아나콘다 홈페이지를 가보니 아나콘다 32비트는 python 버전을 3.7.2를 사용하고 있다.  
기존에 설치했던 python 3.8.2를 삭제하고 python 3.7.2 32비트 버전을 다시 설치하고 실행해보니 정상 동작한다.  
원인은 정확히 모르지만, COM버전의 xing api를 사용하기 위해서는 python 3.7버전 대의 32비트를 사용해야하는 것으로 보인다.  
  
## pyQt설치
~~~sh
pip install pyqt5
~~~
  
## 참고
  
COM 개념, XingAPI 사용 방법  
https://wikidocs.net/4126  
  
xing api 사용시 발생하는 에러  
https://sogentle.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-win32com-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0  
  
예제 코드 참고  
https://wikidocs.net/71790  
  
  
# day 2. 계좌 정보 가져오기, DevCenter 이용하기
  
## 참고
  
https://wikidocs.net/71794  
  
https://wikidocs.net/4129  
  
