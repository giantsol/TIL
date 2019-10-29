# BuildContext의 debugNeedLayout

아하.. 안드에서는 View가 dirty한 상태이냐를 보기 위해서 hasLayoutBeenRequested인가? 그런 함수를 쓸 수 있는데
플러터에서도 비슷한 기능이 필요해서 막 그냥 찾다보니까 debugNeedLayout이 있어서 맹목적으로 쓰고 있었다.

근데 다큐먼트를 잘 읽어보니 release build에서는 무조건 false라고... 말 그대로 "debug" 모드에서만 동작하는 플래그인가부다.

그럼 다른 방법이 필요할텐데... 알게되면 여따 추가하는걸루