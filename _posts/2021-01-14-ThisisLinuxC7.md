---
title: 이것이 리눅스다. - 7. 셸 스크립트 프로그래밍
author: 강민석
date: 2021-01-14 12:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **7.1 셸의 기본** ##

- 셸은 사용자가 입력한 명령을 해석해 커널에 전달하거나, 커널의 처리 결과를 사용자에게 전달하는 역할을 합니다.

### **7.1.1 CentOS의 bash셸** 

CentOS에서 기본적으로 사용하는 셸의 이름은 bash이며 bash셸이라고 읽습니다.

- Alias 기능(명령어 단축 기능)
- History 기능
- 연산 기능
- Job Control 기능
- 자동 이름 완성 기능(Tab)
- 프롬프트 제어 기능
- 명령 편집 기능

### **7.1.2 환경 변수**

셸은 여러 가지 환경 변수를 갖습니다. 설정된 환경 변수는 'echo$환경 변수이름'형식으로 실행하면 확인할 수 있습니다.

![](/assets/img/sample/Linux/ThisisLinux/C7/env.JPG)  

환경 변수 값을 변경하려면 'export 환경변수=값' 형식을 실행합니다.  
'printenv'명령어를 실행하면 출력됩니다.  

-----  


## **7.2 셸 스크립트 프로그래밍 실습**

- 리눅스의 많은 부분이 셸스크립트로 작성되었기에 배워두면 좋다고 합니다.

### **7.2.1 셸 스크립트 작성과 실행**

셸 스크립트 작성
```console
vi name.sh
```

![](/assets/img/sample/Linux/ThisisLinux/C7/namesh.JPG)  

**첫 번째줄의 의미는 bash를 사용하겠다는 의미로 꼭 써야합니다.**

```console
sh name.sh //실행
```

![](/assets/img/sample/Linux/ThisisLinux/C7/result1.JPG)  

### **7.2.2 변수** 

- 변수의 기본
    + 셸 스크립트에서는 미리 선언하는 것이 아닌 처음 변수에 값이 할당되면 자동으로 변수가 생성된다.
    + 변수에 넣는 모든 값은 '문자열' 취급 -> 숫자를 넣어도 문자열 취급
    + 대소문자 구분, 대입 시 '=' 좌우에 공백이 없어야함.

**변수의 입력과 출력**

'$'라는 문자가 들어간 글자를 출력하려면 ''로 묶어주거나 앞에 '\'를 붙여야합니다. 또한, ""로 변수를 묶거나 묶지않아도 됩니다.  

```console
#!/bin/sh
myvar="Hi Woo"
echo $myvar     //정상출력
echo "$myvar"   //정상출력
echo '$myvar'   //'$myvar'출력
echo \$myvar    //$를 문자열 취급하여 $myvar출력
echo input plz :
read myvar
echo '$myvar' = $myvar
exit 0
```
![](/assets/img/sample/Linux/ThisisLinux/C7/result2.JPG)  


**숫자 계산**

