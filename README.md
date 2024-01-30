# ECSDoc

알다시피, 기존 유니티 컴포넌트 시스템은 객체지향적, 즉 <span style="background-color:#fff5b1">OOP로 설계</span>되어있다. 유니티에서는 객체지향의 성격에 따라 각 컴포넌트들은 게임 오브젝트에 더해지는 방식으로 작동되었다.

이때 컴포넌트들은 메모리에 연속적으로 저장되는 것이 아니라 메모리 랜덤위치에 저장되어 게임 오브젝트와 연결이 된다. 이 방식은 <span style="background-color:#fff5b1">캐시 적중률이 낮아져 여러개의 오브젝트들이 쌓일수록 효율성이 떨어지고 만다.</span>

이러한 단점들을 해결하기 위해 유니티는 새로운 컴포넌트 시스템을 만들었다. 그것이 바로 **ECS(Entity Component System)** 이다.





<br/>
<br/>
<br/>
<br/>





# ECS(Entity Component System)

ECS은 객체지향이 아닌 <span style="background-color:#fff5b1">데이터 지향 기술 스택(DOTS)</span>으로 설계된 시스템이다.
여기서 E, C, S는 해당 시스템을 구성하는 요소라고 볼 수 있다.

<br/>
<br/>

### (1) Component
데이터 집합을 말한다. 
예를들어 Position 컴포넌트는 x 좌표, y좌표, z좌표의 '위치'데이터만 가질 것이다.

<br/>
<br/>


### (2) Entity
컴포넌트의 집합이다. 우리가 아는 게임 오브젝트를 엔티티라고 한다.
예를들어 큐브 엔티티는 Position, Rotation 컴포넌트를 가질 수 있을 것이다.


<br/>
<br/>


### (3) System
객체지향에서의 메소드의 개념이다. 
예를들어 moveSystem은 Position, Rotation 컴포넌트를 가진 엔티티들을 대상으로 키 입력시 Position 데이터를 변경할 것이다.

<br/>
<br/>

![image](https://github.com/NonnaKwon/ECSDoc/assets/89887999/66c509b0-a258-4515-8742-be10bbd2432d)

<br/>
<br/>

OOP에서는 행위와 상태를 같은 엔티티가 가지고 있었던것과 다르게, ECS에서의 엔티티는 단순히 데이터를 가리키는 인덱스일 뿐이다. 행위는 시스템이 결정한다.

그럼 여러가지 의문점이 든다. '아니, 그래서 _**데이터가 어떻게 저장되는건데?**_', '시스템이 _**어떻게 동작 하는건데?**_' 이 부분에 대해서 차례대로 살펴보자.






<br/>
<br/>
<br/>
<br/>




# 컴포넌트의 저장 방식

ECS에서의 컴포넌트의 저장 방식은 두 가지가 있다. 하나는 컴포넌트별로 묶어서 배열로 만든 방식인 **Sparse Set 방식**, 또 하나는 같은 컴포넌트 조합별로 묶은 방식인 **Archetype 방식**이다.

<br/>
<br/>

## (1) Sparse Set
![image](https://github.com/NonnaKwon/ECSDoc/assets/89887999/195169a2-11fb-4452-ac64-036bc3fabfe4)


<br/>
Sparse Set 방식은 컴포넌트 배열을 만들어 거기에 해당 id 엔티티의 데이터값을 저장한다.
이렇게 하면 아무리 많은 엔티티가 있더라도 for loop만 돌면 작동을 한다.

<br/>
![image](https://github.com/NonnaKwon/ECSDoc/assets/89887999/303eef1a-506b-4cbe-98de-ce935005435b)


<br/>

이렇게 하면 편하지만 문제는 데이터가 낭비되고 캐시 적중률이 낮다는 문제가 있다.


<br/>
<br/>
<br/>








## (2) Archetype

![image](https://github.com/NonnaKwon/ECSDoc/assets/89887999/da2a5a7b-138a-40f8-8055-4c9415611d21)

컴포넌트 묶음을 Chunk라고 했을 때, 같은 Chunk끼리 관리하는 것을 Archetype이라고 한다. 쉽게 말해서 Archetype은 같은 컴포넌트 조합별로 Packing해서 배열로 만든것이다.


