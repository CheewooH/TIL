# LevelTest
오늘 시험으로 배웠던것들을 다시 확인할수 있었다.
틀린것과 햇갈렸던 것들을 다시 확인할수있는 좋은 타이밍이라고 생각한다.

## 다음 중 C#의 자료형에 대한 내용 중 틀린 답을 고르시오. 
a. int는 정수형 자료형이며 4byte의 크기를 가진다.<br/>
b. float는 실수형 자료형이며 4byte의 크기를 가진다.<br/>
c. double은 실수형 자료형이며 8byte의 크기를 가진다. <br/>
d. char는 문자형 자료형이며 2byte의 크기를 가진다. <br/>
e. string은 문자열 자료형이며 최대 50byte의 크기를 가진다.<br/> 

다음 문제에서 자료형의 크기에 대해 알고있는지 확인할수 있었다. 나는 int는 정수형 float는 실수형
char와 string은 문자형 문자열 정도만 확인하고 크기를 기억하지 못했기 때문에 다시 한번 확인할수 있었다.
다른 자료형들은 크기가 지정을 받지만 string은 크기가 제한되어있지 않기때문에 e가 정답이다

## 다음 중 C#에서 허용하지 않은 문법을 고르시오. 
a. int intValue = 10; <br/> 
b. char charValue = ‘a’; <br/> 
c. float floatValue = 20.3; <br/> 
d. double doubleValue = 10.4; <br/> 
e. bool boolValue = true; <br/> 

이번 문제는 c와 d 사이에서 고민을했다 처음엔 20.3 실수형이니까 들어갈수있는거 아닌가? 라고 생각했지만
c의 20.3 은 기본적으로 double 타입으로 들어가서 float에 저장하려면 20.3f로 바꿨어야 했다. 정답은 c

## 다음 중 C#의 형변환으로 적합하지 않은 것을 고르시오. 
a. int intValue = (int)1.4; <br/>
b. float floatValue = (float)”12.3”;  <br/>
c. float floatValue = 3; <br/>
d. int intValue = int.Parse(“123”); <br/>
e. double doubleValue = 3.14f; <br/>

b의 "12.3"을 float로 변환하기 위해선 Parse 또는 TryParse를 사용해야 했다<br/>
float.TryParse("12.3", out floatValue);또는 float floatValue = float.Parse("12.3");를 해야한다.

## 다음 코드의 출력을 적으시오.
```cs
byte value = 255;
value++;
Console.Write(value);
```
문제를 보자마자 아 단순 ++를 물어보는게 아니구나 했지만 자료형의 크기를 외우지 못해 이문제도 틀렸다
답은 byte는 0 ~ 255 까지 저장할수있기 때문에 256을 출력할수없고 오버플로우가 발생하여 0이 출력된다.

## 다음 코드의 출력을 적으시오.
```cs
Console.Write(true && false || true);
```
처음에 뭘 어떻게 해도 true && false가 있으니까 false 밖에 못나오지 않나? 라고 생각했지만 순서대로 진행해보면 true && false = false 여기서 남은 false || true로 true가 출력된다.

## 다음 코드의 출력을 적으시오.
```cs
int value = 30;             // 30
value -= 10;            // 30 - 10 = 20 
value /= 2;             // 20 / 2 = 10
value *=3;              // 10 * 3 = 30
Console.WriteLine(value);   // 30
```
계산 실수..... 말이 안나온다

## 다음 코드의 출력을 적으시오.
```cs
for (int i = 128; i>=20; i /= 2)
{
    Console.Write(i);
}
```
1286432를 출력해야 하는데 문제를 제대로 보지않아 32만 적어 냈다...
## 다음 코드의 출력을 적으시오.
```cs
static void Swap(int left, int right)
{
    int temp = left;
    left = right;
    right = temp;
}
static void Main(string[] args)
{
    int left = 10;
    int right = 20;
    Swap(left, right);
    Console.WriteLine($"{left}{right}")
}
```
ref를 알고있는가의 문제였다. ref가 없으니 메인에서의 left right의 10,20은 Swap함수 안으로 들어는가지만
계산결과를 들고 나오지 않기때문에 결과는 1020이 출력된다.

## 아래의 코드의 출력을 적으시오.
```cs
int[] array = {1,2,3,4,5};
Console.WriteLine(array.Length);
```
array의 길이를 쓰면되는데 0부터 계산을 하여 4라고 썼다 정답은 5

## 아래의 코드의 출력을 적으시오.
```cs
int[,] matrix = 
{
    { 1,  2,  3,  4,  5},
    { 6,  7,  8,  9 ,10},
    {11, 12, 13, 14, 15},
    {16, 17, 18 ,19, 20},
};
```

이차원 배열 matrix[3,1] 은 y 3 x 1로 17이다 