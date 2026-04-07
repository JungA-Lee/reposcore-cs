# C# CLI 프로그램 개발을 위한 라이브러리 (Octokit & Cocona)

## 1. Cocona

가장 쉽고 빠르게 C# CLI 애플리케이션을 만들 수 있게 해주는 프레임워크입니다.
### 특징
- 최소한의 코드로 강력한 CLI 구현 (Minimal API 스타일)
- 명령어, 옵션, 인자 파싱 및 유효성 검사 자동화
- `--help` 메시지 및 쉘 자동완성 지원
- 의존성 주입(Dependency Injection) 및 로깅 등 엔터프라이즈 기능 통합

### 설치 방법
아래 명령어를 터미널에 입력합니다.
```
dotnet add package Cocona
```

### 사용 예시
아래는 `Cocona`를 사용해 `--token` 옵션을 받아 실행하는 예시입니다.
```csharp
using Cocona;

var app = CoconaApp.Create();

// 명령어를 메서드 형태로 정의
app.AddCommand((string token) => 
{
    Console.WriteLine($"입력된 토큰: {token}");
});

app.Run();
```

### 실행 결과
```
$ dotnet run -- --token abc123
입력된 토큰: abc123
```

---

## 2. Octokit

GitHub API를 C# 객체 지향 방식으로 다룰 수 있게 해주는 공식 클라이언트 라이브러리입니다.

### 특징
- GitHub REST API의 모든 기능(이슈, PR, 레포지토리 관리 등) 지원
- 비동기(Async/Await) 기반의 설계
- 강력한 타입 시스템을 통한 안정적인 API 호출
- GitHub 공식 관리 라이브러리로 높은 신뢰도

### 설치 방법
아래 명령어를 터미널에 입력합니다.
```
dotnet add package Octokit
```

### 사용 예시
아래는 `Octokit`을 사용하여 로그인한 사용자의 정보를 가져오는 예시입니다.
```csharp
using Octokit;

var client = new GitHubClient(new ProductHeaderValue("MyCliTool"));
var tokenAuth = new Credentials("your_github_token_here");
client.Credentials = tokenAuth;

var user = await client.User.Current();
Console.WriteLine($"GitHub 사용자명: {user.Login}");
```

### 실행 결과
```
$ dotnet run
GitHub 사용자명: Octocat
```

---

## 3. Cocona + Octokit (실전 조합)

두 라이브러리를 함께 사용하여 실제 GitHub 정보를 조회하는 CLI 도구 예시입니다.

### 사용 예시
```csharp
using Cocona;
using Octokit;

var app = CoconaApp.Create();

app.AddCommand(async (string token, string repoOwner, string repoName) =>
{
    var client = new GitHubClient(new ProductHeaderValue("MyHelper"));
    client.Credentials = new Credentials(token);

    var repo = await client.Repository.Get(repoOwner, repoName);
    Console.WriteLine($"레포지토리 설명: {repo.Description}");
});

app.Run();
```

### 실행 결과
```
$ dotnet run -- --token MyToken --repo-owner dotnet --repo-name runtime
레포지토리 설명: .NET runtime enables runs on Windows, Linux, and macOS...
```

---

## 라이브러리 비교 및 역할

| 라이브러리 | 역할 | 특징 |
|---|---|---|
| **Cocona** | **UI/UX 및 인터페이스** | 터미널 입력 처리, 명령어 구조화, 도움말 생성 |
| **Octokit** | **데이터 및 비즈니스 로직** | GitHub 서버와의 통신, 데이터 객체화 |

## 참고 링크

- [Cocona GitHub Repository](https://github.com/mayuki/Cocona)
- [Octokit.net 공식 문서](https://github.com/octokit/octokit.net)
- [GitHub API 공식 가이드](https://docs.github.com/ko/rest)
