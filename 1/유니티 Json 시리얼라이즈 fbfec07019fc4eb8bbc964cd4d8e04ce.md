# 유니티 Json 시리얼라이즈

## Json 직렬화

유니티에서 JSON을 시리얼라이즈(직렬화)하는 작업은 **`JsonUtility`**를 사용하여 쉽게 처리할 수 있습니다. 여기서는 간단한 클래스를 정의하고, 그 인스턴스를 JSON 문자열로 시리얼라이즈하는 기본적인 예제를 제공하겠습니다.

먼저, 시리얼라이즈할 클래스를 정의해야 합니다. **`System.Serializable`** 어트리뷰트를 클래스에 추가해야 **`JsonUtility`**가 이 클래스를 시리얼라이즈할 수 있습니다.

```csharp
using System;
using UnityEngine;

// 시리얼라이즈될 클래스 정의
[Serializable]
public class PlayerData
{
    public string name;
    public int level;
    public float health;
}
```

다음으로, **`PlayerData`** 클래스의 인스턴스를 생성하고, 이를 JSON 문자열로 변환하는 방법을 보여줍니다. 이 예제에서는 유니티의 **`Start`** 메서드 안에서 이 작업을 수행합니다.

```csharp
using UnityEngine;

public class JsonExample : MonoBehaviour
{
    void Start()
    {
        // PlayerData 인스턴스 생성
        PlayerData player = new PlayerData
        {
            name = "JohnDoe",
            level = 5,
            health = 76.5f
        };

        // JsonUtility를 사용하여 인스턴스를 JSON 문자열로 시리얼라이즈
        string json = JsonUtility.ToJson(player);

        // 결과 출력
        Debug.Log(json);
    }
}
```

이 코드를 유니티 프로젝트에 추가하고 실행하면, **`PlayerData`** 객체가 JSON 문자열로 변환되어 콘솔에 출력됩니다. 출력된 JSON 문자열은 다음과 같은 형태를 가질 것입니다:

```json
{"name":"JohnDoe","level":5,"health":76.5}
```

이 예제는 기본적인 시리얼라이즈 과정을 보여줍니다. 복잡한 데이터 구조나 다양한 요구 사항에 맞게 **`JsonUtility`**의 사용법을 확장할 수 있습니다. 예를 들어, 배열이나 리스트도 시리얼라이즈할 수 있으며, 필드에 **`[NonSerialized]`** 어트리뷰트를 추가하여 특정 필드를 JSON 변환 과정에서 제외시킬 수 있습니다.

유니티에서 JSON 문자열로부터 데이터를 역직렬화(디시리얼라이즈)하여 객체로 변환하는 과정도 **`JsonUtility`**를 사용하여 간단히 수행할 수 있습니다. 위에서 정의한 **`PlayerData`** 클래스를 사용하여 JSON 문자열을 다시 객체로 변환하는 예제를 작성하겠습니다.

먼저, 역직렬화할 JSON 문자열이 필요합니다. 이 문자열은 **`PlayerData`** 클래스의 인스턴스를 표현해야 하며, 아래 예제에서는 하드코딩된 JSON 문자열을 사용합니다.

```csharp
using UnityEngine;

public class JsonExample : MonoBehaviour
{
    void Start()
    {
        // JSON 문자열
        string json = "{\"name\":\"JohnDoe\",\"level\":5,\"health\":76.5}";

        // JsonUtility를 사용하여 JSON 문자열에서 PlayerData 객체로 역직렬화
        PlayerData player = JsonUtility.FromJson<PlayerData>(json);

        // 결과 확인
        Debug.Log("Player Name: " + player.name);
        Debug.Log("Level: " + player.level);
        Debug.Log("Health: " + player.health);
    }
}
```

이 코드는 **`PlayerData`** 클래스에 해당하는 JSON 문자열을 객체로 변환합니다. **`JsonUtility.FromJson<T>`** 메서드는 제네릭 메서드로, 역직렬화할 객체의 타입을 지정합니다. 이 예제에서는 **`PlayerData`** 타입의 객체를 생성하여 JSON에서 읽어온 데이터로 채웁니다.

이 스크립트를 유니티에서 실행하면, JSON 문자열로부터 읽은 데이터를 사용하여 **`PlayerData`** 객체를 생성하고, 해당 객체의 필드 값들이 콘솔에 출력됩니다. 출력은 다음과 같이 나타날 것입니다:

```yaml
Player Name: JohnDoe
Level: 5
Health: 76.5
```

