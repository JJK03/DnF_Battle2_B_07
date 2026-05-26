```mermaid
sequenceDiagram
    autonumber
    actor Player as 플레이어
    participant UI as Create_Character_UI
    participant Battle as 전투
    participant P_Class as 플레이어 클래스
    participant Char as 캐릭터 (전사/마법사)
    participant Inv as 인벤토리

    Player->>UI: 캐릭터 생성 입력 (플레이어id, 캐릭터명, 직업, 레벨)
    UI->>Battle: 캐릭터생성(플레이어id, 캐릭터명, 직업, 레벨)
    
    %% <<include>> 플레이어체크 반영
    rect rgb(240, 240, 240)
        Note over Battle, P_Class: [Include] 플레이어 검증 수행
        Battle->>P_Class: 플레이어체크(플레이어id)
        P_Class-->>Battle: boolean (true)
    end

    alt 직업 == 전사
        Battle->>Char: 전사 객체 생성 (-캐릭터명, -레벨, -HP, -공격력)
    else 직업 == 마법사
        Battle->>Char: 마법사 객체 생성 (-캐릭터명, -레벨, -HP, -공격력)
    end
    
    %% 클래스 다이어그램의 Composition 관계 반영
    Note over Char, Inv: [Composition] 캐릭터 생성 시 인벤토리 동시 생성
    Char->>Inv: 인벤토리 객체 생성 (-최대용량=10 설정)
    Inv-->>Char: 생성 완료

    Char-->>Battle: 생성 완료 (인벤토리 포함)
    Battle-->>UI: void
    UI-->>Player: 캐릭터 생성 결과 출력 (JSP)