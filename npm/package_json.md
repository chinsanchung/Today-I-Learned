# package.json 파일
[출처 : 모두 알지만 모두 모르는 package.json](http://programmingsummaries.tistory.com/385)

## name
- package.json 에서 가장 중요한 항목은 `name`, `version` 입니다.
  + 그것으로 각 패키지의 고유성을 판별합니다.
  + 패키지의 내용을 바꾸려면 version 을 바꿔야 합니다.
- 규칙
  + name 은 반드시 214자보다 짧아야 합니다. (scoped package 의 scope 포함해서 214자)
  + name 은 . 이나 _ 로 시작하지 못합니다.
  + name 은 대문자를 포함해선 안됩니다.
  + name 은 URL 의 일부이자 커맨드라인의 인수이고 폴더 이름입니다. 따라서 모든 URL-safe 가 아니면 안됩니다.
- 조언
  + Node 의 코어 모듈과 같은 이름을 쓰지 않습니다.
  + name 에 `node` 또는 `js` 를 넣지 않습니다.
  + name 은 자바스크립트 파일 내에서 require() 함수의 인수로 사용되기에 짧고 쉬운 이름으로 지어야 합니다.
  + 설정하기 전에 같은 이름이 있는지 `npm registry` 를 확인합니다.
## version
- version 은 반드시 npm 의 디펜던시에 포함된 node-semver 로 parsing 이 가능해야 합니다.
  + version number 와 범위(range) 는 [semver](https://docs.npmjs.com/misc/semver) 를 참고합니다.
## description
- 설명은 문자열로 작성합니다. `npm search` 로 검색할 수 있어서 도움을 줍니다.
## keywords
- 키워드는 문자열 배열로 설명합니다. `npm search` 로 검색할 수 있습니다.
## hompage
- 프로젝트의 홈페이지가 있다면 작성합니다.
  + 이것은 url 이 아닙니다.
## bugs
- 프로젝트의 이슈와 버그 트래킹을 볼 수 있는 url 과 이슈를 알릴 email 주소를 적습니다.
  + 패키지 사용자가 문제를 만났을 떄 도움을 줍니다.
  + url 을 지정하면 `npm bugs` 명령을 내릴 수 있습니다.
## license
- 패키지 사용자들이 패키지를 사용하기 위해 어떻게 권한을 얻는지, 금지 사항을 알리기 위해서 라이선스를 명시합니다.
  + 가장 간단한 방법은 BSD-3-Clause, MIT 등 일반적인 라이선스의 표준 SPDX ID 를 지정합니다. `"license": "BSD-3Clause"`
  
