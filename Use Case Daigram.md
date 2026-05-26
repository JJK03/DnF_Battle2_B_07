```mermaid
graph TD
    플레이어((플레이어))

    UC_캐릭터생성["캐릭터생성"]
    UC_몬스터공격["몬스터공격"]
    UC_길드가입["길드가입"]
    UC_아이템획득["아이템획득"]
    UC_플레이어체크["플레이어체크"]

    플레이어 --> UC_캐릭터생성
    플레이어 --> UC_몬스터공격
    플레이어 --> UC_길드가입
    플레이어 --> UC_아이템획득

    UC_캐릭터생성 -.->|&lt;&lt;include&gt;&gt;| UC_플레이어체크
    UC_몬스터공격 -.->|&lt;&lt;include&gt;&gt;| UC_플레이어체크
    UC_길드가입 -.->|&lt;&lt;include&gt;&gt;| UC_플레이어체크
    UC_아이템획득 -.->|&lt;&lt;include&gt;&gt;| UC_플레이어체크