- 기본적으로 변수에 넣은 값은 모두 문자열 취급하므로 'expr'키워드를 사용해야 합니다.  
- (`) 키로 수식을 묶어야 한다고 합니다.  
- 괄호를 사용하려면 '\'를 붙여줘야한다고 합니다.
- 사칙연산 중 곱하기(*)에도 '\'를 붙여줘야한다고 합니다.

```console
#!/bin/sh
num1=100
num2=$num1+200
echo $num2
num3=`expr $num1 + 200`
echo $num3
num4=`expr \( $num1 + 200 \) / 10 \* 2`
echo $num4
exit 0
```
![](/assets/img/sample/Linux/ThisisLinux/C7/result3.JPG)  

**에러가 2번 났는데 주의점이 있습니다.**
- 첫 번째는 ' 따옴표가 1번옆에 있는 ` 이걸 써야한다는 점
- 두 번째는 괄호에서 '(' 이부분 에 붙여 쓰면 에러납니다. 그래서 한 칸 띄고 써줘야합니다.

**파라미터 변수**

- 파라미터 변수는 '$0', '$1' 등의 형태를 갖습니다. 이는 실행하는 명령의 부분 하나하나를 변수로 지정한다는 의미입니다.
- 예를 들어 'yum -y install gftp'는 yum은 $0, -y는 $1... 이런 식입니다.  

```console
vi paravar.sh

#!/bin/sh
echo "first excute file name is <$0>"
echo "first parameter is <$1>, second parameter is <$2>"
echo "all of parameter is <$*>"
exit 0

```

![](/assets/img/sample/Linux/ThisisLinux/C7/result4.JPG)  


### **7.2.3 if문과 case문**

- if문의 주의점은 [조건]의 각 단어는 모두 공백이 있어야 합니다.

```console
vi if1.sh

#!/bin/sh
if [ "woo" = "woo" ]
then
        echo "true"
fi
exit 0

sh if1.sh
```

**뒤에는 vi로 문서여는 명령어와 sh로 실행하는 명령어를 적지 않겠습니다. 모두 직접해봤으니 안되는 예제는 없습니다.**

**if else문**

```console
#!/bin/sh
if [ "woo" != "woo" ]
then
        echo "true"
else
        echo "false"
fi
exit 0
```
**조건에 들어가는 비교 연산자**

```console
#!/bin/sh
if [ 100 -eq 200 ]
then
        echo "100 equal 200"
else
        echo "100 nequal 200"
fi
exit 0

```
![](/assets/img/sample/Linux/ThisisLinux/C7/oper.JPG)  


**파일과 관련된 조건**


```console
#!/bin/sh
fname=/lib/systemd/system/httpd.service
if [ -f $fname ]
then
        head -5 $fname
else
        echo "not install web service"
fi
exit 0

```

![](/assets/img/sample/Linux/ThisisLinux/C7/file.JPG)  

**case~esac문**

- if문은 참과 거짓이라는 두 가지 경우만 사용할 수 있습니다. 2중 분기인 if문과 다르게 다중분기인 case문을 사용해봅니다.

```console
#!/bin/sh
case "$1" in
        start)
                echo "start!";;
        stop)
                echo "stop!";;
        restart)
                echo "restart!";;
        *)
                echo "nothing";;
esac
exit 0

```

사용방법은
```console
sh case1.sh stop
sh case1.sh start
sh case1.sh restart
sh case1.sh 아무글자
```

**AND, OR 관계 연산자**

- and는 '-a' 또는 '&&'를 사용하며 or는 '-o' 또는 '||'를 사용한다.

```console
#!/bin/sh
echo "input filename"
read fname
if [ -f $fname ] && [ -s $fname ] ; then
        head -5 $fname
else
        echo "found not file or size is zero"
fi
exit 0
```

### **7.2.4 반복문** ###

**for~in문**

```console
for i in 1 2 3  4 5 6 7 8 9 10
do
        hap=`expr $hap + $i`
done
echo "sum of 1 ~ 10" : $hap
exit 0

//현재 디렉터리에 있는 셸 스크립트 파일 이름과 앞 3줄 출력
for fname in $(ls *.sh)
do
    echo "-----$fname-----"
    head -3 $fname
done
exit 0

```

- 정말 불편하다..

**while문**

```console
#!/bin/sh
while [ 1 ]
do
        echo "CentOS 7"
done
exit 0

//1부터 10까지 합
#!/bin/sh
hap=0
i=1
while [ $i -le 10 ]
do
        hap=`expr $hap + $i`
        i=`expr $i + 1`
done
echo "sum of 1 ~ 10 : " $hap
exit 0


```

- for문보다는 낫네..

**until문**

- until문은 while문과 용도가 거의 같지만, 조건식이 거짓인 동안 계속 반복합니다.
- 1~10 합 코드의 while문을 다음과 같이 바꾸면 됩니다. until [ $i -gt 10 ]

**break,continue,exit,return문**

```console
while [ 1 ] ; do
        read input
        case $input in
          b | B)
           break;;
          c | C)
            echo " continue is return to while"
            continue;;
          e | E)
            echo " exit is exit function"
            exit 1;;
        esac;
done
echo "input beak"
exit 0

```

### **7.2.4 기타 알아둘 내용** ###

**사용자 정의 함수**

```console
#!/bin/sh
myFunction(){
        echo "in function"
        return
}
echo    "start program"
myFunction
echo    "exit program"
exit 0

```

**함수 파라미터 사용**

- 함수 안에서는$1 , $2 ...로 사용합니다.

```console
#!/bin/sh
hap (){
echo `expr $1 + $2`
}
echo "10 + 20"
hap 10 20
exit 0

```

**eval**

- 문자열을 명령문으로 인식하고 실행합니다.

```console
#!/bin/sh
str="ls -l anaconda-ks.cfg"
echo $str
eval $str
exit 0

```

**export**

- 외부 변수로 선언하여 다름 프로그램에서도 사용할 수 있게 합니다.

```console
vi exp1.sh

#!/bin/sh
echo $var1
echo $var2
exit 0

vi exp2.sh

!/bin/sh
var1="local var"
export var2="export var"
sh exp1.sh
exit 0

sh exp2.sh
```

**printf**

- c 언어의 printf() 함수와 비슷하게 형식을 지정해서 출력할 수 있습니다.

```console
#!/bin/sh
var1=100.5
var2="fun...linux"
printf "%5.2f \n\n \t %s \n" $var1 "$var2"
exit

```

**set과 $(명령어)**

- 리눅스 명령어를 결과로 사용하려면 '$(명령어)' 형식을 사용해야 합니다. 또 결과를 파라미터로 사용하고자 할 때는 'set' 명령어와 함게 사용합니다.

```console
#!/bin/sh
echo "today is $(date)"
set $(date)
echo "today is $4"
exit 0
```

**shift**

- 파라미터 변수를 왼쪽으로 한 단계씩 아래로 쉬프트(이동)시킵니다.

- 기존 코드의 문제점
```console
#!/bin/sh
myfunc(){
        echo $1 $2 $3 $4 $5 $6 $7 $8 $9 $10 $11
}
myfunc AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
exit 0

```
![](/assets/img/sample/Linux/ThisisLinux/C7/result5.JPG)  

- AAA0, AAA1이 나온 이유는 $10을 $1 + 0으로 인식하기 때문입니다. 따라서 shift연산을 해줘야합니다.

- 개선한 코드

```console
#!/bin/sh
myfunc(){
        str=""
        while [ "$1" != "" ] ; do
                str="$str $1"
                shift
        done
        echo $str
}
myfunc AAA BBB CCC DDD EEE FFF GGG HHH III JJJ KKK
exit 0

```

-----  


다 따라치느라 너무 힘들었다.. 후 기본적인 문법지식이 있으면 셸 문법봐도 대강 이해할 것이라 생각합니다. 제가 그랬으니까요..

