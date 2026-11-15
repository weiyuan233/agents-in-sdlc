---
title: "레슨 8 - 기능 풀 리퀘스트(PR) 만들기 및 병합"
description: "필터링, 지침, 스킬 업데이트, 품질 보증(QA) 프로필, 테스트를 함께 검토한 뒤 PR을 만들고 Agent Merge를 사용합니다."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

필터링 구현, 지침 업데이트, 스킬 업데이트, 품질 보증(QA) 프로필, 테스트가 하나의 브랜치에 저장되어 있습니다. 이제 모두 함께 검토하고 풀 리퀘스트(PR)를 엽니다. 별점 PR은 직접 병합했지만, 이번에는 **Agent Merge**가 병합 절차를 관리하도록 합니다.

> [!NOTE]
> 일반적으로 기능, 지침 업데이트, 스킬 업데이트, QA 에이전트를 몇 개의 PR로 나눕니다. 워크숍을 간소화하기 위해 필터링과 품질 작업 전체를 하나의 세션과 브랜치에서 진행하고 이 PR에 포함했습니다.

이 레슨에서는 다음 작업을 수행합니다.

- Agent Merge와 병합 수명 주기를 자동화하는 방식을 알아봅니다.
- 전체 기능 PR과 검증 근거를 살펴봅니다.
- 검토를 마친 뒤에만 Agent Merge를 승인하고 PR이 병합되었는지 확인합니다.

## 시나리오

필터링 작업 전반에서 Copilot을 사용해 기능을 계획하고 구현하고 검증했습니다. 이제 Tailspin Toys는 병합 승인을 개발자가 계속 제어하면서 남은 PR 작업을 자동화하려고 합니다.

## Agent Merge 소개

**Agent Merge**는 GitHub Copilot app에서 풀 리퀘스트를 병합하는 데 필요한 나머지 작업을 자동화합니다. 이 기능을 활성화하면 앱의 세션이 PR을 확인하고 병합을 막는 문제를 해결합니다. 여기에는 실패한 지속적 통합(CI) 검사 수정, 검토 의견에 응답, 필요한 경우 리베이스가 포함됩니다. GitHub에서 병합을 허용하면 PR을 병합합니다. Agent Merge는 백그라운드에서 실행되고 앱을 다시 시작해도 계속 작동하며, PR이 병합되면 자동으로 꺼집니다.

지금까지는 직접 **Merge pull request**를 선택했습니다. Agent Merge가 이 작업을 맡을 수 있지만, 코드를 수정하고 병합하려면 개발자의 명시적인 승인이 필요합니다. 병합 권한을 부여하기 전에 허용되는 작업과 변경 내용을 검토합니다.

## Agent Merge로 PR 관리

코드 작성을 마치고 검토했으므로 Agent Merge가 PR 절차를 관리하도록 설정합니다.

1. 에이전트 선택기에서 **Default agent**를 선택합니다.
2. **Create PR** 옆의 드롭다운을 선택합니다.
3. **Agent Merge**를 선택합니다. 버튼이 **Agent Merge**로 바뀝니다.
4. **Agent Merge** 버튼을 선택해 프로세스를 시작합니다. 드롭다운에서 옵션을 선택하는 것만으로는 시작되지 않습니다.

Agent Merge가 다음 작업을 수행합니다.

- 제목과 설명이 있는 PR을 만듭니다.
- 이슈에서 세션을 시작했다면 설명 본문에서 관련 이슈를 참조합니다.
- 대상 브랜치에 맞춰 리베이스하거나 잠재적인 병합 충돌을 처리합니다.
- 모든 검사가 통과하는지 지속적 통합(CI) 절차를 모니터링합니다.
- 다른 개발자나 Copilot 코드 검토의 피드백이 있는지 PR을 확인하고 의견을 해결하도록 변경합니다.
- 모든 작업이 성공하면 PR을 자동으로 병합할 수 있습니다. 이 단계는 선택 사항입니다.

모든 검사가 통과한 뒤 Agent Merge가 PR을 병합하도록 설정합니다.

5. 병합 권한을 부여하기 전에 PR diff와 검증 근거를 검토합니다.
6. **Agent Merge** 옆의 드롭다운을 선택합니다.
7. **Merge pull request** 옆에 확인 표시가 있는지 확인합니다.
8. 에이전트에 병합을 명시적으로 요청합니다.

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 진행 상황을 확인하고 보고된 차단 요인을 해결합니다. 계속하기 전에 PR 상태가 **Merged**인지 확인합니다. 병합 권한이 활성화되었거나 에이전트가 실행 중인 것만으로는 병합이 완료된 것이 아닙니다.

> [!IMPORTANT]
> Agent Merge는 리포지토리 보호나 누락된 권한을 우회하지 않습니다. 계속하기 전에 이러한 차단 요인을 해결합니다.

## 요약 및 다음 단계

코드 생성, 테스트 및 검증, 풀 리퀘스트 절차를 포함한 개발 과정의 여러 단계를 자동화했습니다. 이 레슨에서는 다음 작업을 수행했습니다.

- Agent Merge와 병합 수명 주기를 자동화하는 방식을 알아봤습니다.
- 전체 기능 PR과 검증 근거를 살펴봤습니다.
- 검토 후에만 Agent Merge를 승인하고 PR이 병합되었는지 확인했습니다.

다음으로 [기존 캔버스를 사용하고 분류 캔버스를 만들어][next-lesson] 에이전트와 작업을 살펴보고 계획하고 시각화하는 더 풍부한 방법을 익힙니다.

## 리소스

- [GitHub Copilot app으로 이슈 및 풀 리퀘스트 관리][managing-issues-prs]
- [GitHub Copilot app 정보][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
