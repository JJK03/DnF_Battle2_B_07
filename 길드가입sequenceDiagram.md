```mermaid
sequenceDiagram
    autonumber
    actor Player as 플레이어
    participant UI as Join_Guild_UI
    participant Battle as 전투
    participant P_Class as 플레이어 클래스
    participant Guild as 길드

    Player->>UI: 길드 가입 신청 (플레이어id, 길드명)
    UI->>Battle: 길드가입()
    
    %% <<include>> 플레이어체크 반영
    rect rgb(240, 240, 240)
        Note over Battle, P_Class: [Include] 플레이어 검증 수행
        Battle->>P_Class: 플레이어체크(플레이어id)
        P_Class-->>Battle: boolean (true)
    end

    %% 레벨 제한 없이 바로 길드 정원 체크
    Battle->>Guild: 최대인원 및 현재 길드원 수 확인
    Guild-->>Battle: 확인 결과 (현재 인원 반환)

    alt 현재 인원 < 최대인원(5)
        %% 클래스 다이어그램에 명시된 메서드 시그니처 반영
        Battle->>Guild: 캐릭터가입(캐릭터)
        Guild-->>Battle: void
        
        Battle-->>UI: void
        UI-->>Player: 길드 가입 완료 메시지 출력 (JSP)
    else 현재 인원 >= 최대인원(5)
        Battle-->>UI: void
        UI-->>Player: "길드 정원 초과" 에러 출력 (JSP)
    end
