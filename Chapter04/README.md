## 이차문제

배열에 중복 값이 있는지 확인하는 자바스크립트 애플리케이션을 작성하고 있다고 해보자.

머릿속에 가장 먼저 떠오르는 방법 중 하나가 다음과 같이 중첩 for 루프를 사용하는 것이 아닐까 싶다.

```javascript
function hasDuplicateValue(array) {
    for(var i = 0; i < array.length; i++) {
        for(var j = 0; j < array.length; j++) {
            if(i ! == j && array[i] == array[j]) {
                return true;
            }
        }
    }
    return false;
}
```

결론적으로 배열에 원소 N개가 있을 때 함수는 N2번의 비교를 수행할 것이다. 바깥 루프는 배열을 전부 살펴보기 위해 무조건 N번을 순회해야 하고, 매 순회마다 안쪽 루프는 다시 N번을 순회해야 하기 때문이다. 이는 N단계 * N단계, 바꿔 말해 N2단계이므로 O(N2)의 알고리즘이다.

## 선형 해결법

다음처럼 중첩 루프를 쓰지 않는 hasDuplicateValue 함수를 구현해 볼 수 있다. 함수를 분석해서 첫 번째 구현보다 더 효율적인지 알아보자.

```javascript
function hasDuplicateValue(array) {
    var existingNumbers = [];
    for(var i = 0; i < array.length; i++) {
        if(existingNumbers[array[i]] === undefined) {
            existingNumbers[array[i]] = 1;
        } else {
            return true;
        }
    }
    return false;
}
```

위 구현에는 루프가 하나이며, 찾아봤던 수를 existingNumbers라는 배열에 기록해둔다. 함수가 이 배열을 사용하는 방식이 흥미롭다. 코드가 새로운 수를 찾을 때마다 이 수에 대응하는 existingNumbers 배열의 인덱스에 값 1을 저장하는 식이다.

예를 들어 [3, 5, 8]이라는 array가 있을 때, 루프가 종료된 후 existingNumbers 배열은 다음과 같을 것이다.

[undefined, undefined, undefined, 1, undefined, 1, undefined, undefined, 1]

인덱스 3, 5, 8에 1이 들어 있다. 즉, 주어진 array에 3, 5, 8이 있다는 뜻이다.

하지만 코드는 적절한 인덱스에 1을 저장하기 전에 이 인덱스에 이미 값 1이 들어 있는지 확인한다. 들어 있다면 이미 찾아봤던 수라는 의미이며 따라서 중복 값을 찾은 것이다.

결론적으로 이 새로운 구현을 빅 오 표기법으로 표현하면 O(N)이다.