```mermaid
sequenceDiagram
    autonumber
    actor Player as 플레이어
    participant UI as Add_item_UI
    participant Battle as 전투
    participant P_Class as 플레이어 클래스
    participant Char as 캐릭터
    participant Inv as 인벤토리
    participant Item as 아이템

    %% 설계서 반영: 아이템타입 입력 추가
    Player->>UI: 아이템 획득 요청 (플레이어id, 아이템명, 아이템타입, 아이템가치)
    UI->>Battle: 아이템획득()
    
    %% <<include>> 플레이어체크 반영
    rect rgb(240, 240, 240)
        Note over Battle, P_Class: [Include] 플레이어 검증 수행
        Battle->>P_Class: 플레이어체크(플레이어id)
        P_Class-->>Battle: boolean (true)
    end

    Battle->>Char: 인벤토리 확인 요청
    Char->>Inv: 아이템리스트 개수 및 최대용량 확인
    Inv-->>Char: 상태 반환
    
    alt 현재 아이템 수 < 최대용량(10)
        %% 설계서 반영: 아이템타입(무기/방어구/물약) 정보 세팅
        Battle->>Item: 아이템 객체 생성 (명칭, 타입, 가치)
        Note over Item: [아이템타입]: 무기, 방어구, 물약<br/>[등급 판정]<br/>1000 이상: 전설(Legendary)<br/>500 이상: 희귀(Rare)<br/>500 미만: 일반(Common)
        Item-->>Battle: 객체 반환
        
        %% 클래스 다이어그램에 명시된 메서드 시그니처 반영
        Battle->>Char: 아이템 추가 지시
        Char->>Inv: 아이템추가(아이템)
        Inv-->>Char: void
        
        Char-->>Battle: 처리 완료
        Battle-->>UI: void
        UI-->>Player: 아이템명, 등급, 획득 성공 메시지 출력 (JSP)
    else 현재 아이템 수 >= 최대용량(10)
        %% 설계서 반영: 10개 제한 에러 메시지
        Battle-->>UI: void
        UI-->>Player: "인벤토리가 가득 차서 아이템을 획득할 수 없습니다." 에러 출력 (JSP)
    end