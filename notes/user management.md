## 정리
---

가장먼저 UserDetailsService라는 인터페이스가 있다 이 인터페이스는 loadUserByUsername(String username) 메서드 한개만 가지고 있고
이를 확장한 UserDetailsManager라는 인터페이스가 있다 이 인터페이스는 createUser(UserDetails user), updateUser(UserDetails user), deleteUser(String username), changePassword(String oldPwd, String newPwd), userExists(String username) 메서드가있다.

이 둘을 나눠 놓은 이유는 어떤 프로덕션에선 사용자의 정보를 변경하지 못하게 하고 로드만 할 수 있어야 하지만 또 어떤 프로덕션에선 사용자의 정보를 생성, 변경, 삭제할 수 있어어야 하기 때문에 둘을 나눠 놓았다.

그리고 UserDetailsManager를 스프링 개발자들이 미리 구현해놓은 클래스들이 있는데 InMemoryUserDetalsManager, JdbcUserDetalsManager, LdapUserDetalsManager가 있다

또한 이 모든 기능들에선 UserDetails라는 인터페이스를 사용하는데 이는 최종 사용자를 나타내는 인터페이스로 사용자이름, 자격 증명, 권한등을 포함한다
이는 이름 자체로 설명적이며 createUser(), updateUser()등의 메서드 들이 있다.
스프링에선 이를 단순한 인터페이스로 놔두지않고 User와 같은 구현객체를 만들어서 쉽게사용할 수 있게 만들었다.
## 수정본
---
### Spring Security의 `UserDetailsService`와 `UserDetailsManager`

1. **`UserDetailsService`**
    
    - Spring Security에서 제공하는 핵심 인터페이스로, **사용자 정보를 로드**하는 역할을 합니다.
    - **메서드:**
        - `loadUserByUsername(String username)`
            - 사용자 이름(username)을 기반으로 사용자의 정보를 로드합니다.
2. **`UserDetailsManager`**
    
    - `UserDetailsService`를 확장한 인터페이스로, **사용자 정보의 관리** 기능이 추가되었습니다.
    - **추가 메서드:**
        - `createUser(UserDetails user)`
            - 새로운 사용자를 생성합니다.
        - `updateUser(UserDetails user)`
            - 사용자의 정보를 업데이트합니다.
        - `deleteUser(String username)`
            - 사용자를 삭제합니다.
        - `changePassword(String oldPassword, String newPassword)`
            - 사용자의 비밀번호를 변경합니다.
        - `userExists(String username)`
            - 사용자의 존재 여부를 확인합니다.
3. **이 둘을 나눈 이유**
    
    - **`UserDetailsService`**는 **읽기 전용** 작업이 필요한 경우에 사용됩니다.
        - 예: 사용자 인증만 필요한 시스템.
    - **`UserDetailsManager`**는 **읽기와 쓰기 작업**이 모두 필요한 경우에 사용됩니다.
        - 예: 사용자 관리 기능이 필요한 시스템.

---

### 스프링에서 제공하는 `UserDetailsManager` 구현체

Spring Security는 `UserDetailsManager` 인터페이스의 몇 가지 구현체를 기본으로 제공합니다. 이를 사용하여 다양한 데이터 저장소를 쉽게 다룰 수 있습니다.

- **`InMemoryUserDetailsManager`**
    - 메모리에 사용자 정보를 저장합니다.
    - 주로 테스트나 간단한 애플리케이션에서 사용됩니다.
- **`JdbcUserDetailsManager`**
    - JDBC를 사용하여 데이터베이스에서 사용자 정보를 관리합니다.
- **`LdapUserDetailsManager`**
    - LDAP을 통해 사용자 정보를 관리합니다.

---

### `UserDetails` 인터페이스

- 최종 사용자를 나타내는 인터페이스로, 다음과 같은 정보를 제공합니다.
    - 사용자 이름 (username)
    - 자격 증명 (credentials, 보통 비밀번호)
    - 권한 (authorities)
    - 계정의 활성화 상태 등
- 이름 그대로 **설명적인 인터페이스**이며, Spring Security의 사용자 모델의 핵심 역할을 합니다.

### 구현체

- Spring Security는 `UserDetails`를 구현하는 기본 클래스인 **`User`**를 제공합니다.
    - 이를 통해 `UserDetails` 인터페이스를 쉽게 사용할 수 있습니다.
    - `createUser()`나 `updateUser()` 등의 메서드에서 이 구현체를 사용하는 것이 일반적입니다.

---

### 추가 개선 제안

- `UserDetails`의 역할과 메서드를 구체적으로 설명하면 더 좋습니다. 예를 들어, 계정 활성화 상태를 다루는 메서드(`isAccountNonExpired`, `isAccountNonLocked` 등)도 중요합니다.
- 각 구현체(`InMemoryUserDetailsManager`, `JdbcUserDetailsManager`, `LdapUserDetailsManager`)의 사용 사례를 조금 더 구체적으로 기술하면 활용 방안을 이해하기 쉽습니다.