이 방식을 사용하여 파일, 웹 API 등에서 가져온 JSON 데이터를 유니티 객체로 쉽게 변환할 수 있습니다. 이는 게임 개발에서 설정, 세이브 데이터, 게임 리소스 관리 등 다양한 용도로 활용될 수 있습니다.

Unity에서 복잡한 데이터 구조를 포함하는 객체를 JSON으로 시리얼라이즈할 때는 **`System.Serializable`** 어트리뷰트를 사용하여 클래스를 정의해야 합니다. 이 때, 내부에 다른 객체나 배열 등을 포함할 수 있습니다. 다음은 이러한 구조를 갖는 예제 코드입니다. 이 예제에서는 **`PlayerData`** 객체가 **`InventoryItem`** 객체 배열을 포함하고 있습니다.

### **1. 정의할 클래스들**

먼저, **`PlayerData`** 클래스와 **`InventoryItem`** 클래스를 정의합니다. **`InventoryItem`** 클래스는 각 아이템의 이름과 수량을 저장합니다.

```csharp
using System;
using UnityEngine;

[Serializable]
public class InventoryItem
{
    public string itemName;
    public int quantity;
}

[Serializable]
public class PlayerData
{
    public string name;
    public int level;
    public float health;
    public InventoryItem[] inventory; // InventoryItem 배열을 포함
}
```

### **2. 시리얼라이즈 예제**

이제 **`PlayerData`** 객체를 생성하고, 해당 객체를 JSON 문자열로 시리얼라이즈하는 코드를 작성합니다. **`inventory`** 배열에 몇 가지 **`InventoryItem`** 인스턴스를 추가하여 객체를 더욱 복잡하게 만듭니다.

```csharp
using UnityEngine;

public class SerializeExample : MonoBehaviour
{
    void Start()
    {
        // PlayerData 인스턴스 생성
        PlayerData player = new PlayerData
        {
            name = "JohnDoe",
            level = 10,
            health = 100.0f,
            inventory = new InventoryItem[]
            {
                new InventoryItem { itemName = "Sword", quantity = 1 },
                new InventoryItem { itemName = "Health Potion", quantity = 3 }
            }
        };

        // JsonUtility를 사용하여 인스턴스를 JSON 문자열로 시리얼라이즈
        string json = JsonUtility.ToJson(player, true); // true로 설정하여 JSON을 보기 좋게 포맷

        // 결과 출력
        Debug.Log(json);
    }
}
```

### **JSON 출력 예시**

이 스크립트를 실행하면 다음과 같은 JSON 문자열이 콘솔에 출력됩니다:

```json
{
  "name": "JohnDoe",
  "level": 10,
  "health": 100.0,
  "inventory": [
    {
      "itemName": "Sword",
      "quantity": 1
    },
    {
      "itemName": "Health Potion",
      "quantity": 3
    }
  ]
}
```

이 예제는 Unity에서 복잡한 객체 구조를 JSON으로 쉽게 시리얼라이즈하는 방법을 보여줍니다. **`JsonUtility`**를 사용하여 간단히 처리할 수 있으며, 이를 통해 게임 설정, 세이브 데이터, 네트워크 통신 등 다양한 용도로 활용할 수 있습니다.

## 유니티에서 텍스트 파일 연동하기

유니티에서 외부 텍스트 리소스를 불러와 사용하는 방법은 여러 가지가 있지만, 주로 사용되는 두 가지 방법은 **`TextAsset`** 변수를 이용하거나 **`Resources`** 폴더를 사용하는 것입니다. 이 두 방법을 비교해보겠습니다.

### **1. `TextAsset` 변수 사용하기**

**`TextAsset`**를 사용하는 방법은 주로 에디터 내에서 리소스를 직접 할당할 때 사용됩니다. 예를 들어, Unity 에디터에서 직접 스크립트 컴포넌트에 텍스트 파일을 드래그 앤 드롭하여 할당할 수 있습니다.

```csharp
using UnityEngine;

public class TextAssetExample : MonoBehaviour
{
    // Inspector에서 직접 할당
    public TextAsset textFile;

    void Start()
    {
        if (textFile != null)
        {
            // 텍스트 파일 내용 출력
            Debug.Log(textFile.text);
        }
    }
}
```

이 방법은 주로 개발 중에 텍스트 파일을 쉽게 할당하고 관리할 수 있도록 해주며, 특히 텍스트 파일의 내용이 자주 바뀌지 않을 때 유용합니다.

