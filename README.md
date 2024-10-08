# stroke와 border

# `stroke`

`stroke` 는 `Shape`의 extension에 정의 되어 있으며,

```swift
Traces the outline of this shape with a color or gradient.
```

를 하는 역할이다.

주로(style을 지정하는 방법도 있으나 간단한 내용만 담음)

- **Parameters**:

///   - content: The color or gradient with which to stroke this shape.

///   - lineWidth: The width of the stroke that outlines this shape.

이렇게 2개의 parameter를 받아서 직관적인 테두리를 두르는데

테두리를 그릴 때 linewidth의 중앙과 원본 도형의 중앙이 딱 맞게 그려진다.

즉 원래 도형보다 더 크게 보일 수 있다. 자세한 건 뒤에서 border와의 비교를 통해 보는 편이 낫다.

그리고 `Shape` 의 extension에 다른 하나가 `fill`인데 이 때문인지 기본적으로 fill을 지정하지 않고 storke만 사용하면 도형 내부에 색을 채워넣지 않는다.

# `border`

`border` 는 View의 extension에 정의 되어 있으며,

```swift
Adds a border to this view with the specified style and width.
```

를 하는 역할이다.

중요한 점은

```swift
By default, the border appears inside the bounds of this view.
```

즉 도형의 원래 사이즈를 벗어나지 않고 안쪽으로 width 만큼 커지는 두께를 갖는 테두리를 그린다는 점이다.

- **Parameters**:

///   - content: A value that conforms to the ``ShapeStyle`` protocol, like a ``Color`` or ``HierarchicalShapeStyle``, that SwiftUI  uses to fill the border.

///   - width: The thickness of the border. The default is 1 pixel.

이렇게 2개의 parameter를 받는다.

# `본격 비교!`

Xcode의 playground를 활용해서 간단하게 비교해봤다.

![스크린샷 2024-10-08 오후 2 53 51](https://github.com/user-attachments/assets/2deed8bc-f1b5-497d-bed9-a4c3058b112e)

```swift
import SwiftUI
import PlaygroundSupport

struct ContentView: View {
    var body: some View {
        VStack {
            HStack(spacing: 0) {
                VStack {
                    Rectangle()
                        .stroke(.red.opacity(0.5), lineWidth: 20)
                        .frame(width: 100, height: 100)
                    Text("Stroke")
                }
                Spacer().frame(width: 20)
                VStack {
                    Rectangle()
                        .border(.red.opacity(0.5), width: 20)
                        .frame(width: 100, height: 100)
                    Text("Border")
                }
            }
            .background(.green)
            .padding(20)
        }
        .background(.yellow)
    }
}

PlaygroundPage.current.setLiveView(ContentView())
```

이 코드에서 중요하게 볼 점은

## <storke는 원래 도형의 테두리 선과 결을 같이한다.>

1. 가장 외부의 VStack의 background는 노란색💛 이고
2. 그 안의 HStack의 경우 background로 초록색💚 을 갖는다.
3. 그리고 그 안쪽 HStack이 모든 방향으로 20만큼의 padding을 가지기 때문에
    
![스크린샷 2024-10-08 오후 2 53 51 (1)](https://github.com/user-attachments/assets/97bdef72-3234-4f58-bdcd-ee4a1e92c26a)
    
    이 사진에서 가운데 위쪽 파랑색 선만큼의 길이가 20이라는 것이다.
    

👀 이제 보일 것이다. 파랑색 선의 중간만큼(10) `Stroke`가 튀어나와 있고 이는 곧 `Storke`가 그 width의 반(20의 반인 10)만큼을 원래 도형의 밖으로 그려냈다는 것이다.

## <stroke는 그 자체로는 도형 내부의 색을 채우지 않는다.>

옆의 border의 Rectangle의 경우 안쪽이 검정색으로 채워졌다. 이는 Rectangle의 기본 속성이다.

아래 코드의 결과가

```swift
import SwiftUI
import PlaygroundSupport

struct ContentView: View {
    var body: some View {
        VStack {
            HStack(spacing: 0) {
                VStack {
                    Rectangle()
                        .frame(width: 100, height: 100)
                    Text("Nothing on Rectangle")
                }
            }
            .background(.green)
            .padding(20)
        }
        .background(.yellow)
    }
}

PlaygroundPage.current.setLiveView(ContentView())

```

이런 이미지로 보여지는 게 그 방증이다. ~~(테두리 선은 양해 좀…)~~

![스크린샷 2024-10-08 오후 3 01 42](https://github.com/user-attachments/assets/b56c02b9-2ed0-4f4d-a814-a23b9307be1b)

그런데 stroke의 경우 그 뒤의 배경색인 초록색이 그대로 보이고 있다.

이는 “stroke는 그 자체로는 도형 내부의 색을 채우지 않는다.”라는 증거다.
