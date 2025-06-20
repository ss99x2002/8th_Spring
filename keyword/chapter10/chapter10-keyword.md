# **Spring Security**

- Spring Security는 Spring 애플리케이션의 인증과 인가를 위한 보안 프레임워크이다.

## 주요기능

- 로그인/로그아웃 처리
- 사용자 권한 관리 (Role/Authority 기반)
- CSRF, CORS, 세션 고정 공격 방지 등 보안 기능
- OAuth2, JWT, LDAP 등 다양한 인증 방식 지원
- 커스터마이징 용이한 필터 체인 구조

### 핵심 구성요소

- `SecurityFilterChain`: 보안 필터 체인 설정
- `AuthenticationManager`: 인증 로직 실행
- `UserDetailsService`: 사용자 정보 로딩
- `PasswordEncoder`: 비밀번호 암호화

# **인증(Authentication)과 인가(Authorization)**

## 인증 (**Authentication)**

- 사용자의 신원을 확인하는 절차 : ex) 누구세요? , 아이디/비밀번호 확인

## 인가(Authorization)

- 인증된 사용자가 특정 리소스에 접근 가능한지를 판단 : ex) 이 기능을 사용해도되는 사용자인가?, ROLE 기반 특정 API 사용가능 확인

# **세션과 토큰**

## 세션 기반 인증

- 로그인 시 서버에서 세션 생성 → 세션 ID를 클라이언트에게 쿠키로 전달한다.
- 서버가 세션 상태를 메모리 또는 DB에 저장한다.
- 서버에 상태 저장이 필요하다. (Stateful)
- 확장성에 불리하다. (로드 밸런서 사용 시 세션 공유가 필요)

## 토근 기반 인증 (주로 JWT 사용)

- 로그인 시 토큰(JWT 등) 생성 → 클라이언트가 이를 저장하고, 매 요청마다 헤더에 포함하여 전달
- 서버는 토큰을 검증만하고, 상태 저장은 하지 않는다. (Stateless)
- 마이크로서비스 구조나 모바일 환경에 유리하다.

# **액세스 토큰(Access Token)과 리프레시 토큰(Refresh Token)**

## 엑세스 토큰

- 목적 : 사용자 인증 증명을 위해 사용한다.
- 수명 : 보통 수 분~ 수 시간으로 짧다.
- 저장 위치 : 클라이언트가 저장한다. (로컬 스토리지, 메모리 등)
- 보안 : 유출 시 위험이 크므로 최대한 짧게 설정한다.
- 용도 : API 호출 인증을 위해 사용한다.

## 리프레시 토큰

- 목적 : 엑세스 토큰 재발급용으로 사용된다.
- 수명 : 보통 수 시간~ 수 일로 길다.
- 저장 위치 : 클라이언트가 저장 또는 보안이 높은 서버에 저장한다.
- 보안 : 더 안전하게 저장할 필요가 있다.
- 용도 : 엑세스 토큰 만료 시 갱신용으로 사용한다.

### 예시 흐름 (JWT 기반)

1. 로그인 성공 → **Access Token + Refresh Token 발급**
2. Access Token 만료 → Refresh Token으로 **재발급 요청**
3. Refresh Token도 만료 → **다시 로그인 필요**