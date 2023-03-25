# OX퀴즈 - Programmers 코딩 테스트 입문

## 문제설명
덧셈, 뺄셈 수식들이 'X [연산자] Y = Z' 형태로 들어있는 문자열 배열 quiz가 매개변수로 주어집니다. 수식이 옳다면 "O"를 틀리다면 "X"를 순서대로 담은 배열을 return하도록 solution 함수를 완성해주세요.

<br>

## 제한사항
- 연산 기호와 숫자 사이는 항상 하나의 공백이 존재합니다. 단 음수를 표시하는 마이너스 기호와 숫자 사이에는 공백이 존재하지 않습니다.
- 1 ≤ quiz의 길이 ≤ 10
- X, Y, Z는 각각 0부터 9까지 숫자로 이루어진 정수를 의미하며, 각 숫자의 맨 앞에 마이너스 기호가 하나 있을 수 있고 이는 음수를 의미합니다.
- X, Y, Z는 0을 제외하고는 0으로 시작하지 않습니다.
- -10,000 ≤ X, Y ≤ 10,000
- -20,000 ≤ Z ≤ 20,000
- [연산자]는 + 와 - 중 하나입니다.

<br>

## 입출력 예시
~~~swift
solution(["3 - 4 = -3", "5 + 6 = 11"]) // ["X", "O"]
solution(["19 - 6 = 13", "5 + 66 = 71", "5 - 15 = 63", "3 - 1 = 2"]) // ["O", "O", "X", "O"]
~~~

<br>

## 문제풀이
~~~swift
func solution(_ quiz:[String]) -> [String] {
    var answer: [String] = []

    // 1. 퀴즈 배열 반복
    for i in quiz {
        
        // "=" 연산자를 기준으로 식과 값을 나눔
        var array = i.components(separatedBy: "=")

        // 값 앞뒤 공백 제거
        array[1] = array[1].trimmingCharacters(in: .whitespaces)

        // 식 정수형으로 변환할 수 있게 커스텀
        array[0] = array[0]
            .trimmingCharacters(in: .whitespaces)
            .replacingOccurrences(of: "- ", with: "-")
            .replacingOccurrences(of: "--", with: "+") // 연산자와 우변 모두 "-" 일 때 +로 변경
            .replacingOccurrences(of: "+ ", with: "")
            .replacingOccurrences(of: "+", with: "")

        // 값 더하기
        let sum = array[0]
            .components(separatedBy: " ")
            .map { Int($0)! }
            .reduce(0,+)

        // 최종 결과값 저장
        if sum == Int(array[1])! { answer.append("O") }
        else { answer.append("X") }
    }

    return answer
}
~~~

<br>

## 돌아보기
이전에 배운 메서드를 활용하기 위해 이리저리 고민하며 코드를 짰다. Switch 문으로 연산자를 걸러 계산하는 방법도 있는데 그 방법이 더 직관적이고 좋은 것 같다!