# UnityEngine 물리 및 충돌처리

## Rigidbody
Rigidbody 컴포넌트는 게임 오브젝트에 물리엔진을 적용하는 컴포넌트.  
물체의 질량, 공기저항, 중력에 대한 연산과 기본지원 함수를 사용해 보다 현실적인 물리처리가 가능하다.
### AddForce(x, y, z)
- 매개변수로 입력되는 x, y, z축 방향으로 물리적인 힘을 가한한다.  
입력이 반복될수록 적용되는 힘이 누적된다.
```cs
rigidbody.AddForce(Vector3.forceDirection, ForceMode.ForceModeType);
```
ForceMode 요약
- Force : 지속적인 **힘(Force)**을 가함, 질량 고려함
- Acceleration : 지속적인 **가속도(Acceleration)**를 가함, 질량 무시함
- Impulse : 순간적인 **힘의 충격(Impulse)**을 가함,	질량 고려함
- VelocityChange : 순간적인 **속도 변화(Velocity Change)**를 가함, 질량 무시함

### AddTorque()
- 매개변수로 입력되는 축의 물리적인 회전력을 더한다

### velocity
- 게임 오브젝트에 가해지고 있는 물리력

### angularVelocity
- 게임 오브젝트에 가해지고 있는 회전력

## 충돌처리
충돌처리는 Rigidbody와 Collider를 통해 이뤄집니다. 상황에 따라 IsTrigger 사용 여부를 선택하고 Collision과 Trigger로 사용한다.
### 충돌 Collision
- 충돌체 : 게임오브젝트의 물리적 충돌을 목적으로 모양을 정의
- 게임오브젝트 간의 충돌체로 부딪힘과 반발력을 처리
- 충돌체는 충돌상황에 있을 경우 유니티 충돌 메시지를 받아 상황을 확인
```cs
// <유니티 충돌 메시지>
        private void OnCollisionEnter(Collision collision)
        {
            Debug.Log("CollisionEnter");
        }

        private void OnCollisionStay(Collision collision)
        {
            Debug.Log("CollisionStay");
        }

        private void OnCollisionExit(Collision collision)
        {
            Debug.Log("CollisionExit");
        }
```
### Tirgger
- 하나의 충돌체가 충돌을 일으키지 않고 다른 충돌체의 공간에 들어가는 것을 감지
- 트리거는 겹침상황에 있을 경우 유니티 트리거 메시지를 받아 상황을 확인한다.
```cs
// <유니티 트리거 메시지>
        private void OnTriggerEnter(Collider other)
        {
            Debug.Log("TriggerEnter");
        }

        private void OnTriggerStay(Collider other)
        {
            Debug.Log("TriggerStay");
        }

        private void OnTriggerExit(Collider other)
        {
            Debug.Log("TriggerExit");
        }
```
## Layer 활용
Layer는 많은 오브젝트를 그룹으로 묶어 계층 단위로 관리하기 위한 기능이다. tag와 사용법은 비슷하지만,  
여러 오브젝트를 묶어서 기능적으로 관리할 수 있기 때문에 물리/충돌처리에 유용하게 활용할 수 있다.

## 레이캐스트 (Raycast)
Unity에서 Raycast는 특정 방향으로 광선을 쏘아 객체와의 충돌을 감지하는 기술  
클릭, 캐릭터 시야, 등 다방면으로 사용
```cs
Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition); // 마우스 위치를 기준으로 광선 생성
RaycastHit hit;
                 // 위치  맞은곳     사거리 
if (Physics.Raycast(ray, out hitInfo, 10f)) 
{
    Debug.Log("Hit: " + hit.collider.gameObject.name);
}
```
### 주요 매개변수
- origin: 광선 시작점
- direction: 광선 방향
- maxDistance: 광선의 최대 거리
- layerMask: 특정 레이어와만 충돌하도록 설정 가능

# 그 외 
physics raycaster, ipointer handler 를 찾아서 공부해보자자