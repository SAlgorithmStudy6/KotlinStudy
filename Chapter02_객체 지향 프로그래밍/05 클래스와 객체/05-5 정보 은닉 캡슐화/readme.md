# 05-5 정보 은닉 캠슐화

- 캡슐화 : 클래스를 작성할 때 숨겨야 하는 속성이나 기능을 숨겨서 구현(예 : 자동차의 핸들과 조향장치)
- 가시성 지시자 : 접근 범위를 지시자를 통해 명시해줌
  | 지정자 | 범위 |
  | --- | --- |
  | private | 해당 클래스 내부 |
  | protected | 해당 클래스 + 서브 클래스 내부 |
  | internal | 같은 모듈 내부 |
  | public | 어디서든지 가능, 기본 지정자로서 생략 시 자동으로 지정 됨 |

- iternal
  - 프로젝트 단위의 모듈을 가리치켜 모듈이 달라지면 접근 제한
  - java에서는 package의 이름이 같은 경우 접근을 허용했다면, kotlin에서는 패키지에 제한하지 않고 하나의 모듈 단위로 제한 가능
  - kotlin에서는 package지시자를 사용하지 않음(package가 없는 게 아님!)
  - 자바에서 페키지 단위로 관리했던 가시성의 경우 보안 문제가 발생할 수 있는데 이를 보완하기 위해 모듈 개념을 도입
  - [코틀린 프로젝트 범위 설명 링크](https://acaroom.net/ko/blog/youngdeok/%EC%97%B0%EC%9E%AC-%EC%BD%94%ED%8B%80%EB%A6%B0-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%BD%94%ED%8B%80%EB%A6%B0-%ED%8C%A8%ED%82%A4%EC%A7%80)
  
- 가시성 지시자와 클래스의 관계
  - 가시성 지시자를 활용하여 상속된 하위 클래스에서 접근 범위를 지정하거나 다른 클래스와의 연관 관계를 지정할 수 있음
  - UML의 가시성 표기 기호
    - private : -
    - public : +
    - protected : #
    - package : ~ (코틀린의 internal의 표기법이 아직 지정되지 않았음)
 
      <img width="409" alt="화면 캡처 2022-09-17 171452" src="https://user-images.githubusercontent.com/43957736/190847359-87ca07a3-9b24-49ad-b9bb-88cbacf6ded7.png">
    