### **2. `Resources` 폴더 사용하기**

**`Resources`** 폴더를 사용하는 방법은 런타임 중에 텍스트 파일을 동적으로 불러올 필요가 있을 때 사용됩니다. **`Resources`** 폴더 안에 텍스트 파일을 저장하고, **`Resources.Load`** 메소드를 사용하여 불러옵니다.

```csharp
using UnityEngine;

public class ResourcesExample : MonoBehaviour
{
    void Start()
    {
        // "Resources" 폴더 내의 텍스트 파일 불러오기
        TextAsset textFile = Resources.Load<TextAsset>("myTextFile");

        if (textFile != null)
        {
            // 텍스트 파일 내용 출력
            Debug.Log(textFile.text);
        }
    }
}
```

이 파일(**`myTextFile.txt`**)은 유니티 프로젝트 내 **`Assets/Resources`** 경로 안에 위치해야 합니다. 이 방법은 파일을 프로그래밍적으로 불러와야 할 때 유용하며, 파일을 변경하거나 다른 파일로 교체하는 것이 자주 일어날 때 특히 유리합니다.

### **비교**

- **접근성**: **`TextAsset`** 변수는 개발자가 직접 에디터에서 할당하기 때문에 접근이 쉽고 직관적입니다. 반면, **`Resources`** 폴더는 파일명만 알고 있다면 언제든지 스크립트로 불러올 수 있으며, 다양한 파일을 동적으로 처리할 수 있습니다.
- **유연성**: **`Resources`** 폴더는 파일을 동적으로 불러올 수 있는 큰 장점이 있지만, **`Resources`** 시스템은 빌드 시 모든 리소스가 포함되므로 사용하지 않는 리소스라도 메모리를 차지할 수 있습니다. **`TextAsset`**는 필요한 파일만 할당하여 관리할 수 있습니다.
- **사용 용도**: 작은 규모의 프로젝트나 특정 리소스를 고정적으로 사용할 때는 **`TextAsset`**이 적합할 수 있고, 다양한 리소스를 런타임 중에 불러와야 하는 경우에는 **`Resources`** 폴더가 더 적합할 수 있습니다.

개발 요구 사항에 따라 가장 적합한 방법을 선택하여 사용하면 됩니다.

## 직렬화 이슈

Unity에서 JSON으로 데이터를 직렬화할 때 **`GameObject`**와 같은 Unity의 내부 객체들은 직렬화 대상에서 제외되는데, 이에는 몇 가지 주요한 이유가 있습니다.

### **1. 객체의 복잡성**

Unity의 **`GameObject`**는 매우 복잡한 객체입니다. **`GameObject`**는 다양한 컴포넌트들(**`Transform`**, **`Renderer`**, **`Collider`** 등)을 포함할 수 있으며, 이러한 컴포넌트들은 각기 다른 데이터와 상태를 가지고 있습니다. 이 컴포넌트들은 간단한 값 형식이 아니라, 깊은 계층적 구조와 상호 참조를 포함할 수 있습니다. 이러한 복잡성을 단순한 JSON 형식으로 표현하는 것은 매우 어렵습니다.

### **2. 성능 및 메모리 문제**

**`GameObject`**와 같은 복잡한 객체를 직렬화하려면 상당한 양의 계산과 메모리가 필요합니다. 게임의 성능에 영향을 미칠 수 있으며, 특히 큰 씬이나 많은 객체를 포함하는 게임에서는 더욱 그렇습니다. 게임의 실시간 성능을 유지하기 위해서는 이러한 과정을 최소화하는 것이 중요합니다.

### **3. 상호 의존성**

**`GameObject`**들은 서로 간에 복잡한 관계를 가질 수 있습니다. 예를 들어, 한 **`GameObject`**가 다른 **`GameObject`**를 참조하거나 여러 **`GameObject`**들이 동일한 리소스를 공유할 수 있습니다. 이러한 상호 의존성을 JSON으로 적절히 표현하고 다시 복원하는 것은 매우 어려운 작업입니다.

### **4. 유니티 에디터와 게임 엔진의 통합**

Unity는 **`GameObject`**와 같은 객체를 에디터 내에서 시각적으로 관리하고 조작할 수 있는 강력한 도구를 제공합니다. 이러한 객체들은 게임의 런타임과 에디터의 환경 간에 특별히 최적화되어 있습니다. JSON 직렬화는 이러한 통합된 환경의 세밀한 조정을 대체할 수 없습니다.

