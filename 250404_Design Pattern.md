# 디자인 패턴 이해와 기본 원칙
디자인 패턴은 상황에 따른 도구일뿐 도구를 사용하기위해 상황을 만드는 상황은 좋지 못할수있다  

## 팩토리 패턴
객체 생성을 한곳에서 관리할수 있어 코드의 유지보수와 확장성이 증가한다
```cs
internal class Program
{   
    static void Main(string[] args)
    {
        // 스테이지 1-1
        MonsterFactory level1Factory = new MonsterFactory();
        level1Factory.rate = 1;

        // 1. 처음 맵 만들어 졌을 때 -> 몬스터 3마리
        Monster monster11 = level1Factory.Create("슬라임");
        Monster monster33 = level1Factory.Create("슬라임");
        Monster monster55 = level1Factory.Create("슬라임");

        // 2. 다음 장소로 이동했을 때 -> 몬스터 5마리
        Monster monster1 = level1Factory.Create("슬라임");
        Monster monster2 = level1Factory.Create("슬라임");
        Monster monster3 = level1Factory.Create("슬라임");
        Monster monster4 = level1Factory.Create("고블린");
        Monster monster5 = level1Factory.Create("고블린");

        // 3. 보스룸 입장하면 -> 몬스터 2마리
        Monster bossMonster = level1Factory.Create("오크족장");
        Monster subMonster1 = level1Factory.Create("고블린");
        Monster subMonster2 = level1Factory.Create("고블린");


        // 스테이지 2-1
        MonsterFactory level2Factory = new MonsterFactory();
        level2Factory.rate = 1.1f;
        Monster level2monster1 = level2Factory.Create("슬라임");
        Monster level2monster2 = level2Factory.Create("슬라임");

    }
}
public class HardCoreFactory
{
    public Monster Create(string name)
    {
        Monster monster;
        switch (name)
        {
            case "슬라임": monster = new Monster("슬라임", 1, 1); break;
            case "고블린": monster = new Monster("고블린", 3, 1); break;
            case "오크족장": monster = new Monster("오크족장", 10, 1); break;
            default: return null;
        }
        return monster;
    }
}

public class MonsterFactory
{
    public float rate;

    public Monster Create(string name)
    {
        Monster monster;
        switch (name)
        {
            case "슬라임": monster = new Monster("슬라임", 1, 100); break;
            case "고블린": monster = new Monster("고블린", 3, 200); break;
            case "오크족장": monster = new Monster("오크족장", 10, 2000); break;
            default: return null;
        }

        monster.hp = (int)(monster.hp * rate);
        return monster;
    }
}

public class Monster
{
    public string name;
    public int level;
    public int hp;

    public Monster(string name, int level, int hp)
    {
        this.name = name;
        this.level = level;
        this.hp = hp;
    }
}
```
## 메서드 체이닝 (Method Chaining)
객체의 메서드를 연속해서 호출할 수 있도록 반환값을 객체 자신으로 설정하는 기법  
```cs
internal class Program
{
    static void Main(string[] args)
    {
        GameObject gameObject = new GameObject();

        gameObject.SetX(10).SetY(5).SetZ(3); // 코드를 간결하고 가독성 좋게 만들 수 있음
        
    }
}
public class GameObject
{
    public int x;
    public int y;
    public int z;
    public GameObject SetX(int x)
    {
        this.x = x;
        return this;
    }
    public GameObject SetY(int y)
    {
        this.y = y;
        return this;
    }
    public GameObject SetZ(int z)
    {
        this.z = z;
        return this;
    }
}
```
## 빌더 패턴 (Builder Pattern)
복잡한 객체들을 단계별로 생성할 수 있도록 도와주는 디자인 패턴  
C#에서도 stringBuilder 기능이 빌더패턴을 사용중이다
```cs
    internal class Program
    {
        static void Main(string[] args)
        {
            // 빌더
            MonsterBuilder orcArcherBuilder = new MonsterBuilder();
            orcArcherBuilder
                .SetName("오크 궁수")
                .SetWeapon("나무 활")
                .SetArmor("가죽 갑옷");
            MonsterBuilder orcWarriorBuilder = new MonsterBuilder(); 
            orcWarriorBuilder
                .SetName("오크 전사")
                .SetWeapon("나무 몽둥이")
                .SetArmor("철 갑옷");

            Monster monster0 = orcArcherBuilder.Build();  
            Console.WriteLine($"이름 : {monster0.name}, 무기 : {monster0.weapon} 갑옷 : {monster0.armor}");

            Monster monster1 = orcWarriorBuilder.Build();
            Console.WriteLine($"이름 : {monster0.name}, 무기 : {monster0.weapon} 갑옷 : {monster0.armor}");

        }
    }
    public class MonsterBuilder
    {
        public string Name;
        public string Weapon;
        public string Armor;

        public MonsterBuilder()
        {
            Name = "몬스터";
            Weapon = "기본무기";
            Armor = "기본갑옷";
        }
        public Monster Build()
        {
            Monster monster = new Monster();
            monster.name = Name;
            monster.weapon = Weapon;
            monster.armor = Armor;

            return monster;
        }
        public MonsterBuilder SetName(string name)
        {
            this.Name = name;
            return this;
        }
        public MonsterBuilder SetWeapon(string weapon)
        {
            this.Weapon = weapon;
            return this;
        }
        public MonsterBuilder SetArmor(string armor)
        {
            this.Armor = armor;
            return this;
        }
    }
    public class Monster
    {
        public string name;
        public string weapon;
        public string armor;
    }
```


