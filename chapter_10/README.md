## 10. 조건부 로직 간소화

> 조건부 로직은 프로그램을 복잡하게 하는 주요 원흉이다.

### 10.1 Decompose Conditional
조건문 분해하기

> 조건식과 그 조건식에 딸린 조건절을 각각 함수로 추출하는 기법

```javascript
// before
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd))
  charge = quantity * plan.summerRate;
else
  charge = quantity * plan.regularRate + plan.regularServiceCharge;

//after
if (summer())
  charge = summerCharge();
else
  charge = regularCharge();
```

### 10.2 Consolidate Conditional Expression 
조건식 통합하기
> 비교 조건은 다르지만, 그 결과로 수행하는 동작은 똑같은 경우 조건 검사를 하나로 합치는 게 낫다.

절차
- 조건식에 부수 효과가 없어야 한다.
- 조건문 두 개를 선택하여 논리 연산자로 결합한다.

#### or 연산자 사용하기
```javascript
// before
if (anEmployee.seniority < 2) return 0;
if (anEmployee.monthsDisabled > 12) return 0;
if (anEmployee.isPartTime) return 0;

// after
if (isNotEligableForDisability()) return 0;

function isNotEligableForDisability() {
    return ((anEmployee.seniority < 2)
        || (anEmployee.monthsDisabled > 12)
        || (anEmployee.isPartTime));
}
```

### 10.3 Replace Nested Conditional with Guard Clauses
중첩 조건문을 보호 구문으로 바꾸기

> if then-else 구조와 보호 구문은 다르다. 
> 
> if else 절은 똑같은 중요성을 각 구문에 부여하지만,
> 
> 보호 구문은 "이 조건은 함수의 핵심이 아니며, 함수에서 빠져나오라"는 의도를 전달한다.

<details><summary>Guard clause (보호 구문)?
</summary>

*비정상 조건일 경우 함수를 종료하는 경우를 뜻한다.* 
</details>


```javascript
/** before 
 * 이 함수가 진짜 의도한 일은 모든 조건이 거짓일 때만 실행된다.
 * 이 상황에서는 보호구문을 사용하면 의도가 더 잘 드러난다.
 * */
function getPayAmount() {
    let result;
    if (isDead)
        result = deadAmount();
    else {
        if (isSeparated)
            result = separatedAmount();
        else {
            if (isRetired)
                result = retiredAmount();
            else
                result = normalPayAmount();
        }
    }
    return result;
}

// after
function getPayAmount() {
    if (isDead) return deadAmount();
    if (isSeparated) return separatedAmount();
    if (isRetired) return retiredAmount();
    return normalPayAmount();
}
```


### 10.4 Replace Conditional with Polymorphism
조건부 로직을 다형성으로 바꾸기

<details><summary>Polymorphism?
</summary>

*다형성은 객체 지향 프로그래밍의 4가지 개념 중 하나로, 
특정 기능을 선언(설계)부분과 구현(동작)부분으로 분리한 후 구현부분을 다양한 방법으로 만들어
선택하여 사용할 수 있게 하는 기능이다.
하나의 변수가 다양한 종류의 클래스로 만든 여러 객체를 가리킬 수 있다.*

https://anerim.tistory.com/78
</details>

```javascript
// before
switch(bird.type) {
    case 'EuropeanSwallow':
  return "보통이다";
    case 'AfricanSwallow':
  return (bird.numberOfCoconuts > 2) ? "지쳤다" : "보통이다";
    case 'NorwegianBlueParrot':
  return (bird.voltage > 100) ? "그을렸다" : "예쁘다";
 default:
  return "알 수 없다";
}

// after
class EuropeanSwallow {
 get plumage() {
  return "보통이다";
 }}
 
 class AfricanSwallow {
 get plumage() {
  return (bird.numberOfCoconuts > 2) ? "지쳤다" : "보통이다";
 }}

 class NorwegianBlueParrot {
 get plumage() {
  return (bird.voltage > 100) ? "그을렸다" : "예쁘다";
 }
}
```