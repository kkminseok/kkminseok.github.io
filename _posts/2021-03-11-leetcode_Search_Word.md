---
title: leetcode(리트코드)79-Search Word
author: 강민석
date: 2021-03-11 01:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 79 - Search Word 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/word-search/>  

![](/assets/img/sample/leetcode/79/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/79/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- board에 있는 문자를 연결하여 주어진 문자열을 만들 수 있는지 확인합니다.
- string의 제한이 클 수도 있고 board의 크기도 거의 무제한일 수 있으므로 이 부분에 유의해야합니다.
- c++은 제가 작성한 코드지만, board의 크기를 지정해버려서 좋은 코드가 아닙니다. python으로 옮기기에 비효율적이라 python은 다른사람들의 코드를 긁어왔습니다.

-----  

## 5. code

**python**

```python
class Solution:
    def exist(self, board, word):
    if not board:
        return False
    for i in xrange(len(board)):
        for j in xrange(len(board[0])):
            if self.dfs(board, i, j, word):
                return True
    return False

# check whether can find word, start at (i,j) position    
def dfs(self, board, i, j, word):
    if len(word) == 0: # all the characters are checked
        return True
    if i<0 or i>=len(board) or j<0 or j>=len(board[0]) or word[0]!=board[i][j]:
        return False
    tmp = board[i][j]  # first character is found, check the remaining part
    board[i][j] = "#"  # avoid visit agian 
    # check whether can find "word" along one direction
    res = self.dfs(board, i+1, j, word[1:]) or self.dfs(board, i-1, j, word[1:]) \
    or self.dfs(board, i, j+1, word[1:]) or self.dfs(board, i, j-1, word[1:])
    board[i][j] = tmp
    return res
        
```

-----  

**c++**

```c++
int dx[4] = {-1,0,1,0};
int dy[4] = {0,-1,0,1};
bool v[200][200] ={0,};
class Solution {
public:
    bool DFS(int x,int y,vector<vector<char>>& board,int m,int n,string word,int depth)
    {
        if(depth == word.size()-1 )
        {
            if(board[x][y] == word[depth])
                return true;
            else
                return false;
        }   
        for(int k = 0;k<4;++k)
        {
            int newX = x + dx[k];
            int newY = y + dy[k];
            if(0<=newX && newX<m && 0<=newY && newY<n && board[newX][newY] == word[depth+1] && !v[newX][newY])
            {
                //cout<<newX<<newY<<board[newX][newY]<<" "<<depth<<'\n';
                v[newX][newY] = true;
                if(DFS(newX,newY,board,m,n,word,depth+1))
                    return true;
                v[newX][newY]=false;
            }
        }
        return false;
        
        
    }
    
    bool exist(vector<vector<char>>& board, string word) {
        //DFS
        int row = board.size();
        int col = board[0].size();
        if(row * col < word.size())
            return false;
        for(size_t i = 0;i<row;++i)
        {
            for(size_t j = 0 ; j<col;++j)
            {
                memset(v,false,sizeof(v));
                if(board[i][j] == word[0])
                {
                    v[i][j]=true;
                    if(DFS(i,j,board,row,col,word,0))
                        return true;
                }
                
            }
        }
        return false;
        
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 40% python 5%..?**  
![](/assets/img/sample/leetcode/79/result.JPG)  


**c++ 100% 12ms code**

```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if(board.empty()) return false;
        h = board.size();
        w = board[0].size();
        for(int i = 0; i<h; ++i) {
            for(int j = 0; j<w; ++j)
            if(search(board, word, 0, i, j)) return true;
        }
        return false;
    }
private:
    int w, h;
    
    bool search(vector<vector<char>>& board, string& word, int d, int i, int j) {
        if(i<0 || j<0 || i>=h || j>=w || board[i][j]!=word[d]) return false;
        if(d == word.size()-1) return true;
        char cur = board[i][j];
        board[i][j] = '0';
        bool found = search(board, word, d+1, i+1, j) 
                  || search(board, word, d+1, i-1, j)
                  || search(board, word, d+1, i, j+1)
                  || search(board, word, d+1, i, j-1);
        board[i][j] = cur;
        return found;
    }
};
```


방문처리를 임시 값인 '0'로 대체하여 풀었고 로직은 비슷합니다..


