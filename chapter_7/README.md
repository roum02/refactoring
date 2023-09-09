### Encapsulate Record

> Record
> 1) 필드 이름 노출
> 2) 필드 이름 숨겨, 내가 원하는 이름 사용 - hash, map, dictionary etc


### 7.6 Inline Class
> 1. 더 이상 제 역할을 못 하는 클래스를 가장 많이 사용하는 클래스로 흡수시키기
> 2. 두 클래스의 기능을 지금과 다르게 배분하고 싶을 때
> 
c.f 7.5 Extract Class 의 반대

```javascript
// before
class TrackingInformation {
  // ...
  get shippingCompany() {
    return this._shippingCompany;
  }
  set shippingCompany(arg) {
    this._shippingCompany = arg;
  }
  get trackingNumber() {
    return this._trackingNumber;
  }
  set trackingNumber(arg) {
    this._trackingNumber = arg;
  }
  get display() {
    return `${this.shippingCompany}: ${this.trackingNumber}`;
  }
}

class Shipment {
  // ...
  get trackingInfo() {
    return this._trackingInformation.display;
  }
  get trackingInformation() {
    return this._trackingInformation;
  }
  set trackingInformation(aTrackingInformation) {
    this._trackingInformation = aTrackingInformation;
  }
}
```

1. 위임 함수 생성
```javascript
set shippingCompany(arg) {
    this._shippingCompany = arg;
}
```
2. 클라이언트에서 위임 함수 호출

```javascript
class Shipment {
  get trackingInfo() {
    return `${this.shippingCompany}: ${this.trackingNumber}`;
  }
  get shippingCompany() {
    return this._shippingCompany;
  }
  set shippingCompany(arg) {
    this._shippingCompany = arg;
  }
  get trackingNumber() {
    return this._trackingNumber;
  }
  set trackingNumber(arg) {
    this._trackingNumber = arg;
  }
}
```


### 7.7 Hide Delegate
> 위임 객체의 인터페이스가 바뀌더라도, 클라이언트에서 코드를 수정할 일이 없도록 
> 
> 위임 매서드를 만들고 위임 객체의 존재를 숨겨, 의존성을 없애는 기법.
> 
c.f. 반대: 중개자 제거하기

```javascript
// before
class Person {
  constructor(name) {
    this._name = name;
  }
  get name() { return this._name; }
  get department() { return this._department; }
  set department () { this._department = arg; }
}

class Department {
  get chargeCode() { return this._chargeCode; }
  set chargeCode() { this._chargeCode = arg; }
  get manager() { return this._manager; }
  set manager() { this._manager = arg; }
}

// 클라이언트
const manager = aPerson.department.manager;
```

의존성을 줄이기 위해,
클라이언트가 부서 클래스를 볼 수 없게 숨기고,
대신 사람 클래스에 위임 메서드를 만든다.

```javascript
// after
class Person {
  // ...
  get manager() {
    return this._department.manager;
  }
}

// 클라이언트
const manager = aPerson.manager;
```


### 7.8 Remove Middle Man
> 
c.f. 중개자 제거하기