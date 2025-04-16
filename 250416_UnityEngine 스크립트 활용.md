# UnityEngine 스크립트 활용

## Transform
- 게임오브젝트의 위치, 회적 크기를 저장하는 컴포넌트
- 게임오브젝트의 부모-자식 상태를 저장하는 컴포넌트
- 게임오브젝트는 반드시 하나의 트랜스폼 컴포넌트를 가지고 있으며 추가 & 제거 할수없음

```cs
[Serialize] Transform target;
[Range(0f,1f)]
[Serialize] float rate;

[Serialize] float moveSpeed;
[Serialize] float rotateSpeed;

[Serialize] Transform turret;
[Serialize] float rotate;

private void Update()
{
    #region Position
    // 1. 벡터를 이용한 위치 설정
    transform.position = targetPos;

    // 2. forward 방향으로 이동시키기
     transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);

    // 3. 목적지로 이동하기
     transform.position = Vector3.MoveTowards(transform.position, taget.position, moveSpeed * Time.deltaTime);
    // 4. 보간해서 이동시키기
    transform.position = Vector3.Lerp(source.position, taget.posistion, rate);
    #endregion

    // 1. 회전 직접 지정 : Euler를 이용하여 Quaternion으로 변환하여 사용 권장
    transform.rotation = Quaternion.Euler(0,60,0);
    // 2. 축을 기준으로 회전
    transform.Rotate(Vector3.forward, rotateSpeed * Time.deltaTime )
    // 3. 지점을 기준으로 회전
    transform.RotateAround(target.position, Vector3.up, rotateSpeed * Time.deltaTime)
    // 4. 지점을 바라보도록 회전
    transfrom.LookAt(target.position);

    // 1-1. 오일러를 쿼터니언으로 변환환 
    Quaternion a = Quaternion.Euler(0,60,0);
    // 1-2. 쿼터니언을 오일러로 변환
    Quaternion b = transfrom.rotation.eulerAngles;
    // 1-3. 방향백터를 쿼터니언으로 변환
    Quaternion C = Quaternion.LookRotation(Vector3.right);
    // 1-4. 현재 회전을 방향백터로 변환환
    Vecrtor3 d = transfrom.right;

    // 월드 기준
    Vector3 worldPosition = transform.position;
    Quaternion worldPosition = transform.position;

    // 로컬 기준
    Vector3 localPosition = transform.localPosition;
    Quaternion localPosition = transform.localPosition;


} 
```
## 위치 이동 구현
### Input Class

#### GetKey
단일 키 입력에 대한 여부를 반환받기 위한 함수.  
키가 눌리는 판정에 따라 세 가지 bool 타입 반환 함수가 있다.
```cs 
Input.GetKey()      // 키 입력이 진행되는 동안 true를 반환합니다.        누르고있는동안
Input.GetKeyDown()  // 키 입력이 시작되는 순간 1회만 true를 반환합니다.  눌렀을때 한번만
Input.GetKeyUp()    // 키 입력이 종료되는 순간 1회만 false를 반환합니다.  눌렀다 땠을때 한번만만
```
#### GetAxis
유니티에서 제공하는 Input Manager에 대응하는 코드.  
Input Manager
- 여러 장치의 입력을 입력매니저에 이름과 입력을 정의
- 입력매니저의 이름으로 정의한 입력의 변경사항을 확인
- 유니티 에디터의 Edit -> Project Settings -> Input Manager 에서 관리
- 단, 유니티 초창기의 방식이기 때문에 키보드, 마우스, 조이스틱에 대한 장치만을 고려함  

유니티 좌표상의 X(Horizontal), Z(Vertical)축에 해당하는 입력 데이터이며, 미 입력시 0, 방향과 입력에 따라 -1.0 ~ 1.0 사이의 float 타입을 반환한다다. 이 함수는 게임패드(조이스틱)와도 연동되는 코드이며, 숙련된다면 유용하게 사용할 수 있다.  

#### GetAxisRaw
GetAxis와 마찬가지로 Input Manager에 대응하고 유니티 좌표상의 X, Z축에 해당하는 입력 데이터를 -1, 0, 1로 반환한한다. 이는 키보드 입력에 대한 이동 구현에 적합한 함수.
```cs 
if (Input.GetButtonDown("Fire1"))
{
    Debug.Log("Fire1 버튼 눌림")
}
result = Input.GetAxisRaw("Horaizontal")

float axis = 0;
if(Input.GetKey(KeyCode.LeftArrow))
{
    asix -= 1;
}
if(Input.GetKey(KeyCode.RightArrow))
{
    asix += 1;
}
```