---

위와 같이 정리하면 더 읽기 쉽고 명확하게 내용을 전달할 수 있을 것 같습니다. 😊


---
지금은 UserDetailsManager 또는

UserDetailsService라는 컴포넌트에 집중해 보겠습니다

이것은 UserDetailsManager와 UserDetailsService 인터페이스 및

그 구현 클래스에 대한 혼란을 해소하기 위해

준비한 슬라이드입니다

그래서 다음 몇 번의 강의에서는

Spring Security 프레임워크 내에서

사용자 관리와 관련된 중요한 클래스와

인터페이스에 대해 논의할 것입니다

가장 먼저, UserDetailsService라는 인터페이스가 있습니다

이 인터페이스 안에는

사용자 세부 정보를 어떤 저장 시스템에서든 로드하는

단일 추상 메소드가 있습니다

그것은 메모리 내 저장 시스템일 수도 있고

데이터베이스일 수도 있습니다

저장 시스템이 무엇이든 상관없이

이 메소드의 목적은 주어진 사용자 이름을 기반으로

저장 시스템에서 사용자 세부 정보를 로드하는 것입니다

이제 이 인터페이스는

UserDetailsManager라는 또 다른 인터페이스에 의해 확장됩니다

그래서 이 UserDetailsManager에는 많은 다른 추상 메소드가 있습니다

첫 번째는 createUser()로

우리가 입력으로 제공하려는 객체를 기반으로

새로운 사용자를 생성할 것입니다

이러한 메소드에 제공해야 하는 객체의 유형은

UserDetails입니다

기본적으로, 새로운 사용자를 생성하거나

새로운 계정을 등록하려고 할 때마다

이것이 Spring Security 프레임워크가

저장 시스템 내에서 UserDetails를 생성하는 데 사용할 메소드입니다

다음으로, 최종 사용자가 자신의 세부 정보를 업데이트하려는 시나리오입니다

이러한 시나리오에서는 updateUser() 메소드가 등장하고

그 다음으로 deleteUser() 메소드가 사용됩니다

이 메소드는 우리가 입력으로 제공하려는 사용자 이름을 기반으로

저장 시스템에서 사용자 세부 정보를 삭제합니다

그러나 최종 사용자가 단순히

비밀번호를 변경하려는 시나리오에서 그렇습니다

이러한 시나리오에서는 updateUser()를 사용하는 대신

changePassword() 메소드를 사용합니다

이 메소드는 기존 비밀번호와 새로운 비밀번호를 입력받습니다

마지막으로, 이 인터페이스 내에는

userExists()라는 헬퍼 메소드도 있습니다

예를 들어, 누군가가 주어진 사용자 이름을 기반으로

저장 시스템 내에 사용자가 있는지 이해하고자 할 때

실제로 인증을 수행하기 전에

이 메소드를 사용할 수 있습니다

여기서, 왜 UserDetailsService와

UserDetailsManager가 분리되어 있는지에 대한

질문이 있을 수 있습니다

Spring Security 팀이

이 메소드들을 두 개의 다른 인터페이스로 분리한 데에는 매우 좋은 이유가 있습니다

우리가 맨 처음 보는 인터페이스는 UserDetailsService입니다

이 인터페이스가 사용자 이름으로 사용자 세부 정보를 로드하는 데만

도움이 되는 메소드를 가지고 있는 이유는

때때로 조직에서는 단순히 항상 사용자를

인증하고 싶어하기 때문입니다

그들은 최종 사용자가 웹사이트나

API를 사용하여 직접 등록하는 시나리오를

절대 가지지 않습니다

따라서 사용자를 생성하거나 삭제하거나

업데이트하는 시나리오가 없다면

UserDetailsService 구현만으로도 충분합니다

이 인터페이스의 구현 클래스는

항상 사용자 세부 정보를 읽을 것입니다

그렇다면 언제 사용자 세부 정보를 읽을까요?

로그인 시도 중입니다

일부 조직에서는

최종 사용자에게 명을 제공하고

로그인 시 항상 해당 자격 증명을 사용하도록

요구할 것입니다

그들은 새로운 계정을 다시 시작하거나 세부 정보를 업데이트하거나

비밀번호를 변경할 수 있는 유연성을 제공하지 않을 것입니다

따라서 이러한 조직의 경우

이 인터페이스에 대한 구현 클래스 하나만 있어도 충분합니다

그러나 조직이 사용자 세부 정보의 전체 기능을 원할 경우

예를 들어 생성, 업데이트, 삭제, 읽기, 비밀번호 변경이나

