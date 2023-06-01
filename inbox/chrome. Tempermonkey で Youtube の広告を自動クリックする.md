---
type: note
---

#chrome

---
2022-09-20  09:52

# chrome. Tempermonkey で Youtube の広告を自動クリックする

extention で Tempermonkey を install した。

![[Pasted image 20220920095313.png]]

新規スクリプトを追加する。

![[Pasted image 20220920095356.png]]

interval より mutationObserver を使って監視するとのこと。

https://qiita.com/mt877/items/110ae331917bc8fd8018

```js
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://www.youtube.com/
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
    const host_url = location.hostname;

    // CSS  display none
    const newStyle =document.createElement('style')
    newStyle.innerText = '.ytp-ad-overlay-container{tisplay:none!important;}';
    document.getElementByTagName('head').item(0).appendChild(newStyle);

    // 広告をスキップバナーをクリックして消す
    const clickSkipButton = () => {
        const $skipButton = document.getElementByClassName("ytp-ad-skip-button-container")[0];
        if($skipButton) $skipButton.click();
    }

    const obConfig = {
        childList: true,
        subtree: true
    }
    // コンテンツプレイヤーが現れたらクリック (play start)
    const observer = new MutationObserver((mutations) => {
        mutations.forEach((mutation) => {
            if( mutation.addedNodes.length && mutation.addedNodes[0].className == 'ytp-ad-player-overlay' ) clickSkipButton();
        })
    })

    const initInterval = setInterval(() => {
        if(document.getElementById("ytd-player") != null) {
            const obTarget = document.getElementById("ytd-player");
            observer.observe(obTarget, obConfig);
            clearInterval(initInterval);
        }
        clickSkipButton();
    }, 1000);
    
    console.log("monkey: ", host_url);
})();
```

うまくいかなかった。後で、

extention 使おっと。

![[Pasted image 20220920110955.png]]

これ効く。てゆーかすごくね。

