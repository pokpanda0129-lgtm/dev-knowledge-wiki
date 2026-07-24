# 실제 subagent 운영 지침

Codex에서 프로젝트 파일만으로 Claude Code처럼 커스텀 subagent가 자동 등록되는 구조는 사용하지 않습니다.

대신 개발 위키 작업에서 계획, 예시 스토리 구성, 리뷰를 분리하면 품질이 좋아지는 경우 Codex의 실제 subagent 실행 기능을 사용합니다.

## 사용할 때

- 여러 문서 후보를 동시에 조사해야 할 때
- 작성과 리뷰를 분리해서 검증하고 싶을 때
- 예시 스토리 구성, 문서 리뷰를 병렬로 진행하면 품질이 좋아질 때
- 사용자가 명시적으로 subagent 또는 병렬 agent 작업을 요청했을 때
- 사용자가 skill/subagent 언급 없이 위키 문서를 요청했지만, 작업 범위가 넓어 독립 검토가 도움이 될 때

## 사용하지 않을 때

- 단순 문서 하나를 바로 작성하면 충분할 때
- 오타 수정, 링크 수정, README 정리처럼 위임 비용이 더 큰 작업
- 바로 다음 작업이 subagent 결과에 막혀 있는 경우
- 사용자가 subagent를 쓰지 말라고 요청했을 때

## 역할 매핑

프로젝트에서 쓰는 역할 이름은 실제 subagent 프롬프트의 역할 설명으로 사용합니다.

- `wiki-planner`: 주제 범위, 저장 위치, 섹션 계획을 정합니다.
- `story-explainer`: 개념을 기억하기 쉬운 짧은 예시 스토리 방향을 정합니다.
- `wiki-reviewer`: 작성된 문서의 품질을 검토합니다.
- `accuracy-reviewer`: 비유, 단정 표현, 프레임워크 동작, 캐시/운영 명령어의 기술 정확성을 검토합니다.

## 실행 방식

1. 먼저 메인 Codex가 전체 요청을 분석합니다.
2. 병렬로 처리할 가치가 있는 독립 작업만 subagent에 위임합니다.
3. 각 subagent에는 하나의 좁은 책임만 맡깁니다.
4. subagent 결과를 그대로 붙이지 않고 메인 Codex가 통합합니다.
5. 최종 문서는 `assets/article.md`와 `references/writing.md` 기준으로 정리합니다.

## 예시

사용자 요청:

```text
subagent를 써서 frontend의 SSR과 CSR 차이 문서를 만들어줘
```

위임 예시:

- `wiki-planner`: 저장 경로와 섹션 계획 작성
- `story-explainer`: 비개발자도 이해할 수 있는 짧은 예시 스토리 구성 작성
- `wiki-reviewer`: 최종 문서 검토
- `accuracy-reviewer`: 비유와 기술 사실이 어긋나지 않는지 검토
