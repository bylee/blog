---
layout: single
title:  "북마클릿을 이용한 지갑 만들기"
date:   2019-09-20 00:33:21 +0900
categories: blockchain
tags: blockchain, wallet, bookmarklet
---

이번 블로그에서는 웹 환경에서 키스토어 파일을 사용해서 지갑을 활성화하는 기능을 개선하는 과정을 담고 있습니다. 글을 읽기에 부담이 없도록 세가지 소주제로 나누어 진행합니다.

1. 웹 환경에서 키스토어 파일을 사용하는 지갑
1. 북마클릿
1. 북마클릿을 활용한 지갑

블록체인을 사용하기 위해서는 지갑이라고 불리는 앱이 필수적입니다. 콜드 월렛이라고 불리는 실물 형태의 지갑도 있고, 모바일 앱이나 브라우져의 플러그인 형태도 있습니다. 또 이러한 설치형 지갑이 아닌 서비스형 지갑의 형태가 존재하는데 바이낸스 분산 거래소나 마이 이더 월렛같은 것이 있습니다. 오늘은 웹 앱에서 지갑을 사용하는 것에 대해서 살펴보고 어떤 문제가 있는지 알아보도록 하겠습니다.

# 웹 환경에서 키스토어 파일을 사용하는 지갑

아르고를 바이낸스 분산 거래소에 상장하는 과정에서 지갑을 생성하고, 사용해보면서 많이 놀랐는데 그 이유는 사용하기가 매우 불편했기 때문입니다. 블록체인 기술이 충분히 성숙되지 않아 기술에 더 많이 신경을 쓴 탓이겠지만, 일반 사용자를 대상으로 하는 서비스 치고는 UX가 좋지 못합니다. 이후에 마이 이더 월렛이라는 지갑도 비슷한 방식으로 사용하는 것을 보고 단지 바이낸스만의 문제는 아닌 것을 알게되었습니다. 그리고, 이런 문제를 어떻게 해결하고 더 편리하게 서비스를 사용할 수 있을까하는 고민을 시작하게 되었습니다.(왜 니가 남의 서비스를 고민하냐고 한다면 할 말이 없습니다)
먼저 바이낸스 거래소를 사용하는 형태를 살펴보겠습니다.

![Binance Exchange](/blog/assets/images/bookmarklet-wallet/binance-step1.png "Binance Exchange")

우측 상단에 Connect Wallet이라는 버튼이 보이는데, 이 버튼을 사용해서 지갑을 활성화할 수 있습니다.

![Select wallet type](/blog/assets/images/bookmarklet-wallet/binance-step2.png "Select wallet type")

네 가지 종류의 지갑을 지원하는데, 오늘 살펴볼 지갑은 Keystore File을 사용한 지갑입니다.

![Wallet step](/blog/assets/images/bookmarklet-wallet/binance-step3.png "Wallet step")

지갑을 활성화하기 위해서는 총 네 단계를 거쳐야 합니다.

1. 지갑의 종류를 선택한다.
1. 개인키 파일을 선택한다.
1. 패스워드를 입력한다.
1. 해제 버튼을 누른다.

바이낸스 거래소를 사용하면서 매우 불편하게 느꼈던 지점이었습니다.

이더리움 지갑으로 유명한 마이이더월렛도 비슷합니다.

![My ehterwallet](/blog/assets/images/bookmarklet-wallet/mew-step1.png "My ehterwallet")

지갑의 유형을 선택하고

![Select file in my ehterwallet](/blog/assets/images/bookmarklet-wallet/mew-step2.png "Select file in my ehterwallet")

키스토어를 눌러서 파일을 선택하고

![Input password](/blog/assets/images/bookmarklet-wallet/mew-step3.png "Input password")

패스워드를 입력합니다.

UI가 조금 다른 형태지만 전체적인 흐름은 같다고 볼 수 있습니다.

이 UX의 불편함을 해결하기 위한 기반 기술인 ‘북마클릿’에 대해 알아보겠습니다.

# 북마클릿

북마클릿은 표준 웹 기술로 분류되지만 크게 관심받지 못하는 기술입니다. W3C 스펙에 Favelets라는 기술로 소개되지만 개인적으로는 북마클릿이라는 이름이 이 기술을 잘 드러내기 때문에 선호합니다.

모든 브라우저는 북마크(책갈피)라는 기능을 가지고 있습니다. 북마크는 미리 홈페이지의 주소를 저장해 놓고 북마크를 클릭하는 것만으로 해당 홈페이지로 이동할 수 있는 기능입니다. 일종의 단축 아이콘 같은 것이죠.

북마크에 저장되는 홈페이지의 주소는 Uniform Resource Locator, 즉 URL이라는 형태로 표현됩니다.

