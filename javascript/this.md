#Javascript의 this
##this는 전역 객체를 참조
전역 변수는 `this.(전역 변수)`로 적근 가능

##메소드를 호출할 때, 메소드 내부의 this는 해당 메소드를 호출한 부모 객체를 참조
```javascript
var val = 100;
var counter = {
   val: 0,
   increment: function() {
      this.val += 1;
   }
};
var inc = counter.increment;
inc();
console.log(counter.val);// 0
console.log(val); // 101
```
`counter.increment()`는 counter가 메소드를 호출한 부모 객체이며, increment메소드 내의 this는 counter를 참조

`inc()`는 전역에서 호출, this는 전역객체    

```javascript
var val = 100;
var counter = {
   val: 1,
   func1: function() {
      this.val += 1;
      console.log('func1() this.val: '+ this.val);       // func1() this.val: 2
      func2 = function() {
         this.val += 1;
         console.log('func2() this.val: '+ this.val);    // func2() this.val: 101
         func3 = function() {
            this.val += 1;
            console.log('func3() this.val: '+ this.val); // func3() this.val: 102
         }
         func3();
      }
      func2();
   }
};
counter.func1();
```
메소드 호출 위치가 중요한 것이 아님. 중첩된 구조의 내부에서 호출되더라도 **부모 객체가 명시되지 않으면** 전역에서 호출 한 것

```javascript
function Person(name) {
   this.name = name;
}
Person.prototype.getName = function() {
   return this.name;
}
var foo = new Person('foo');
console.log(foo.getName());              // foo
Person.prototype.name = 'person';
console.log(Person.prototype.getName()); // person
```
프로토타입 객체 메소드도 마찬가지로 **명시적으로 호출한 부모 객체**를 참조    
##생성자 함수를 호출할 때, 생성자 함수 코드 내부의 this는 새로 생성된 객체를 참조
```javascript
function F(v) {
   this.val = v;
}
var f = new F("constructor function");
console.log(f.val); // constructor function
```
##apply()와 call()메소드로 호출할 대, 함수의 this는 첫 번째 인자로 넘겨받는 객체를 참조