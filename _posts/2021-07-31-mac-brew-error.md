---
title : \[Mac OS] Error ; Unknown command ; cask 오류 해결 방법
date : 2021-07-31 00:00:00+0900
tags : [etc,mac]
---

Silicon Mac (M1) 에서 Homebrew를 통해 IntelliJ IDEA를 설치하는 도중 다음과 같은 오류를 만났다.

![brew error messages](/assets/images/2021-07-31-brew-error-unknown-command-cask.png)

해당 오류를 찾아보니 기존에는 terminal에서 `brew cask install <package name>` 을 입력하면 되었는데, 현재는 사용방법이 바뀌었기 때문에 위와 같은 오류가 난 것이었다.

`brew install --cask <package name>` 로 입력하면 정상적으로 설치된다.

![brew error solves](/assets/images/2021-07-31-brew-error-unknown-command-cask-solve.png)


### 참조
- [stack overflow - Homebrew cask option not recognized?](https://stackoverflow.com/questions/30413621/homebrew-cask-option-not-recognized)