(예시) https://www.blocko.io/resource/blog/

‘://’ 앞 부분은 URL을 어떻게 처리해야 하는지를 지칭하는 부분으로 ‘schema’ 또는 ‘protocol’로 부릅니다. ‘www.blocko.io’는 ‘서버의 이름’ 또는 ‘IP 주소’가 들어가는 부분이고, 그 뒤의 나머지 부분은 ‘경로’라고 하는 부분입니다.

URL은 더 많은 부분으로 구성되어 있지만, 본 문서에서 기억해야 할 것은 ‘스키마(schema)’입니다. 브라우저는 스키마(schema)에 따라 URL을 처리하는 주체가 달라지는데, http나 https뿐만 아니라 file이나 ftp 같은 다양한 스키마(schema)를 처리할 수 있습니다.

그중에서도 javascript라는 스키마(schema)를 가진 URL은 자바 스크립트 엔진에 전달됩니다. javascript라는 스키마(schema)를 가진 URL을 북마크에 저장하면 어떤 일이 벌어질까요?

네, 방금 당신은 북마클릿이라는 기술을 배웠습니다. 단지 북마크에 ‘javascript://’로 시작하는 URL을 넣으면 그것을 우리는 북마클릿이라고 부릅니다. 어떻게 동작하는지 간단히 확인해보겠습니다.



먼저 북마크 툴바에서 우 클릭을 하면 “새 북마크”라는 메뉴가 나타납니다.

![Start bookmarklet](/blog/assets/images/bookmarklet-wallet/bookmark-step1.png "Start bookmarklet")

이 메뉴를 누르고 북마크 이름에 Hello, world를 입력하여 북마크 주소에 아래와 같이 입력합니다.

```javascript
javascript:alert('북마클릿의 세계에 오신 것을 환영합니다.')
```

![Input hello, world](/blog/assets/images/bookmarklet-wallet/bookmark-step2.png "Input hello, world")

이제 버튼을 눌러 보면, 작은 대화 상자에 “북마클릿의 세계에 오신 것을 환영합니다.”를 볼 수 있습니다.

![Run hello, world](/blog/assets/images/bookmarklet-wallet/bookmark-step3.png "Run hello, world")

북마클릿으로 사용되는 자바스크립트는 약 5kb의 길이 제한을 가지고 있지만, 미리 자바스크립트를 서버에 올려두고 이를 로딩하는 부트스트랩 코드를 북마클릿으로 만드는 방법을 통해 해결할 수 있습니다.

북마클릿이 재미있는 이유는 브라우저가 다른 사이트를 방문한 상태에서 북마클릿을 실행하면 사이트를 떠나지 않을 뿐만 아니라 사이트의 정보에 접근할 수 있다는 점입니다.

앞에서 만든 북마클릿을 다음과 같이 수정해보겠습니다.

```javascript
javascript:alert(document.title + '의 세계에 오신 것을 환영합니다.')
```

![Run in daum](/blog/assets/images/bookmarklet-wallet/bookmark-step4.png "Run in daum")

![Run in facebook](/blog/assets/images/bookmarklet-wallet/bookmark-step5.png "Run in facebook")

북마클릿을 사용한 멋진 사이트를 한번 구경해보시죠.