### **5. 보안 및 무결성 문제**

직렬화 과정에서 **`GameObject`**와 같은 복잡한 객체의 내부 상태를 외부에 노출시키는 것은 보안상의 위험을 초래할 수 있습니다. 이는 게임의 무결성을 해칠 수 있으며, 특히 네트워크를 통한 데이터 전송 시 중요한 문제가 될 수 있습니다.

### **결론**

이러한 이유들로 인해 Unity에서는 **`GameObject`**와 같은 복잡한 객체를 직접적으로 JSON으로 직렬화하지 않습니다. 대신, 이러한 객체들의 상태를 저장하거나 전송할 필요가 있을 때는 보다 단순화된 데이터 구조를 사용하거나, 필요한 정보만을 추출하여 직렬화하는 방법을 선택하는 것이 일반적입니다.

## 참고 - Json 개요

JSON (JavaScript Object Notation)은 데이터 교환 형식으로, 텍스트 기반으로 되어 있어 인간에게도 이해하기 쉽고 기계가 파싱하고 생성하기에도 용이합니다. JSON은 JavaScript의 객체 표기법에서 유래했지만, 언어 독립적이며 대부분의 프로그래밍 언어에서 사용할 수 있습니다. JSON 데이터의 기본 구성요소는 다음과 같습니다:

### **1. 객체 (Object)**

- 객체는 중괄호 **`{}`**로 묶인 키-값 쌍의 집합입니다.
- 키는 문자열이며, 쌍따옴표로 둘러싸여 있습니다. 값은 다양한 데이터 타입이 될 수 있습니다.
- 예시: **`{"name": "John", "age": 30}`**

### **2. 배열 (Array)**

- 배열은 대괄호 **`[]`**로 묶인 값들의 순서 있는 목록입니다.
- 배열의 요소는 어떤 타입의 값이든 될 수 있으며, 다른 객체나 배열도 포함할 수 있습니다.
- 예시: **`["apple", "banana", "cherry"]`**

### **3. 값 (Value)**

- 값은 문자열, 숫자, 객체, 배열, 참(true), 거짓(false), null 등이 될 수 있습니다.
- 문자열은 쌍따옴표로 둘러싸여 있습니다.
- 숫자는 정수 또는 부동소수점이 될 수 있습니다.
- 예시: **`"hello"`**, **`123`**, **`95.5`**, **`true`**, **`false`**, **`null`**

### **4. 문자열 (String)**

- 문자열은 유니코드 문자들의 연속이며, 쌍따옴표로 둘러싸여 있습니다.
- 문자열 내에서는 특정 특수 문자를 이스케이프 해야 할 수도 있습니다 (예: **`\"`**는 쌍따옴표를 나타냄).
- 예시: **`"Hello, world!"`**

### **5. 숫자 (Number)**

- 숫자는 정수나 부동 소수점 형태로 표현됩니다.
- JSON은 숫자 표현에 대해 특정 형식을 지정하지 않지만, 대부분의 구현체들이 정수, 부동소수점 수, 음수를 지원합니다.
- 예시: **`42`**, **`15`**, **`3.14159`**

JSON은 이러한 기본 구성요소들을 조합하여 복잡한 데이터 구조를 표현할 수 있습니다. 이 특성 때문에 웹 개발에서는 물론이고, 다양한 소프트웨어 어플리케이션과 시스템 간의 데이터 교환 포맷으로 널리 사용됩니다.

JSON (JavaScript Object Notation)은 계층적인 데이터 구조를 표현하는 데 탁월한 방식을 제공합니다. 계층적 데이터 구조란 데이터 요소들이 여러 수준의 상하 구조를 이루고 있는 것을 의미합니다. 이 구조는 실제 세계의 복잡한 데이터 관계를 모델링하는 데 유용하며, JSON은 이를 간단하고 효과적으로 표현할 수 있도록 합니다.

### **계층적 데이터 구조의 구성**

### **1. 중첩 객체 (Nested Objects)**

- JSON에서 객체는 다른 객체를 포함할 수 있으며, 이를 통해 데이터 간의 계층적 관계를 나타낼 수 있습니다.
- 예시: 한 사람의 정보를 포함하는 객체가 주소 정보를 담은 또 다른 객체를 포함할 수 있습니다.
    
    ```json
    {
      "name": "John Doe",
      "address": {
        "street": "123 Maple Street",
        "city": "Springfield",
        "zip": "12345"
      }
    }
    ```
    

### **2. 배열의 활용**

