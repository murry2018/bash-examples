# dotemacs repository 패치
[목록으로 돌아가기](README.md)
2019.06.19

```sh
$ diff dotemacs/preferences/init-packages.el `# 구 버전 파일` \
       .emacs.d/preferences/init-packages.el `# 새 버전 파일` \
  | sed '1,4 d' -                            `# 1~4줄 자름` \
  | tee dotemacs/new.patch                    # diff 결과 확인
$ patch dotemacs/preferences/init-packages.el dotemacs/new.patch 
```

## 설명

[dotemacs 리포지토리](https://github.com/murry2018/dotemacs/)는 이맥스
설정 파일(~/.emacs.d)의 사본이다. 로컬의 .emacs.d는 직접 설정을
바꿔보면서 이맥스가 어떻게 동작하는지 살펴볼 때 쓰인다. 성공적으로
적용된 변경 사항들은 dotemacs에 보관되고 [메인
리포지토리](https://github.com/murry2018/dotemacs/)에 업로드된다. 두
커맨드열은 각각 변경 사항을 확인하고(`diff|sed|tee`), 변경 사항을
사본으로 쓴다(`patch`).

1. 1번째 줄(`diff`)과 2번째 줄은 기존 버전의 설정 파일과 새 버전의
   설정 파일을 비교해 [diff](https://ko.wikipedia.org/wiki/Diff)
   출력물을 만들어낸다. (파일 예시는 아래 [Appendix 1](#Appendix-1)
   참고)
   
2. 3번째 줄(`sed`)은 diff 출력에서 첫 네 줄을 잘라낸다. 첫 네 줄은
   OS별로 다른 설정을 관리하는 부분이라서 dotemacs 리포지토리에 바로
   반영되면 안 되기 때문이다. (sed 적용 전의 diff 출력 결과는 아래
   [Appendix 2](#Appendix-2) 참고)

3. 4번째 줄(`tee`)는 3번째 줄까지 만들어진
   [패치파일](https://ko.wikipedia.org/wiki/Patch)내용을 `new.patch`에
   쓰고, 그 내용을 확인시켜준다.

4. 출력된 패치 내용이 원하는 내용이 맞다면, 5번째 줄(`patch`)의 명령을
   통해 패치를 적용한다.

## Appendix 1

### 데이터 파일의 예시

dotemacs/preference/init-packages.el의 예 (`*DMZ*`의 값이 `nil`임)

```lisp
;; package archive settings
(require 'package)
(defvar *DMZ* nil
  ;; 시스템 환경에 의존하는 설정들
  )
;; 기존의 설정들
```

.emacs.d/preference/init-packages.el의 예 (`*DMZ*`의 값이 `t`임)

```lisp
;; package archive settings
(require 'package)
(defvar *DMZ* t
  ;; 시스템 환경에 의존하는 설정들
  )
;; 새로운 설정들
```


## Appendix 2

### diff 결과의 예시

```
3c3
< (defvar *DMZ* nil
---
> (defvar *DMZ* t
6c6
< ;; 기존의 설정들
---
> ;; 새로운 설정들
```

첫 네줄 부분은 시스템 환경마다 다를 수 있는 설정에 대한 안전장치이므로
곧바로 리포지토리에 적용되면 안된다.