[https://eatponies.com/][eatponies]->홈페이지 위에 펜으로 글씨를 쓰거나 그림을 그릴 수 있게 해줍니다.

[http://www.memonic.com/][memonic]->홈페이지를 메모에 기록해 둘 수 있습니다.

이렇게 북마클릿은 어떤 사이트에 마치 플러그인처럼 기능을 부가할 수 있도록 해줍니다.

# 북마클릿을 활용한 지갑

북마클릿으로 키스토어 지갑을 개선하는 개략적인 방법은 UX를 개선하는 자바스크립트를 만들고 이것을 북마클릿에 등록하는 것입니다. 예시로 바이낸스 거래소의 로그인 과정을 개선해보겠습니다.

바이낸스 로그인 화면에서 파일 업로드를 위한 입력을 개발자 도구로 찾아봅니다.

![Check upload button](/blog/assets/images/bookmarklet-wallet/inject-step1.png "Check upload button")

안타깝게도 id와 class를 가지고 있지 않습니다. 태그 이름으로 확인해보겠습니다.

![Check upload button](/blog/assets/images/bookmarklet-wallet/inject-step2.png "Check upload button")

두 개의 input이 있고 그중에 첫 번째가 파일을 위한 input입니다. 이 input에 파일의 내용을 입력하도록 프로그래밍을 하겠습니다.

```javascript
const fileInput = document.querySelector('input[type="file"]');
```

![Check upload button attrs](/blog/assets/images/bookmarklet-wallet/inject-step3.png "Check upload button attrs")

react를 사용해 만들어진 것 같습니다. react의 컴포넌트의 이벤트 핸들러를 트리거해야 합니다.

```javascript
const instanceAttr = Object.keys(fileInput).find(key=>key.startsWith("__reactInternalInstance$"));
const reactComponent = fileInput[instanceAttr];
```

![Find handler](/blog/assets/images/bookmarklet-wallet/inject-step4.png "Find handler")

우리가 원하는 이벤트 핸들러가 저기 보이는군요. 이젠 이벤트 핸들러에 전달할 인자를 만들어보겠습니다.

```javascript
const content = '{"version":1,"id":"784517f5–5dc1–437b-b154-ed1759414a08","crypto":{"ciphertext":"f4c000cb92df43055568591a1a51c5f7f5eb83f73a73c7933c3dc26e0a1cdb76","cipherparams":{"iv":"2e2f8e2e2b9a10c3770aa19b7dfb92f9"},"cipher":"aes-256-ctr","kdf":"pbkdf2","kdfparams":{"dklen":32,"salt":"f4f609e63c16c6133ff68967c2c33ba0872603fa09e8efd6d098a1685a7c0228","c":262144,"prf":"hmac-sha256"},"mac":"9615f559373167aaf099e0487325cf21a10041a5401f477c75c337ca17fdcf5db0b4adaf6ff62509 a8b36dbae9a7ed3df794c66901b4e0b69fe8d8fb919307a3"}}';
const blob = new Blob([content], {type : 'application/json'});
const outputfile=new File([blob], "adhoc.keystore");
```

키 스토어 파일의 내용을 기반으로 파일 객체를 만듭니다.

이 객체를 핸들러에게 전달합니다.

```javascript
reactComponent.memoizedProps.onChange({ currentTarget: { files: [outputfile] } });
```

이 과정을 실행하면, 화면에 파일이 선택되었다는 체크박스가 나타납니다.

![File selected](/blog/assets/images/bookmarklet-wallet/inject-step5.png "File selected")

편의를 개선하기 위해 처음에 지갑의 종류를 선택하는 동작과 패스워드 입력에 포커스가 가도록 합니다.

 

지갑 선택은 다음의 스크립트로 가능합니다.

```javascript
document.querySelector('[data-cy="menu-KeyStore File"]').click()
```

마지막으로 패스워드 입력에 포커스를 보내는 명령입니다.

```javascript
document.querySelector('input[type="password"]').focus()
```

필요한 모든 기능을 자동화하였습니다. 공간을 절약하기 위해 임시변수들을 제거하면 전체 스크립트는 다음과 같습니다.

```javascript
document.querySelector('[data-cy="menu-KeyStore File"]').click();

document.querySelector('input[type="file"]')[Object.keys(document.querySelector('input[type="file"]')).find(key=>key.startsWith("__reactInternalInstance$"))]. memoizedProps.onChange({ currentTarget: { files: [new File([new Blob(['{"version":1,"id":"784517f5–5dc1–437b-b154-ed1759414a08","crypto":{"ciphertext":"f4c000cb92df43055568591a1a51c5f7f5eb83f73a73c7933c3dc26e0a1cdb76","cipherparams":{"iv":"2e2f8e2e2b9a10c3770aa19b7dfb92f9"},"cipher":"aes-256-ctr","kdf":"pbkdf2","kdfparams":{"dklen":32,"salt":"f4f609e63c16c6133ff68967c2c33ba0872603fa09e8efd6d098a1685a7c0228","c":262144,"prf":"hmac-sha256"},"mac":"9615f559373167aaf099e0487325cf21a10041a5401f477c75c337ca17fdcf5db0b4adaf6ff62509a8b 36dbae9a7ed3df794c66901b4e0b69fe8d8fb919307a3"}}'], {type : 'application/json'})], "adhoc.keystore")] } });

document.querySelector('input[type="password"]').focus();
```

이제 이 자바스크립트를 북마크에 등록하기만 하면 됩니다.

javascript: 를 앞에 붙여주시는 것 잊지마시고요

![Input complete bookmarklet](/blog/assets/images/bookmarklet-wallet/inject-step6.png "Input complete bookmarklet")

# 마치며

지금까지 북마클릿을 사용하여 지갑의 사용성을 향상시키는 내용이었습니다. 블록체인 기술은 그 혁신성으로 많은 관심을 받고 있는 것에 비해 서비스/앱의 사용성을 등한시하는 경우가 많아 안타까운 경우가 많습니다. 앞으로 좋은 블록체인 서비스와 앱들이 더욱 사랑받고, 탈중앙화의 혜택이 모두에게 돌아가길 바랍니다.

[eatponies]: [https://eatponies.com/]
[memonic]: [http://www.memonic.com/]