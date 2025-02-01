JSON Path 는 JSON Query Language
## JSON PATH Basic
### Root Document
![](images/JSON%20path-1.png)
`$` : Root (항상 들어가야 함)

### Dictionary
![](images/JSON%20path-2.png)
JSON path 의 모든 쿼리 결과는 대괄호 쌍 안에 캡슐화되어 있음.

### List
![](images/JSON%20path-3.png)

### Dict + List
![](images/JSON%20path-4.png)

## 조건 연산
![](images/JSON%20path-5.png)
![](images/JSON%20path-6.png)
조건 연산시 항상 대괄호로 둘러싸야함.
## Wildcard
![](images/JSON%20path-8.png)
![](images/JSON%20path-7.png)


### 실습
```shell
cat q9.json | jpath $.prizes[?(@.year == 2014)].laureates[*].firstname
```
![](images/JSON%20path-9.png)
위처럼 `(` 포함된 쿼리문을 `""` 따옴표로 묶지 않을 경우 오류 발생

## List 문법
### 범위 List
![](images/JSON%20path-11.png)
`[START : END]`  일 때 END의 index는 포함하지 않는 것에 주의

### List 역순 참조 (`-`)
![](images/JSON%20path-10.png)
`[-1]` 연산은 불가능.
- `[-1:0]`
- `[-1:]` 
두 옵션 사용