# FontSize in TextStyle and StrutStyle

`TextStyle` 위젯의 `fontSize`랑 `StrutStyle`의 `fontSize`는 다르게 작동한다.
예컨대 `TextStyle#fontSize`를 동일하게 14로 적용한 두 개의 위젯이 있다고 했을 때, 한쪽에는 영어, 다른 한쪽에는 한글을 넣게 되면
캐릭터에 따라 위젯의 실제 높이는 다르게 측정될 수 있다.

한편, 같은 상황에서 `StrutStyle#fontSize`도 14로 적용해주면 어느 캐릭터가 들어가든 항상 14의 글자 크기의 높이로 고정된다.
위젯의 문단 사이즈를 14로 고정한다는 느낌일까나.

더 자세한건 요 [링크](https://medium.com/@najeira/control-text-height-using-strutstyle-4b9b5151668b)에
