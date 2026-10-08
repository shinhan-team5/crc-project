---
name: problem-definition
description: W01 [실습] 상담사 문제정의(P36)를 조별 고객 세그먼트에 맞게 수행한다. segment-analyst → problem-writer → problem-reviewer 서브에이전트를 순서대로 돌리고 검토 결과에 따라 최대 2회 수정한다. "문제정의 실습", "6조 문제정의 해줘", "/problem-definition 3" 같은 요청에 사용.
argument-hint: <조 번호 1~6> [추가 요청]
---

# 상담사 문제정의 실습

인자: `$ARGUMENTS`
작업 폴더: `problem-definition/`

## 조와 세그먼트 (P36)
| 조 | 세그먼트 |
|---|---|
| 1 | 신규가입 6개월 이내 |
| 2 | 장기보유고객 |
| 3 | VIP·고액사용고객 |
| 4 | 휴면직전고객 |
| 5 | 다중카드보유고객 |
| 6 | 연회비부담고객 |

인자에 조 번호가 없으면 진행하지 말고 몇 조인지 묻는다.

## 흐름
```
segment-analyst ──▶ work/{N}조-1-segment.md
        │
problem-writer ──▶ {N}조-{세그먼트}.md          (최종본, 폴더 루트)
        │
problem-reviewer ──▶ work/{N}조-2-review-r1.md
        │  REVISE
problem-writer(수정) ──▶ 최종본 갱신 + work/{N}조-3-changes.md
        │
problem-reviewer ──▶ work/{N}조-2-review-r2.md   (최대 r2, 그래도 REVISE면 사용자에게 넘김)
```

## 절차
2. **segment-analyst** 호출. 전달: 조 번호, 세그먼트, 출력 경로.
3. 프로필의 "이탈위험 신호"와 "확인 필요"를 사용자에게 짧게 보여주고, 바로잡을 점이 있는지 묻는다. 사용자가 실제 업무 경험을 알려주면 프로필에 반영하도록 다시 지시. (사용자가 "알아서 끝까지"라고 했으면 생략)
4. **problem-writer** 호출. 전달: 조 번호, 세그먼트, 프로필 경로, 최종본 경로 `problem-definition/{N}조-{세그먼트}.md`.
5. **problem-reviewer** 호출. 전달: 최종본 경로, 프로필 경로, 회차 r1.
6. REVISE면 problem-writer에 검토 의견 경로를 주어 수정 → reviewer r2. r2도 REVISE면 멈추고 남은 쟁점을 사용자에게 제시.
7. PASS면 최종본 상단의 `검토:` 표기를 갱신하고, 사용자에게 보고:
   - 문제정의 문장
   - 검토에서 고쳐진 핵심 2~3개
   - 남은 가정 (실습 시간에 확인할 것)

## 주의
- 서브에이전트는 다른 서브에이전트를 부를 수 없으므로 순서 제어는 이 스킬(메인 세션)이 한다.
- 각 단계 사이에 파일로만 결과를 주고받는다. 에이전트 보고 내용을 다음 에이전트 프롬프트에 붙여넣지 말고 경로를 넘긴다.
- 여러 조를 한 번에 요청하면 조별로 segment-analyst를 병렬 호출해도 되지만, 검토 루프는 조별로 따로 돈다.
