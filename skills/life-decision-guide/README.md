# 人生决策 skill（life-decision-guide）

让 AI 助手照《高性价比人生指南》回答具体问题：该不该做、值不值、怎么选、出事了先做什么、能领哪笔钱、这么干犯不犯法。

它做的事只有一件：**先在 `book/` 查找并读完相关条目，再按书的算账方式回答**。每条注明「源自第 X 章，第 Y 个条目」；查不到就说查不到，不凭记忆编数字。

规则全在 [SKILL.md](SKILL.md) 里，两个工具共用同一个文件，不维护两份。

## 装到 Claude Code

在本仓库里开 Claude Code，不用装——`.claude/skills/life-decision-guide/` 已经指向这份规则。

想在任何目录下都能用，复制到个人 skill 目录：

```bash
mkdir -p ~/.claude/skills/life-decision-guide && curl -fsSL -o ~/.claude/skills/life-decision-guide/SKILL.md "https://raw.githubusercontent.com/tranhaiduong835-dev/In-Loactin-Hosrt/main/skills/life-decision-guide/SKILL.md"
```

之后直接问「每天通勤两小时值不值」「朋友让我替他担保，签不签」就会触发；也可以显式说「用 life-decision-guide 回答」。

## 装到 Codex

在本仓库里开 Codex，根目录的 `AGENTS.md` 会指引它读取这份规则；但 `skills/life-decision-guide/` 本身**不是** Codex 自动发现技能的位置。要在技能列表里看到它，并用 `$life-decision-guide` 显式调用，需把 `SKILL.md` 安装到 Codex 识别的 `.agents/skills/life-decision-guide/`。以下从仓库根目录执行，安装到个人目录，任何项目都可用：

Windows PowerShell：

```powershell
$skillDest = Join-Path $env:USERPROFILE '.agents\skills\life-decision-guide'
New-Item -ItemType Directory -Force $skillDest | Out-Null
Copy-Item .\skills\life-decision-guide\SKILL.md $skillDest
```

macOS / Linux：

```bash
mkdir -p ~/.agents/skills/life-decision-guide
cp skills/life-decision-guide/SKILL.md ~/.agents/skills/life-decision-guide/
```

也可以只在当前仓库安装到仓库根目录的 `.agents/skills/life-decision-guide/`，不影响其他项目。更新本仓库的 `SKILL.md` 后，已复制安装的版本需重新复制。Codex 通常会自动发现新技能；如未出现，重启 Codex。[Codex 技能位置与调用方式](https://learn.chatgpt.com/docs/build-skills)。

### 在 Codex 中使用

在 Codex CLI 或 IDE 中可输入 `$life-decision-guide` 显式调用；在 ChatGPT 桌面应用的 Codex 中可通过技能选择器调用。也可以直接提问，Codex 可能根据技能描述自动选用，但需要确定使用本技能时请显式指定。例如：

```text
$life-decision-guide 朋友让我替他担保，签不签？请先查书中条目。
```

技能会先从当前仓库的 `book/` 搜索并读完整条目，再按成本、收益和证据等级说明理由，给出「源自第 8 章，第 18 个条目」这样的出处；书里没有的数字或结论不会补造。离开本仓库使用时，需允许它读取网络上的本仓库正文，或事先把仓库克隆到当前工作区。

## 正文从哪来

本地有这个仓库就读本地的 `book/`；没有就现取：

```bash
git clone --depth 1 https://github.com/tranhaiduong835-dev/In-Loactin-Hosrt.git
```

取不到网络就如实说明，不能用记忆替代正文。

## 改动须知

节的清单以仓库根目录 `README.md` 的「这本书想回答的问题」表为准，条号以 `book/` 中的标题为准，性价比档算法以 `index.html` 的 `COST_W` 和 `e.ratio` 为准。当前正文为 34 个章节、767 个条目；增删条目后先核对示例出处，不能凭本说明里的旧数字作答。
