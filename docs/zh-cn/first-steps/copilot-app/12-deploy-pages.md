---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流，审查其拉取请求，并验证已发布的 Space Quiz。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

通过 GitHub Pages 为 Space Quiz 提供公开访问地址。Copilot 可以准备工作流并指导部署，但你仍负责审查更改并确认站点已上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 审查工作流和拉取请求，然后再合并。
- 跟踪部署过程并验证已发布站点。

## 场景

Space Quiz 已位于 GitHub 仓库中，但人们仍需要一个简单的方法来试用它。使用 GitHub Pages 发布单页站点，可以在不添加服务器或运行时依赖的情况下，通过公共统一资源定位符 (URL) 访问。

## 让 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。在发送此提示前，请检查批准设置和目标仓库。启用 **Approve all** 后，不要指望会出现单独的确认提示。

在 `space-quiz` 的 Copilot app 会话中发送提示，让 Copilot 准备部署，同时等待你审查。

1. 发送以下提示：

   ```plaintext
   Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
   ```

## 审查工作流和拉取请求

合并前先检查工作流的触发条件、权限和部署目标，确保它只会从默认分支部署。

1. 确认只有在仓库中不存在 README 时才添加 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤、触发条件和权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 请求修正并审查更新后的差异。在授权合并前，等待必需的拉取请求检查通过。

## 授权合并和部署

> [!WARNING]
> 下面的提示会授权智能体合并已审查的拉取请求、修改仓库的 Pages 设置并发布站点。只有在完成审查并确认仓库和拉取请求无误后，才发送此提示。根据批准设置，这些操作可能不会再次请求确认。

将 `<PR_NUMBER>` 替换为已审查的拉取请求编号，然后单独发送以下提示：

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段部署示意图：已合并的 space-quiz 拉取请求；GitHub Actions 中成功运行的 Deploy website 工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

部署运行成功后，打开报告中的站点地址，确认访客可以访问 Space Quiz。

1. 跟踪 GitHub Actions 部署运行，直到成功。
2. 打开报告中的统一资源定位符 (URL)，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请遵循它提供的具体说明。在部署成功且在线站点可以正常加载前，不要将任务视为完成。

## 总结与后续步骤

你已完成以下工作：

- 让 Copilot 配置了 GitHub Pages 部署。
- 在合并前审查了工作流和拉取请求。
- 跟踪部署过程并验证了已发布站点。

接下来，继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