- 배열은 같은 유형의 여러 아이템을 순차적으로 저장하는 데 사용됩니다. 배열 내의 각 아이템은 자체적으로 객체 또는 다른 배열을 포함할 수 있습니다.
- 예시: 한 사용자가 여러 개의 전화번호를 가지고 있을 때, 이를 배열로 표현할 수 있습니다.
    
    ```json
    {
      "name": "Jane Doe",
      "phoneNumbers": [
        {"type": "home", "number": "555-1234"},
        {"type": "work", "number": "555-5678"}
      ]
    }
    ```
    

### **3. 복잡한 구조**

- JSON은 이러한 중첩 객체와 배열을 조합하여 더욱 복잡한 계층적 구조를 만들 수 있습니다. 이를 통해 실제 생활의 복잡한 정보 시스템을 모델링할 수 있습니다.
- 예시: 회사 조직 구조를 표현할 때, 각 부서가 여러 팀을 포함하고, 각 팀이 여러 직원을 포함하는 구조를 JSON으로 표현할 수 있습니다.
    
    ```json
    {
      "company": "XYZ Corp",
      "departments": [
        {
          "name": "Engineering",
          "teams": [
            {
              "name": "Product Development",
              "members": [
                {"name": "John Doe", "role": "Software Engineer"},
                {"name": "Jane Doe", "role": "Project Manager"}
              ]
            },
            {
              "name": "QA",
              "members": [
                {"name": "Jim Beam", "role": "QA Analyst"}
              ]
            }
          ]
        },
        {
          "name": "Human Resources",
          "staff": [
            {"name": "Jack Daniels", "role": "HR Manager"}
          ]
        }
      ]
    }
    ```
    

### **장점**

- **명료성과 접근성**: JSON은 텍스트 기반 형식으로, 사람이 읽고 쓰기 쉽습니다. 또한, 대부분의 프로그래밍 언어에서 지원되므로, 데이터 교환에 효과적입니다.
- **유연성**: 중첩 구조를 통해 다양한 데이터 모델을 쉽게 표현할 수 있습니다.

### **사용 시 고려 사항**

- **복잡도 관리**: 너무 깊은 중첩이나 복잡한 구조는 처리하기 어렵고 성능 이슈를 일으킬 수 있습니다. 가능한 한 구조를 간단하게 유지하는 것이 중요합니다.
- **데이터 크기**: 중첩과 배열을 많이 사용할 경우, JSON 문자열의 크기가 커져 네트워크 전송 시간이나 저장 공간에 영향을 줄 수 있습니다.

이러한 계층적 구조는 JSON을 통해 다양한 데이터 관계를 효과적으로 표현하고, 응용 프로그램 간의 호환성을 높이는 데 기여합니다.

## 참고 - Yaml

