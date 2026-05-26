```mermaid
classDiagram
    class Create_Character_UI {
        <<boundary>>
    }
    class Attack_Monster_UI {
        <<boundary>>
    }
    class Add_item_UI {
        <<boundary>>
    }
    class Join_Guild_UI {
        <<boundary>>
    }

    class 전투 {
        +캐릭터생성(플레이어id: String, 캐릭터명: String, 직업: String, 레벨: int) boolean
        +몬스터공격(플레이어id: String) boolean
        +아이템획득(플레이어id: String) boolean
        +길드가입(플레이어id: String) boolean
    }

    class 플레이어 {
        -플레이어id: String
        +플레이어체크(플레이어id: String) boolean
    }

    class 캐릭터 {
        <<abstract>>
        -캐릭터명: String
        -레벨: int
        -HP: int
        -공격력: int
        -인벤토리: Inventory
        +스킬발동()* int
    }

    class 전사 {
        +스킬발동() int %% 검 휘두르기
    }

    class 마법사 {
        +스킬발동() int %% 파이어 볼
    }

    class 인벤토리 {
        -아이템리스트: List~아이템~
        -최대용량: int
        +아이템추가(아이템: 아이템) boolean
    }

    class 아이템 {
        -아이템명: String
        -타입: String
        -가치: int
        -등급: String
    }

    class 길드 {
        -길드명: String
        -캐릭터리스트: List~캐릭터~
        -최대인원: int
        +캐릭터가입(캐릭터: 캐릭터) boolean
    }

    Create_Character_UI ..> 전투 : 사용
    Attack_Monster_UI ..> 전투 : 사용
    Add_item_UI ..> 전투 : 사용
    Join_Guild_UI ..> 전투 : 사용
    전투 ..> 플레이어 : 검증 요청
    전투 --> 캐릭터 : 관리

    캐릭터 <|-- 전사
    캐릭터 <|-- 마법사

    캐릭터 "1" *-- "1" 인벤토리 : 소유

    인벤토리 "1" *-- "0..*" 아이템 : 포함
    
    길드 "1" o-- "0..*" 캐릭터 : 길드원