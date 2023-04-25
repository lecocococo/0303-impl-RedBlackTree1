# 레드블랙트리 

- 균형 이진 탐색트리
- 각 노드들은 red or black을 가진다.
- 삽입, 삭제에 logN

## 레드블랙트리의 규칙
1. 모든 노드는 red아니면 black의 색상을 가진다.
2. 루트노드와 모든 리프노드(null 노드)는 black색상이다.
3. 레드 노드의 자식으로 블랙 노드만을 가진다.
4. 루트노드에서 모든 리프 노드까지의 경로에서 거치는 블랙노드의 개수가 같다.

-----
## 삽입

1. 기본적으로는 BST의 삽입 알고리즘과 같음
2. 루트노드를 제외한 모든 노드들은 삽입 될때 빨간색이다. 
3. 균형을 맞추기위해 2가지 작업이 추가됨

    ### 기본

        삽입하려는 노드 K의 부모노드 P의 색이 검은색인 경우
         => 자신의 위치에 삽입
    ### restructuring (삽입후에 일어남으로 시간복잡도 O(logN), restructuring 자체는 O(1))
        삽입하려는 노드 K의 부모노드 P의 색이 빨간색
         a. 부모노드 P의 형제노드가 검은색이거나 null인경우 발생
         => restructuring 발생
        
        삽입한 노드 K, 부모 노드 P, 조상노드 G를 가지고 수행
        1. K. P ,G 중 중간값을 부모로 만듬
        2. 새롭게 부모가 된 노드를 검은색으로 만들고 나머지를 빨간색으로 만든다.
        3. 중간 값이 루트가 된후 자식의 개수가 3개라면 아래와같이 G의 왼쪽 또는 오른쪽 자식으로 붙인다.
    ![img.png](img.png)

    ### recoloring (시간 복잡도 O(logN))
        삽입하려는 노드 K의 부모노드 P의 색이 빨간색
         a. 부모노드 P의 형제노드가 빨간색인 경우 발생
         => recoloring 발생

        삽입한 노드 K, 부모 노드 P, 조상노드 G를 가지고 수행
        1. P와 P의 형제를 검은색으로 바꾸고 G를 빨간색으로 바꾼다.
           1-a. G가 루트 노드라면 검은색으로 바꾼다.
           1-b. G를 빨간색으로 바꿨을 때 또다시 red-red(double red)가 발생한다면 
                계속 상황에 맞춰 restructuring, recoloring을 진행해서 red-red 문제가 발생하지 않을 때까지 반복한다.
                (루트노도까지 진행해야할수도 있다.)
    ![img_1.png](img_1.png)

    위의 작업들은 <strong>[루트노드에서 모든 리프 노드까지의 경로에서 거치는 블랙노드의 개수가 같다.]</strong>라는
    규칙을 위배하지 않는다.
-----
## 삭제

1. 일반적인 BST 삭제를 수행한다.
   - 일반적인 BST 삭제 작업을 수행할 때 항상 리프이거나 하나의 자식만 있는 노드를 삭제하게 된다.
   - 내부 노드의 경우 후속 노드를 복사한 다음 후속 노드에 대해 재귀적으로 삭제를 호출하기때문
2. 삭제될 노드 A와 대체될 노드 B 중 하나가 빨간색일 경우: 교체된 노드를 검은색으로 칠함
3. 삭제될 노드 A와 대체될 노드 B 모두 검은 색인 경우:
   1. 대체될 노드를 삭제된 노드 A위치로 옮기고 더블 블랙이라고 표시한다.
   2. 더블 블랙 인 노드 B가 루트가 아닌동안 수행: (노드B의 형제 노드를 C라고한다.)
      1. C가 검은색이고 C의 자식중 적어도 하나가 빨간색이면 회전을 수행한다. (C의 빨간색 자식을 r로 설정)
         - case: L L (C와 r(왼쪽이거나 모든 자식)이 L L)
           1. 색깔 변경
           2. B의 부모노드를 기준으로 right rotate 수행
           
         - case: L R
           1. 색 변경
           2. C를 기준으로 left rotate 수행
           3. B의 부모노드를 기준으로 right rotate 수행

         - case: R R (r이 오른쪽 자식이거나 모든 자식)
           1. 색변경
           2. B의 부모노드를 기준으로 left rotate 수행
           
         - case: R L
           1. 색변경 
           2. C를 기준으로 right rotate 수행
           3. B의 부모노드를 기준으로 left rotate 수행
      2. C가 검은색이고 두 자식 모두 검은색인 경우
         - recoloring을 수행하고 C의 부모가 검은색이라면 부모에 대해 반복한다.
      3. C가 빨간색인 경우:
         - case C가 왼쪽: 부모노드를 기준으로 right rotate
         - case C가 오른쪽: 부모노드를 기준으로 left rotate
--------
## Node 클래스 
```java
public class Node {


    public Node parent;
    public Node left;
    public Node right;
    public boolean isBlack;

    public Node() {
        
    }

    public Node(Node parent, Node left, Node right, boolean b) {
        this.parent = parent;
        this.left = left;
        this.right = right;
        this.isBlack = b;
    }

}

```

## Red Black Tree 인터페이스
```java
// boolean isEmpty()
// boolean(or void) insert(Node node)
// boolean(or void) delete(Node node)
// void leftRoatate()
// void rightRotate()

```