Unity 엔진은 자체 구성과 데이터 관리에 주로 YAML (YAML Ain't Markup Language)을 사용합니다. YAML은 데이터를 사람이 읽고 쓰기 쉬운 형태로 표현하는 데이터 직렬화 언어로, 설정 파일, 데이터 저장 및 응용 프로그램 간 통신에 널리 사용됩니다.

### **YAML의 특징**

1. **가독성이 높다**: YAML은 JSON과 비교하여 더 읽기 쉬운 형식을 제공합니다. 들여쓰기를 사용하여 데이터 구조를 나타내므로, 사람이 직접 문서를 읽거나 편집하기가 용이합니다.
2. **계층적 구조**: YAML도 JSON처럼 계층적인 데이터 구조를 지원합니다. 들여쓰기를 통해 부모-자식 관계를 명확하게 표현하며, 복잡한 데이터 모델을 쉽게 표현할 수 있습니다.
3. **다중 문서 지원**: 하나의 YAML 파일 내에서 여러 개의 문서를 **`--`**를 사용해 구분하여 포함할 수 있습니다. 이는 여러 설정이나 객체를 한 파일에 효과적으로 관리할 수 있도록 합니다.
4. **언어 독립적**: YAML은 다양한 프로그래밍 언어에서 지원되며, 어떤 언어에서도 쉽게 파싱하고 생성할 수 있습니다.

### **Unity에서의 YAML 사용**

Unity는 게임 오브젝트, 씬 설정, 프리팹 등의 구성 정보를 저장할 때 내부적으로 YAML 형식을 사용합니다. 예를 들어, 유니티 프로젝트의 **`.unity`** 파일이나 **`.prefab`** 파일들은 사실 YAML 형식으로 저장되어 있습니다. 이 파일들은 다음과 같은 정보를 포함할 수 있습니다:

- 오브젝트의 컴포넌트 및 프로퍼티
- 에셋에 대한 참조
- 씬 내의 객체 간의 관계 및 계층 구조

YAML 파일을 통해 유니티는 편집기 내에서 자산을 관리하고, 씬을 구성하며, 게임의 동작 방식을 정의할 수 있습니다. 사용자는 유니티 편집기의 시각적 인터페이스를 통해 이러한 YAML 데이터를 간접적으로 수정하게 됩니다.

Unity의 이러한 내부 사용은 개발자가 직접 YAML 파일을 수정할 필요가 없도록 해주지만, 고급 사용자는 Unity의 메타 파일이나 일부 설정 파일을 직접 열어볼 때 YAML 형식을 직접 볼 수 있습니다. 이를 통해 프로젝트의 세부적인 설정이나 데이터 구조에 더 깊이 개입하고 조정할 수 있습니다

Unity에서 YAML을 사용하는 기본적인 예시로는 Unity의 자산이나 구성 파일을 살펴볼 수 있습니다. 예를 들어, 간단한 Unity 프리팹이 YAML 형식으로 어떻게 저장되는지 예제를 들어 설명하겠습니다.

### **Unity 프리팹 YAML 예제**

Unity에서 프리팹은 게임 오브젝트와 그에 연결된 컴포넌트 및 프로퍼티를 포함하는 재사용 가능한 템플릿입니다. 프리팹을 저장할 때, Unity는 YAML 형식을 사용하여 필요한 모든 정보를 파일에 기록합니다.

아래는 Unity 프리팹 파일의 간략화된 예입니다. 이 YAML 파일은 **`GameObject`** 하나와 그에 추가된 **`Transform`** 컴포넌트를 설명합니다.

```yaml
%YAML 1.1
%TAG !u! tag:unity3d.com,2011:
--- !u!1 &1001
GameObject:
  m_ObjectHideFlags: 0
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  serializedVersion: 6
  m_Component:
  - component: {fileID: 4001}
  m_Layer: 0
  m_Name: ExampleGameObject
  m_TagString: Untagged
  m_Icon: {fileID: 0}
  m_NavMeshLayer: 0
  m_StaticEditorFlags: 0
  m_IsActive: 1

--- !u!4 &4001
Transform:
  m_ObjectHideFlags: 0
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Father: {fileID: 0}
  m_RootOrder: 0
  m_LocalRotation: {x: 0, y: 0, z: 0, w: 1}
  m_LocalPosition: {x: 0, y: 0, z: 0}
  m_LocalScale: {x: 1, y: 1, z: 1}
  m_Children: []
  m_IsActive: 1
```

### **설명**

- **파일 시작**:
    - **`%YAML 1.1`**은 문서가 YAML 버전 1.1을 따르고 있다는 것을 나타냅니다.
    - **`%TAG`**는 태그 네임스페이스를 정의하고, Unity 내부적으로 사용되는 커스텀 태그를 정의합니다.
- **GameObject 섹션**:
    - **`-- !u!1 &1001`**는 새로운 문서의 시작을 알리며, Unity에서 특정 객체 유형(**`GameObject`**)를 나타내고, 이 객체의 고유 ID가 **`1001`**임을 표시합니다.
    - **`m_Name`**은 게임 오브젝트의 이름을 나타냅니다.
    - **`m_Component`**는 이 GameObject에 연결된 컴포넌트의 리스트를 나타내며, 각 컴포넌트는 자신의 ID를 참조합니다.
- **Transform 섹션**:
    - **`-- !u!4 &4001`**는 다른 섹션의 시작을 나타내며, **`Transform`** 컴포넌트를 설명합니다.
    - **`m_LocalPosition`**, **`m_LocalRotation`**, **`m_LocalScale`**는 각각 위치, 회전, 스케일을 나타냅니다.

이 예제는 Unity 프리팹의 기본 구조를 보여줍니다. 실제 프리팹 파일은 더 많은 정보와 컴포넌트를 포함할 수 있으며, 더 복잡한 구조를 가질 수 있습니다. Unity 프로젝트에서 이러한 YAML 파일들을 직접 보고 수정하기 위해서는 Unity 에디터 외부에서 파일을 열어야 합니다. 이는 프로젝트의 **`Assets`** 폴더 내에서 **`.prefab`** 파일을 찾아서 텍스트 에디터로 열면 가능합니다.