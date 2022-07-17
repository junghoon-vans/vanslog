---
title: "🔮 utterances 다이나믹 테마 적용하기"
date: 2022-07-17T15:12:13+09:00
draft: false
categories:
    - 블로그
tags:
    - hugo
    - utterances
    - 블로그
---

`utterances`는 개인 기술 블로그에서 많이 사용되는 오픈소스 댓글 서비스입니다. `깃헙 스타일`의 깔끔한 디자인과 `다크 모드`를 지원한다는 특징을 가지고 있습니다.

최근에는 나이트 모드와 다크모드를 `다이나믹`하게 변경할 수 있는 사이트가 많아지고 있습니다. 제 블로그 또한 이러한 방식이므로 `utterances`의 테마를 다이나믹하게 적용해야 했습니다.

구글링을 해보니 이러한 방법에 대해서 언급하는 경우가 거의 없었습니다. 그래서 이번 포스팅에서는 utterances의 테마를 다이나믹하게 적용할 수 있는 방법을 중점으로 소개하려고 합니다.

# Utterances 적용 방법

## 단일 테마 적용

웹사이트가 하나의 테마만 적용될 때는 아래와 같은 일반적인 방법으로 댓글 기능을 적용하여 사용하시면 됩니다.

1. 댓글 기능을 적용하고자 하는 레포를 `public`으로 설정
2. 깃헙에 [utterances app](https://github.com/apps/utterances) 설치
3. [utterances](https://utteranc.es) 사이트에 접속
4. `repo`와 `theme`를 지정해주고 하단에 위치한 스크립트 복사
5. 댓글을 위치시킬 부분에 해당 코드를 붙여넣기

> utterances app을 설치할 때, 모든 레포 혹은 댓글 기능이 필요한 레포만 지정하면 됩니다.

## 다이나믹 테마 적용

다크모드와 나이트모드를 모두 지원하는 사이트의 경우, `Utterances` 또한 여기에 맞춰서 테마를 변경해주어야 합니다. 그렇지 않다면 디자인적인 통일성을 잃게 됩니다.

### 댓글을 적용할 위치 선정

```html
/layouts/partials/comments.html

{{- /* Comments area start */ -}}
<div class="comments">
    <script>
        <!-- 자바스크립트 작성 -->
    </script>
</div>
{{- /* Comments area end */ -}}

```

우선 댓글을 넣을 부분을 찾습니다. 제 블로그에 사용된 hugo의 경우 테마를 오버라이딩해서 사용할 수 있습니다. 테마를 제작하는 분들도 이러한 것을 고려하여 오버라이딩할 수 있도록 파일을 미리 만들어 둡니다. 그래서 저는 `comments.html`이라는 파일만을 오버라이딩하여 댓글 기능을 적용하였습니다.

### 자바스크립트 코드 작성

```js
loadComment();
const callback = (mutationsList) => {
    mutationsList.forEach(mutation => {
        if (mutation.attributeName === "class" && document.querySelector('.utterances-frame')) {
            const message = {
                type: 'set-theme',
                theme: getTheme()
            };
            const iframe = document.querySelector('.utterances-frame');
            iframe.contentWindow.postMessage(message, 'https://utteranc.es');
        }
    })
}
const mutationObserver = new MutationObserver(callback);
mutationObserver.observe(document.body, { attributes: true });

function getTheme() { // 사이트의 테마를 가져오는 메서드
    var theme = window.localStorage && window.localStorage.getItem("pref-theme");
    if (theme == null) {
        theme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
    }
    return theme === 'dark' ? 'github-dark' : 'github-light';
}

function loadComment() {
    let s = document.createElement('script');
    s.src = 'https://utteranc.es/client.js';
    s.setAttribute('repo', 'junghoon-vans/vanslog'); // 본인 레포 지정
    s.setAttribute('issue-term', 'pathname');
    s.setAttribute('theme', getTheme());
    s.setAttribute('crossorigin', 'anonymous');
    s.setAttribute('async', '');
    document.querySelector('div.comments').innerHTML = '';
    document.querySelector('div.comments').appendChild(s);
}
```

위 코드는 `loadComments` 메서드를 이용하여 `utterances` 댓글을 불러온 이후, `mutationObserver`가 DOM의 상태 변경을 인지하여 utterances의 테마를 변경시킵니다. `getTheme` 메서드는 현재 사이트가 어떤 모드인지를 확인해서 댓글에 적용할 테마를 지정합니다.

제 블로그 테마인 `papermod`는 현재 모드에 대한 정보를 로컬스토리지에 `pref-theme`라는 이름으로 저장합니다. 그래서 이것을 통해 현재 다크 모드인지 라이트 모드인지를 확인하였습니다. 또한 테마를 직접 지정한 적이 없다면 로컬스토리지에 해당 값이 `null`일 수 있으므로, 이러한 경우 자동으로 `시스템 설정`을 따라가도록 하였습니다.

> 혹시라도 이 글을 참고하여 본인 블로그에 적용하려 하시려는 분이 계시다면 다음을 유의하시면 됩니다. `getTheme` 메서드의 경우 사이트에 따라 색상 모드를 지정하는 방식이 다를 수 있으므로, 꼭 모드를 지정하는 변수가 어떤 것인지를 확인하셔야 합니다. 둘째로 `loadCommant` 메서드에서 레포지토리 지정을 본인의 것으로 변경해주셔야 합니다.

# 마치며

블로그를 직접 커스텀하는 과정에서 프론트엔드 소스코드에 손을 대게 되었습니다. 이전까지 저의 영역은 백엔드에 한정되어 있다고 생각했는데, 막상 해보니 프론트엔드도 재미있다는 생각이 들었습니다. 자신의 한계를 제 스스로 설정하고 있었던 것은 아닌지에 대해서 생각을 하게 되는 계기가 된 것 같습니다. 앞으로는 조금 더 열린 마음으로 여러 방면으로 공부하는 마음가짐을 가져야 겠다는 생각을 하게 되었습니다.
