# `TextDecoration.linethrough` 속성 버그

영어로 된 Text는 괜찮은데, 영어 외의 언어 (e.g. 한국어, 중국어)에 위의 프로퍼티를 적용하면 UI가 어색하게 나오는 버그가 있는거같다.
![image](images/linethrough_with_other_languages.jpeg)

고래서 일단 플러터에 [이슈](https://github.com/flutter/flutter/issues/41668) 제기해봄!
