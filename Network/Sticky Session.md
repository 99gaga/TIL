# Stiky Session

---

Http는 무상태를 지향하기 때문에 세션을 가지고 사용자의 정보를 저장하고 이용하기도 한다. 하지만, 서버가 여러대일 경우 각 서버마다 각자의 세션정보를 다르게 가지고 있기 때문에 **세션 불일치** 문제가 일어난다. 만약 서버에 로그인을 하고 잠시 후 다른 요청을 보낸다면 이전 로그인을 처리한 서버와 다른 서버에 요청이 가게 될 경우 그 서버에는 세션에 로그인 정보가 없기 때문에 다시 로그인하라는 메시지를 받을 것이다.<br>

이러한 **세션 불일치**의 해결 방법중 하나로 `Sticky Session`이 있다.

### Sticky Session

- L4 Sticky Session<br>
  source ip 주소를 사용하여 세션을 유지한다. 동일한 IP 주소의 요청은 동일한 서버로 라우팅이 된다.<br>
  하지만 클라이언트의 source ip 주소가 변경되거나 세션 기간이 초과할경우 동일한 문제가 발생한다.<br>
  그런데 하나의 공인 IP를 사용하는 곳에서 여러개의 사설 IP의 요청이 온다면 서버 입장에서는 하나의 IP로 인식하여 모두 같은 서버로 몰리게되는 문제점도 있다.

- L7 Sticky Session<br>
  로드 밸런서 쿠키 또는 애플리케이션 쿠리를 사용하여 세션을 유지한다.<br>

1. 로드 밸런서 쿠키 : 로드 밸런서는 클라이언트로부터 요청을 받은 후 쿠키를 생성한다. 쿠키가 있는 모든 후속 요청은 동일한 서버로 라우팅 된다.<br>
2. 애플리케이션 쿠키: 백엔드 서버에 배포된 애플리케이션은 클라이언트로 부터 첫 번째 요청을 받은 후 쿠키를 생성한다. 쿠키가 있는 모든 요청은 동일한 백엔드 서버로 라우팅 된다.<br>
   <br>
   하지만 클라이언트의 요청에 쿠키가 포함되어 있지 않거나 세션 기간을 초과하면 문제가 발생한다.<br>
