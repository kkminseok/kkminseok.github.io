---
title: The deployment failed because no instances were found for your deployment group. AWS 해결방법
author: 강민석
date: 2022-02-03 00:50:50 +0800
categories: [Error,AWS]
tags: [AWS]
math: true
mermaid: true
image: 
comments: true
---

The deployment failed because no instances were found for your deployment group. Check your deployment group settings to make sure the tags for your Amazon EC2 instances or Auto Scaling groups correctly identify the instances you want to deploy to, and then try again.

해결방법이다.

![](/assets/img/sample/aws/deploy.png)


수 많은 고통의 흔적이 보이는가?

진짜 삽질을 줫나 많이했다.

구글링을 하다보면 크게 3가지 방법을 알려준다.

1. 위의 오류는 태그 이름의 매칭 실패에 관한 에러입니다.  
해결 방법은 ec2>인스턴스에서 인스턴스의 이름과 배포 그룹의 태그 Name의 value값이 일치해야합니다.
2. /var/log/aws/code-agent 로그를 봐라
3. 그냥 인스턴스 갈아 엎어라.
4. 사용자의 role이 잘 설정되었나 봐라.
등 ..

1번의 경우는 모든 연관관계가 잘 설정되었나 보라는거다.

봤다.

실패

2번 로그를 봐도 별 내용이 없었던 것 같았다.

3번은 시도조차 하기 싫었다. 

4번은 잘설정되어 있었다.

도대체 뭐가 문제였을까?

그래도 결국 로그를 보라고..


```text
\"error_code\":5,\"script_name\":\"\",\"message\":\"The CodeDeploy agent did not find an AppSpec file within the unpacked revision directory at revision-relative path ~어쩌구 저ㄱ쩌구
```

ㅇ?

갑자기 막 배포를 시도하다보니 로그에 찍혔다.

해석을 해보니 AppSpec파일을 모르겠단다.

분명 나는 작성해쓴데 왜 모르겠단거지?

하면서 github action에 등록한 .yml 파일을 봤다.

```text
~
    # Jar 파일 Copy
    - name: Copy Jar
      run: cp ./build/libs/*.jar ./deploy/

    # 압축파일 형태로 전달
    - name: Make zip file
      run: zip -r -qq -j ./coinserver.zip ./deploy
      
    # S3 Bucket으로 copy
    - name: Deliver to AWS S3
      ~ 가독성을 위한 코드

    # appspec.yml 파일 복사
    - name: Copy appspec.yml
      run: cp appspec.yml ./deploy
      
    # 배포
    - name: CodeDeploy
      ~ 가독성을 위한 코드
```

이렇게 되어 있었다.

내가 대충 알기론 이 코드들의 흐름이 없다고 알고 있는데 설마?..

S3 Bucket으로 보내고 appspec.yml을 복사해서 S3에 appspec.yml이 도달하지 않았나?

라는 생각을 하게 되었다.

그래서 코드의 순서를 바꾸었다.

```text
~
    # Jar 파일 Copy
    - name: Copy Jar
      run: cp ./build/libs/*.jar ./deploy/

    # appspec.yml 파일 복사
    - name: Copy appspec.yml
      run: cp appspec.yml ./deploy

    # 압축파일 형태로 전달
    - name: Make zip file
      run: zip -r -qq -j ./coinserver.zip ./deploy
      
    # S3 Bucket으로 copy
    - name: Deliver to AWS S3
      ~ 가독성을 위한 코드
      
    # 배포
    - name: CodeDeploy
      ~ 가독성을 위한 코드
```

요로콤

그랬더니 성공

진짜 어이가 없지만 해결해서 다행이다 

![](/assets/img/sample/aws/result.png)

내 EC2에 잘 올라왔다..

ㅠㅠㅠㅠㅠ

