---
title: 깃블로그 chirpy jekyll theme 오류해결하기
date: 2022-05-18 11:37:00 +/0900
categories: [posting,welcome]
tags: [chirpy theme, 오류해결]    
---

 며칠 jekyll theme을 구경하던 중 찾은 chirpy theme 깔끔하고 원하는 기능이 다 들어있어서 사용하려는데
몇번을 다시 깔아도 오류가 발생했다. <br><br>
 튜토리얼대로 작성을해도 커밋 후 올리면

![shouldnt-be-like-this screen captured pic](/assets/img/this_is_wrong.png)

이렇게 jekyll theme 적용이 안된 index.html 파일만 그대로 보여줬다.

오랜 삽질끝에 찾은 해결방법은 역시 소스의 github issue 창이다.

나와 동일한 문제를 겪은 사용자가 2월 10일에 글을 작성했고

![someone posted same issue on chirpy jekyll theme github](/assets/img/github_issue.png)

리눅스 플랫폼 설정을 추가해보라는 글이 있다.

![someone answered to add linux platform](/assets/img/add_linux_platform.png)


```
bundle lock --add-platform x86_64-linux
```

 이전에도 해본 방법이지만 혹시 몰라 다시 시도하고 포스팅을 하나 추가했지만 여전히 blank page
 
 깃허브 action을 확인해보니 커밋한 사항이 반영이 안되어 있다.<br>
 또한 자동으로 생성된다는 gh-pages 브랜치가 생성이 안됐으며 run fail이 떴다. 
 
 이 문제 역시 해결방법이 있었다.
 
 ![someone answered to change workflow permissions](/assets/img/permisson_issue.png)
 
 Settings> Actions > General 에 가서 permission scope를 변경한다.
 

 fail이 발생한 커밋 상세페이지에 들어가 rerun을 클릭하니 제대로 돌아갔고!!

 ![finally succeed to upload git pages](/assets/img/gitpage.png)
 
 제대로 동작하는 깃 블로그를 확인할 수 있었다.
 
 오늘의 삽질로 얻은 교훈 : 바로 구글링하지말고 공식문서에 달린 이슈와 답글을 먼저 확인하자