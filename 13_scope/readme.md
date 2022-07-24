# 13장 스코프

1. 스코프란?
    1. 스코프는 변수 그리고 함수와 깊은 관련
    2. 매개변수는 함수 몸체까지가 스코프
    3. **식별자가 유효한 범위**
        1. 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)자신이 선언된 위치에 의해 다른 코드가 참조할 수 있는 유효 범위
    4. 자바스크립트 엔진이 어떤 변수를 참조해야 할 지 결정하는 것을 **식별자 결정**
        1. 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙

    ```jsx
    var x = 'global';
    
    function foo() {
        var x = 'local';
        console.log(x); // 1
    }
    
    foo();
    
    console.log(x) // 2
    
    // local
    // global
    ```

    1. 스코프가 없다면
        1. 변수 충돌로 변수명을 하나밖에 사용할 수 없다
2. 스코프의 종류
    1. 전역과 전역 스코프
        1. 전역에 변수를 선언하면 전역 변수
        2. 전연 변수는 어디서든지 참조
    2. 지역과 지역 스코프
        1. 지역이란 함수 몸체 내부
        2. 지역에 변수를 선언하면 지역 스코프를 갖는 지역 변수
        3. 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에 유효

        ```jsx
        var x = 'g x';
        var y = 'g y';
        
        function outer() {
            var z = 'o l z';
        
            /**
             * g x
             * g y
             * o l z
             */
            console.log(x)
            console.log(y)
            console.log(z)
        
            function inner() {
                var x = 'i l x';
        
                /**
                 * i l x
                 * g y
                 * o l z
                 */
                console.log(x)
                console.log(y)
                console.log(z)
            }
        
            inner()
        }
        
        outer();
        
        try {
            /**
             * g x
             * ReferenceError: z is not defined
             */
            console.log(x)
            console.log(z)
        } catch (e) {
            console.log(e)
        }
        ```

3. 스코프 체인
    1. 함수는 중첩이 가능
    2. 지역 스코프도 중첩이 가능, 이는 스크프가 함수의 중첩에 의해 계층적 구조를 갖는다는 것을 의미
    3. 전역 스코프 ← 지역 스코프(← 지역 스코프)
        1. 스코프가 계층적으로 연결된 것을 **스코프 체인**이라 함
    4. 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 **변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동**하며 선언된 변수를 검색한다.
        1. 스코프 체인은 물리적인 실체로 존재
        2. 렉시컬 환경을 실제로 생성
        3. 변수 식별자는 렉시컬 환경에 key로 등록되고 변수 검색도 렉시컬 환경에서 이뤄짐
    5. 스코프 체인에 의한 변수 검색
        1. 자바스크립트 엔진은 스코프 체인을 따라 변수를 참조하는 코드의 스코프에서 시작해서 상위 스코프 방향으로 이동하면 선언된 변수를 검색
        2. 상위 스코프에서 유효한 변수는 하위 스코프에서 참조 가능, **하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없음을 의미**
    6. 스코프 체인에 의한 함수 검색
        1. 변수 검색과 같은 형태로 함수를 가리키는 식별자를 검색
        2. 따라서 변수를 검색할 대 사용하는 규칙이라고 표현하기보다는 **식별자를 검색하는 규칙**

        ```jsx
        function foo() {
            console.log('global')
        }
        
        function bar() {
            function foo() {
                console.log('local')
            }
            foo();
        }
        
        bar(); // local
        ```

4. 함수 레벨 스코프
    1. 지역 스코프는 코드 블록이 아닌 함수에 의해서 생성
    2. var 키워드로 선언된 변수는 함수 몸체만을 지역 스코프로 인정, 이러한 특성을 함수 례벨 스코프라 함
5. 렉시컬 스코프
    1. 두가지 패턴을 예측
        1. 함수를 **어디서 호출**했는지에 따라 상위 스코프 결정
            1. 동적 스코프
        2. 함수를 **어디서 정의**했는지에 따르 상위 스코프 결정
            1. 렉시컬 스코프, 정적 스코프
    2. 자바스크립트는 렉시컬 스코프
        1. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않음
        2. 함수의 상위 스코프는 함수 정의 시 정적으로 결정
        3. 함수 객체는 상위 스코프를 기억
        4. 클로저와 깊은 관계(24장 자세히)