사용자가 존재하는지 확인하는 기능을 원할 경우

이러한 시나리오에서는 UserDetailsManager의 구현을

구축할 수 있습니다

기본적으로 이 UserDetailsManager는

모든 CRUD 작업을 지원합니다

C는 생성 Create, R은 Read를 의미합니다

이 읽기 기능은 상위 인터페이스에서 가져오며

U는 Update

D는 Delete를 의미합니다

이를 통해 조직이

최종 사용자에게

모든 CRUD 작업 옵션을 제공하려는 경우

UserDetailsManager 인터페이스를 구현해야 한다는 점이 매우 명확해집니다

그렇지 않고 단순히 최종 사용자를 인증하려는 경우

UserDetailsService 인터페이스를 구현해야 합니다

따라서 이것들은 Spring Security에서 제공하는

인터페이스 또는 계약으로

저장 시스템 내에서 사용자 세부 정보를 다룰 때 따라야 합니다

그러나 Spring Security의 장점은

팀이 단순히 계약이나 인터페이스만 제공하는 데

그치지 않았다는 점입니다

대신, 몇 가지 샘플 구현 클래스도 제공했습니다

예를 들어, InMemoryUserDetailsManager는

이 UserDetailsManager의 구현 클래스입니다

이전에 애플리케이션의 메모리 내에서 사용자를 생성하고자 할 때

이러한 구현 클래스를 사용하면 된다는 것을 보았습니다

우리는 이전 강의에서 이 클래스를 많이 탐구했는데

이는 Spring Security를 시작하는 많은 조직에서

매우 일반적인 요구 사항이기 때문입니다

Spring Security 팀이 한 일은

라이브러리 내부에서 UserDetailsManager의 구현인

InMemoryUserDetailsManager를 제공한 것입니다

다음으로 일반적인 요구 사항은

사용자 정보를 데이터베이스에 저장하는 것입니다

사용자 정보가 데이터베이스에 저장되는 시나리오를 처리하기 위해

Spring Security에서 제공하는 구현 클래스 중 하나는

JdbcUserDetailsManager입니다

Jdbc와 InMemory 외에도

사용자 정보를 저장하는 또 다른 일반적인 접근 방식은 LDAP입니다

따라서 LDAP는 저장 시스템 내에서

UserDetails를 저장하는 레거시 스타일 중 하나입니다

많은 회사가 이 LDAP 시스템을 사용하지는 않지만

일부 조직이 이 LDAP 저장 시스템을 사용하고 있다면

Spring Security 팀에서 제공하는 LdapUserDetailsManager를 활용할 수 있습니다

어떤 이유로든 이 세 가지 옵션이 모두 도움이 되지 않는다면

사용자 정의 UserDetailsManager를 구현할 수 있으며

언제든지 사용자 정의 UserDetailsManager를 구축할 수 있습니다

우리는 다음 섹션에서

사용자 정의 구현을 구축하는 옵션을 탐구할 것입니다

지금은 이 세 가지 계층의 역할에 대해

명확히 이해하고 계신다고 가정하겠습니다

첫 번째 계층은 인터페이스인 UserDetailsService이고

그 다음은 또 다른 인터페이스인 UserDetailsManager이며

그 다음은 Spring Security 팀에서 제공하는 세 가지 다른 구현 클래스입니다

그리고 마지막으로

UserDetails 인터페이스를 나타내는 녹색 화살표를 보셨을 것입니다

Spring Security 팀에서 제공하는 이 모든 계약의 장점은

이들이 모두 UserDetails 인터페이스를 활용한다는 점입니다

Spring Security 내부에서

사용자는 UserDetails로 표현됩니다

따라서 UserDetails는 최종 사용자를 나타내는 인터페이스로

사용자 이름, 자격 증명, 권한 등을 포함합니다

이러한 모든 세부 정보를 저장하기 위해

Spring Security 프레임워크 내에

UserDetails라는 계약이 있습니다

이름 자체가 설명적이며

최종 사용자의 세부 정보를 보유할 것입니다

그리고 이것은 인터페이스입니다

createUser(), updateUser()와 같은 모든 메소드를 보실 수 있습니다

이 메소드들은 UserDetails 타입의 객체를 받고 있으며

이와 유사하게 loadUserByUsername()의 반환 타입도

UserDetails 자체입니다

이 UserDetails 인터페이스와 관련하여

Spring Security 팀은 단순히 계약이나 인터페이스를 정의하는 것만으로

라이브러리 코드를 남겨두지 않았습니다

대신, 그들이 한 일은

이 인터페이스의 샘플 구현을 제공한 것입니다

이 샘플 구현 클래스는 user입니다

따라서 Spring Security 프레임워크 내에

UserDetails 인터페이스를 구현한 user 클래스가 있습니다

