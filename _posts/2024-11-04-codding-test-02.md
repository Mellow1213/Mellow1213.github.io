---
title: "[BOJ][Silver I] 7562번: 나이트의 이동"
excerpt: "좌표 기반 BFS 응용"
category: boj
toc: true
last_modified: 2024-11-04T14:55:00
---
# 문제
--- 

[7562번: 나이트의 이동 바로가기](https://www.acmicpc.net/problem/7562)

![alt text](/images/image.png))

나이트가 이동하려고 하는 좌표가 있으면 몇 번 움직여야 해당 좌표로 움직이나? 에 대한 문제.

체스 말 나이트의 이동 방식을 기반으로 특정 위치까지 도달하는데 걸리는 이동 시간의 최소를 구하는 문제이다.


즉 좌표 상에서 최소 거리를 구하는 BFS 문제이다.
대신 X, Y가 +1 -1씩 움직이는 것이 아니라 나이트의 이동처럼 대각선 위와 같은 방식으로 탐색을 하면 된다.

테스트케이스는 따로 빼고,
처음 시작하는 좌표와 도착 좌표를 받은 후
BFS로 탐색을 진행한다.

좌표 기반 BFS므로 범위 설정 잘 해주면 한 좌표에서 다른 좌표로 나이트가 갈 수 있는 좌표의 범위가 8경우가 됨<br>
방문 여부를 고려해서 해당 범위마다 탐색해주자.



```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

const int dx[8] = {1, 2, 2, 1, -1, -2, -2, -1};
const int dy[8] = {2, 1, -1, -2, -2, -1, 1, 2};
vector<vector<int>> vec;

bool OutOfBound(const int& x, const int& y){
    return x<0 || x>=vec.size() || y<0 || y>=vec.size();
}

void BFS(int& startX, int& startY, int& endX, int& endY){
    queue<pair<int, int>> q;
    q.push(make_pair(startX, startY));
    vec[startX][startY] = 0;
    
    while(!q.empty()){
        int x = q.front().first;
        int y = q.front().second;
        if(x==endX && y==endY){
            break;
        }
        q.pop();
        
        for(int i=0; i<8; i++){
            int curX = x + dx[i];
            int curY = y + dy[i];
            
            if(OutOfBound(curX, curY))
                continue;
                
            if(vec[curX][curY] == -1){
                vec[curX][curY] = vec[x][y]+1;
                q.push(make_pair(curX, curY));
            }
        }
    }    
    cout << vec[endX][endY] << '\n';
}

void Solve(){
    int length;
    int startX, startY, destX, destY;
    cin >> length;
    cin >> startX >> startY;
    cin >> destX >> destY;
    
    vec.assign(length, vector<int>(length, -1));
    
    BFS(startX, startY, destX, destY);
}

int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int testCase;
    cin >> testCase;
    
    for(int i=0; i<testCase; i++){
        Solve();
    }
}
```
