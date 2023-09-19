---
layout: post
title: Restart Github Blog
---

Github Blog를 새로운 테마로 다시 시작한다.  
사내 정책으로 인해 각종 저장(업로드, 클라우드, 글쓰기 등) 행위에 관한 것은 거의 전부 막혀있다고 볼 수 있고, TIL을 작성하고 싶은 나는 Github를 통해 기록을 할 수 밖에 없었다.  
오랫동안 Markdown 작성을 손에 놓고 Notion으로 작성을 했기에 다시 익숙해지는 데에는 시간이 걸릴듯하다.

로컬에서 Github Blog 환경을 구성하는데 계속 문제가 발생하였다.

## Error installing jekyll

```
C:\Developments\FrontEnd\rubygems-2.7.7>gem install jekyll
Temporarily enhancing PATH for MSYS/MINGW...
Building native extensions. This could take a while...
ERROR:  Error installing jekyll:
        ERROR: Failed to build gem native extension.

    current directory: C:/Developments/FrontEnd/Ruby25-x64/lib/ruby/gems/2.5.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
C:/Developments/FrontEnd/Ruby25-x64/bin/ruby.exe -r ./siteconf20180806-33956-l8y76h.rb extconf.rb
creating Makefile

current directory: C:/Developments/FrontEnd/Ruby25-x64/lib/ruby/gems/2.5.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
make "DESTDIR=" clean
'make' is not recognized as an internal or external command,
operable program or batch file.

current directory: C:/Developments/FrontEnd/Ruby25-x64/lib/ruby/gems/2.5.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
make "DESTDIR="
'make' is not recognized as an internal or external command,
operable program or batch file.

make failed, exit code 1
```

위의 에러가 계속 반복되었다.  
jekyll은 32bit에서 동작한다고 하여서 `Ruby installer for windows x86`으로 계속 설치를 진행하였다. 동일 문제가 계속 반복되었고, Ruby의 문제인줄로만 알았다... 위의 에러를 해석해보자면 `MSYS/MINGW x86`으로 `jekyll installer`가 진행되는데 parsing문제가 계속 발생하는 것. DESTDIR이 정말 공백인건가? 아니면 짤리는 것인가?  
혹시 몰라 `Ruby installer for windows x64`로 Ruby 설치 → bundler 설치 → jekyll 설치. 되네?? 뭐야?  
에이, 그래도 jekyll은 32bit에서만 동작하니까 serving은 안될거야. 이것도 된다. 된다. 된다...  
그냥 시스템에 맞게 설치하고, __Devkit 포함되어야한다__ 그리고 Devkit역시 installer에 맞게 비트가 되어있어서 `MSYS/MINGW x64`로 설치되었고 그제서야 DESTDIR 문제 발생을 안하는것보니 parser가 제대로 parsing을 하는듯하다... 오늘도 삽질했다.