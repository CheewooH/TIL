# UnityEngine 스크립트 활용
## 오브젝트 (Object)
게임 오브젝트란 게임 내에 존재하는 모든것이다, 캐릭터 몬스터 풀 나무 돌 지면 등..  
게임에서 오브젝트란 컴포넌트(기능) 를 담을수 있는 상자 라고 볼수있다.
그렇기에 물체가 가지는 기본적인 특징인 크기, 회전, 위치에 대한 정보를 가지고있다.
### Transform Component
위치, 회전각, 크기에 대한 3축(x, y, z)의 정보를 가지고 있다.
- Position : 위치에 대한 정보  
- Rotation : 회전각  
- Scale : 크기(배율)  

## 프리팹(Prefab) 관리
유니티에서의 프리팹은 미리 만들어둔 게임 오브젝트의 원본으로  
동일한 오브젝트를 여러번 재사용이 용이하게 도와주는 기능이다.
### 특징
- 반복적으로 사용할 오브젝트를 에디터에서 미리 설정하고 저장해둔다
- 원본 프리팹을 수정하면 모든 복제본에도 적용할수있어 유지보수가 좋다
## 생성과 파괴
규모가 증가할 수록 게임에서 다루는 오브젝트 종류가 많아지게 된다.  
게임 구동중 오브젝트를 생성하고 파괴해야 한다면 어떻게 해야할까?
```cs
// 오브젝트 생성 함수
// 게임 오브젝트 타입을 매개변수로 입력
Instantiate(GameObject Object);

// 오브젝트 파괴 함수
// 게임 오브젝트 타입을 매개변수로 입력
Destroy(GameObject Object);
```
위 함수를 사용해 오브젝트르 생성/파괴 할수있다. 하지만 반복적인 생성/파괴는 주의를 해야한다.  
모든 객체가 생성될때 메모리에는 해당 객체를 담기위한 공간이 할당이 된다.  
그리고 더이상 사용하지 않는(참조되지 않는) 객체에 대해 자동적으로 할당을 해주는것이 가비지 컬렉터의 기능이다.  
분명 편리한 기능이지만, 조건에 따라서는 메모리 단편화와 순환참조 객체미해제 같은 비효율적인 결과가 보이기도 한다.

## 오브젝트 풀 패턴 (Object Pool Pattern)
오브젝트 풀은 자주생성되고 파괴되는 오브젝트를 미리 만들어두고 재사용하는 디자인 패턴이다.
### 특징
- `Instantiate()`와 `Destroy()`를 반복하면 성능 저하 발생한다 → 이를 방지하기 위해 사용
- 미리 오브젝트들을 생성해서 풀(Pool)에 저장하고 필요할 때 꺼내서 사용
- 사용이 끝난 오브젝트는 파괴하지 않고 다시 풀에 반환한다.  
### 유니티가 지원하는 오브젝트 풀 기능 사용 예시
```cs
using UnityEngine;
using UnityEngine.Pool; // 유니티 공식 오브젝트 풀 기능을 사용하기 위한 네임스페이스
// 총알 스크립트
public class Bullet : MonoBehaviour 
{
    public IObjectPool<Bullet> pool; // 총알이 자신을 다시 풀에 반환하기 위해 참조하는 풀

    void OnDisable() 
    {
        pool.Release(this); // 총알이 비활성화될 때 풀로 반환
    }
}
// 총알 스폰 스크립트
public class BulletSpawner : MonoBehaviour 
{
    public Bullet bulletPrefab; // 생성할 총알의 프리팹

    IObjectPool<Bullet> bulletPool; // 총알을 관리할 풀

    void Awake() 
    {
        // 총알 풀 생성 (생성자 매개변수로 각각의 동작을 정의)
        bulletPool = new ObjectPool<Bullet>(
            CreateBullet,      // 새 오브젝트를 만들어야 할 때 호출
            OnGet,             // 오브젝트를 꺼낼 때 호출
            OnRelease,         // 오브젝트를 풀에 반환할 때 호출
            OnDestroyBullet,   // 풀 크기를 넘겨서 제거할 때 호출
            defaultCapacity: 10, // 초기 풀 크기
            maxSize: 50          // 최대 풀 크기
        );
    }

    Bullet CreateBullet() 
    {
        // 총알 생성 시 풀을 참조할 수 있도록 연결
        Bullet bullet = Instantiate(bulletPrefab);
        bullet.pool = bulletPool;
        return bullet;
    }

    void OnGet(Bullet bullet) 
    {
        bullet.gameObject.SetActive(true); // 총알을 꺼낼 때 활성화
    }

    void OnRelease(Bullet bullet) 
    {
        bullet.gameObject.SetActive(false); // 총알을 반환할 때 비활성화
    }

    void OnDestroyBullet(Bullet bullet) 
    {
        Destroy(bullet.gameObject); // 풀의 최대치를 넘으면 총알을 제거
    }

    public void Shoot() 
    {
        Bullet bullet = bulletPool.Get(); // 풀에서 총알을 꺼내옴
        bullet.transform.position = transform.position; // 총알 위치 설정
    }
}

```
