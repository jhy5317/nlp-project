# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

한국어 챗봇 응답 생성(Q→A) 과제: 토큰화 → 임베딩 → 사전학습 Transformer 전이학습 → 파인튜닝 → 평가 → 오류분석 → 발표.
데이터는 `dataset/ChatbotData.xlsx` (Q, A, 감정 label 3종).

토큰화,임베딩, 토큰화를 어떤것을 선택해서할건지,
인코더,디코더,에 대한 뭐라도 선택했을때 그 구조를 선택한 근거
전이학습 활용

자연어처리 (오류를 분석해야됨)
베이스라인을 뭘 고쳤을때 뭐가 개선 되었는지
오류분석에대한 결과(뭐뭐 같아요 x)
임베딩하면서 어떻게 됬다(step by step별로 흐름을 봐야됨)
트랜스포머 다운로드 파이프라인 설계, 평가하고 결과를 발표

제출목록: 코드, 결과보고서, 발표자료

BPE토큰화, 워드투벡터, 엘렉트라, 수업에 사용한 자료들
파인튜닝
인코더,디코더 세트에 대한 구조,
디코더전용구조 이거나 선택

구조선택의 근거, 실험했을때 달라지는것의 비교
하이퍼파라미터비교실험 있어야함,
디코딩전략이 있어야함(접근법)
평가지표,
오류분석

## 코드에 대한 지침

모든 코드 관련 파일은 .ipynb를 사용

진행상황에 대한 모든 로그는 .pdf로 작성.
코드작성 시 매일 날짜를 주석으로 달기.
코드에 대한 간단한 설명도 주석으로 달기.

보고서는 .pdf파일로 작성.

## 수정에 대한 지침

모든 수정 및 생성 전에 사용자에게 물어보고 해달라고 한것만 수정 및 생성하기.
수정 및 생성을 해달라고 하기전엔 절대 먼저 수정 및 생성하면 안됨.

======== 무언가를 하기전에 항상 확인해야 할 지침 ========

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
