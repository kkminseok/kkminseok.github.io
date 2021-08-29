---
title: [Project]로스트아크 레벨 알려주는 디스코드 챗봇만들기 
author: 강민석
date: 2021-08-29 00:34:50 +0800
categories: [Toy,chatbot]
tags: [chatbot]
math: true
mermaid: true
image: 
comments: true
---


# 로스트아크 챗봇

!["결과"](/assets/img/sample/chatbot/1.JPG)  


만든이유 : 같이 로스트아크하는 대학교 선배,친구들이 서로의 템렙을 수시로 물어봐서


## - 만든 과정

기존에 있던 코드를 바꿈. 필요한건 냅두고 필요없는 것은 버림.

내부 동작방식을 유추할 수 있게끔 살짝 공부했다.



```python
import asyncio
import discord
from urllib.request import urlopen
from bs4 import BeautifulSoup
from urllib import parse
from collections import deque
client = discord.Client()

# 디스코드에서 생성된 토큰을 여기에 추가
token = "디스코드 토큰값"

# 아래는 봇이 구동되었을 때 동작하는 부분
@client.event
async def on_ready():
    print("Logged in as ") #봇의 아이디, 닉네임이 출력
    print(client.user.name)
    print(client.user.id)

# 봇이 새로운 메시지를 수신했을때 동작하는 부분
@client.event
async def on_message(message):
	# 아래는 로스트 아크 홈페이지에서 아이디로 페이지를 읽어 레벨을 가져오는 부분
    #message.content는 레벨
    channel = message.channel
    if message.content == "!도움말"  : 
        await channel.send("!레벨 -> 모든 사람들 캐릭터 레벨 정보\n!사람이름 -> 해당 사람의 캐릭터 레벨들\n")
    #나      
    dic = {}
    dic["강민석"] = deque()
    dic["강민석"].append("두둥챠")
    dic["강민석"].append("BTS김지완그리고나")
    dic["강민석"].append("불닭보꺼")
    dic["강민석"].append("초이록산")
    dic["강민석"].append("두두챠")

    ##세원이형
    dic["고세원"] = deque()
    dic["고세원"].append("Fani01")
    dic["고세원"].append("Fani02")
    dic["고세원"].append("Fani03")
    dic["고세원"].append("Fani04")
    dic["고세원"].append("Fani05")
    dic["고세원"].append("Fani06")
    dic["고세원"].append("Fani07")
    if message.content == "!레벨" :   
        for name in dic : 
            #url = "https://lostark.game.onstove.com/Profile/Character/"+parse.quote(message.content[1:])
            #channel이라는 변수에는 메시지를 받은 채널의 ID를 담습니다.
            
            await channel.send(name + "\n-----------------\n")
            while dic[name] : 
                data = dic[name].popleft()
                url = "https://lostark.game.onstove.com/Profile/Character/"+parse.quote(data)
                html = urlopen(url)
                bsObject = BeautifulSoup(html, "html.parser")
                tmpContent = bsObject.find_all("div", {"class":"level-info2__item"})[0].find_all("span")[1].text
                tmp = bsObject.find_all("div", {"class":"level-info2__item"})[0].find_all("span")[1].text
                jobs = bsObject.find_all("div",{"class":"profile-character-info"})[0].find("img")["alt"]
                
                await channel.send(data+" / " + jobs + "\n"+str(tmpContent))
            await channel.send( "-----------------\n")
    elif str(message.content[1:]).isdigit() : 
        print("숫자입니다.")


    elif message.content[1:] in dic: 
        name = message.content[1:]
        while dic[name] : 
            data = dic[name].popleft()
            url = "https://lostark.game.onstove.com/Profile/Character/"+parse.quote(data)
            html = urlopen(url)
            bsObject = BeautifulSoup(html, "html.parser")
            tmpContent = bsObject.find_all("div", {"class":"level-info2__item"})[0].find_all("span")[1].text
            jobs = bsObject.find_all("div",{"class":"profile-character-info"})[0].find("img")["alt"]
                
            await channel.send(data+" / " + jobs + "\n"+str(tmpContent))
                
client.run(token)
```

## - 만든 후기 

크롤링을 하는 방식이고, 직접 아이디를 하드코딩하는게 불편함.

더 좋은 방식이 있을까 고민했지만.. 음 그사람의 개인정보로 캐릭터 레벨을 아는 것도 보안상의 문제가 있으니 넘어가기로 했따.