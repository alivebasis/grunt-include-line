# LINE + Markup 템플릿화 참고가이드

- grunt include plugin을 활용한 방법으로, 라인플러스 작업환경 기준으로 정리된 내용입니다.



## 적용방법

### 1. 폴더구조 세팅 (template-mobile 기준 / 아래 그림 참고)
* src/html : include 완료된 결과 html 파일이 담긴 폴더
* src/html/tpls(생성 필요) : include 선언 html 파일이 담긴 폴더
* src/html/tpls/include(생성 필요) : include 되는 html 파일이 담긴 폴더
    (순수 컴포넌트 태그 내용만 담긴 html)
    
  ![include](http://thisisneverthat.dothome.co.kr/study/1.PNG)

### 2. Grunt 플러그인 세팅
* 아래 구문을 이용하여 플러그인 설치 (구문 뒤에 --save-dev를 붙이면, package.json와 연동되어 자동 추가됨.
```
npm install grunt-include-replace --save-dev
```
    
* Gruntfile.js 내 관련 구문 추가
```javascript
module.exports = function(grunt) {
     grunt.initConfig({
          watch: {
                //---------- COPY START
          includes: {
               files: [
                    '<%= prj.app %>/html/tpls/*.html',
                    '<%= prj.app %>/html/include/*.html'
               ],
               tasks: [
                    'includereplace',
                    'prettify'
               ]
          }
          //---------- COPY END
     }

     //--------- COPY START
     includereplace: {
         dev: {
             cwd: '<%= prj.app %>/html/tpls', // include 선언파일 폴더경로
             src: '*.html', // 결과물파일 형식
             dest: '<%= prj.app %>/html/', // 결과물파일 생성될 폴더경로
             expand: true // 결과물파일 생성을 위한 구문
         }
     }
     //---------- COPY END
     });
};
```
    
### 3. 적용 예시
* include 선언파일 - src/html/tpls/exam.html
  * 컴포넌트 태그 내에 제어할 내용이 필요한 경우, 선언 구문 내에 { "옵션명" : "옵션값" } 을 추가하여 선언하면 된다. 그렇지 않을 경우, @@include('include/component.html') 까지만 선언해주면 된다.
    
```html
<!DOCTYPE html>
<html lang="ko">
<head>
<title>HIVELAB</title>
<meta charset="utf-8">
</head>
<body>

  @@include('include/component.html', {
    "display" : "block"
  })
  
</body>
</html>
```

* include 컴포넌트 파일 - src/html/tpls/include/component.html
```html
  <div style="display:@@display">
    blah blah
  </div>
```

* 결과파일 - src/html/exam.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
<title>HIVELAB</title>
<meta charset="utf-8">
</head>
<body>
  <div style="display:block">
    blah blah
  </div>
</body>
</html>
```

