---
name: problem-definition
description: W01 [실습] 상담사 문제정의(P36)를 조별 고객 세그먼트에 맞게 수행한다. segment-analyst → problem-writer → problem-reviewer 서브에이전트를 순서대로 돌리고 검토 결과에 따라 최대 2회 수정한다. "문제정의 실습", "6조 문제정의 해줘", "/problem-definition 3" 같은 요청에 사용.
argument-hint: <조 번호 1~6> [추가 요청]
allowed-tools: Bash(cat:*)
---

<!-- 래퍼 파일. 스킬 원본은 .agents/skills/problem-definition/SKILL.md 한 곳에만 둠 -->
인자: `$ARGUMENTS`

!`cat .agents/skills/problem-definition/SKILL.md`
