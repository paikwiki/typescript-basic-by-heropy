# Heropy 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강의(@Goorm Edu)

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강의(@Goorm Edu)를 정리한 노트입니다. 강사의 블로그에 [이 강의 전체를 정리해둔 포스팅](https://heropy.blog/2020/01/27/typescript/)이 있습니다. 이 포스트에는 일부 업데이트된 내용이 있습니다. 필요한 경우, Heropy 님의 블로그 포스트에 추가된 내용도 함께 학습하며 요약 노트에 적었습니다.

## 안내

- 학습한 날짜를 아래의 표에 정리합니다.
- 본 노트는 강의의 내용 중, 개인적으로 필요한 내용을 정리해두었습니다.
- 따라서, 강의 내용 중 생략된 부분이 있을 수 있습니다.
- 강사의 블로그 포스트, "[한 눈에 보는 타입스크립트](https://heropy.blog/2020/01/27/typescript/)"에는 본 강의의 최신 내용이 담겨 있습니다. 이 포스트로 타입스크립트의 기초문법을 익히는 걸 추천드립니다.

## 강의 추천

간결하게 잘 구성되어 공식 문서 읽는 것보다 부담이 적습니다. 블로그의 글이 최신이기는 하지만, 구름에듀에서 학습하는 것도 진도율 체크를 할 수 있어 학습 의욕을 높이는 좋은 방법이라고 생각합니다. 평균 별점이 5.0인 데에는 다 이유가 있구나 싶은 강의였습니다.

나의 수강평:

> 타입스크립트를 시작하면서 전반적인 문법을 훑어보고 싶어 자료를 찾다가 이 강의를 발견했습니다. 강사님(Heropy) 블로그에 이 콘텐츠의 최신 업데이트 버전이 있는 것을 발견했으나, 챕터별로 학습하며 진도율을 확인할 수 있는 구름에듀에서 학습을 완주했습니다. 업데이트된 부분은 복습삼아 강사님 포스트를 쭉 읽어보며 정리하려 합니다. 군더더기 없이 필요한 내용을 잘 요약해주셨고, 특히 사용방법에 대한 예시에 매번 오류가 나는 케이스까지 정리해주셔서 내용을 이해하는데 많은 도움이 됐습니다. 종종 다시 수강하면서 복습해야겠네요. 좋은 강의 제공해주셔서 고맙습니다.

## 동영상 강의 링크와 요약 노트

이 강의는 텍스트로 구성된 강의로, 각 챕터마다 강의 텍스트 문서의 수가 다릅니다. 강의 링크는 각 챕터의 첫 번째 강의 텍스트 문서의 링크이며, 제목 뒤에 "(1/{전체 문서 수})" 형태로 각 챕터별 텍스트 문서의 개수를 표기했습니다.

| 연번 | 강의 링크                          | 노트        | 학습일        |
| -: | --------------------------------- | ----------- | ----------- |
| 01 | [Intro](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1040385/history)(1/1) | - | 2021.07.19. |
| 02 | [타입스크립트 개요](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1040388/%EC%99%9C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%B8%EA%B0%80)(1/3) | - | 2021.07.19. |
| 03 | [개발환경](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1040389/vscode%EC%99%80-webstorm)(1/6) | - | 2021.07.19. |
| 04 | [타입 기본](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1040412/%ED%83%80%EC%9E%85-%EC%A7%80%EC%A0%95)(Types)(1/9) | [:memo: note](./notes/chapter04.md) | 2021.07.20. |
| 05 | [인터페이스](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042565/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%9E%80)(Interface)(1/6) | [:memo: note](./notes/chapter05.md) | 2021.07.21. |
| 06 | [타입 별칭](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042571/%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)(Type Aliases)(1/1) | [:memo: note](./notes/chapter06.md) | 2021.07.21. |
| 07 | [제네릭 (Generic)](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042572/%EC%A0%9C%EB%84%A4%EB%A6%AD%EC%9D%B4%EB%9E%80)(1/3) | [:memo: note](./notes/chapter07.md) | 2021.07.22. |
| 08 | [함수](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042575/this)(1/2) | [:memo: note](./notes/chapter08.md) | 2021.07.22. |
| 09 | [클래스](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042577/%ED%81%B4%EB%9E%98%EC%8A%A4%EB%9E%80)(1/3) | [:memo: note](./notes/chapter09.md) | 2021.07.22. |
| 10 | [Optional](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042580/%EB%A7%A4%EA%B0%9C-%EB%B3%80%EC%88%98-parameters)(1/4) | [:memo: note](./notes/chapter10.md) | 2021.07.22. |
| 11 | [모듈](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042584/%EB%82%B4%EB%B3%B4%EB%82%B4%EA%B8%B0-export-%EC%99%80-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0-import)(1/3) | [:memo: note](./notes/chapter11.md) | 2021.07.22. |
| 12 | [TS 유틸리티 타입](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042587/ts-%EC%9C%A0%ED%8B%B8%EB%A6%AC%ED%8B%B0-%ED%83%80%EC%9E%85%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)(1/17) | [:memo: note](./notes/chapter12.md) | 2021.07.23. |
| 13 | [참고 자료 (References)](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/lesson/1042604/%EC%B0%B8%EA%B3%A0-%EC%9E%90%EB%A3%8C)(1/1) | - | 2021.07.23. |

## 저작권 안내

이 저장소는 학습 목적으로 생성한 저장소입니다. 모든 예제 코드와 내용에 대한 저작권은 [원저자(Heropy)](https://heropy.blog/)에게 있습니다.
