<p align="right">
  🌐 <a href="./README.md">中文</a> · <a href="./README.en.md">English</a> · <b>日本語</b>
</p>

# 鲁班.skill (luban-skill)

<p align="center">
  <img src="./intro.png" alt="luban-skill: 専門家の方法論を蒸留する、固定観念ではなく — distill expert methodology, not LinkedIn bios. 左側の素材（critique reviews / postmortems / interview banks / standards docs / failure cases）が蒸留器を通り、構造化された SKILL ドキュメント（anchors / critique rubric / honest limits）として出力されます" width="780" />
</p>

> Nuwa は人を蒸留する。luban は専門領域を蒸留する。

*Distill expert methodology into Claude Code Skills. Stop role-playing experts — generate ones that actually critique your PRDs, refuse bad calls, and name trade-offs.*

## Before / After 比較

> 同じ「ひどい PRD」：「3 週間以内に AI セールスアシスタントをリリース。LangChain でオーケストレーション。自動メール返信＋ミーティング設定。目標は営業メールの 50% を agent が自動処理」

**❌ 普通の prompt /「シニア AI PM を演じる」ペルソナ**
> "Great initiative! Consider adding a risk assessment section. Make sure to A/B test your email templates. Align with stakeholders before launch."
>
> *(典型的な LinkedIn-bio 出力：丁寧で、それっぽいが、本質を捉えていない)*

**✅ luban で蒸留した `/infra-pm` — Mira the PM（v0.4.0 の実出力）**
> これは feature の羅列であって、assumption-first ではありません。いきなり LangChain オーケストレーションに飛びついているのは Anthropic Building Effective Agents の核心スタンス「workflow を agent より優先」に反します。「50% 自動処理」は capability metric であって product metric ではない — 顧客が返信を受け取った後の返信率こそが本物です。メール送信は不可逆操作なのに、PRD に confirmation gate も sandbox もありません。3 週間 + eval pipeline 不在 = eval theater です。

差は「より良い prompt」から来ているのではありません。Mira の SOUL.md はデフォルトで framework hype を拒否し、critique-rubric は「capability metric vs product metric」の区別を強制し、anti-patterns は「premature platform」「eval theater」を明示的に名指します。**スタンスは構造的であり、prompt チューニングで補えるものではない** — 詳細は [How it works](#how-it-works) を参照。

---

**「AI が専門家を演じる」を「AI が本当にこの専門領域を理解する」へアップグレード。**

luban はあらゆる手仕事 — B2B SaaS プロダクトマネージャー、刑事弁護士、M&A ファイナンスアドバイザー、UX デザインディレクター — を Claude Code Skill に蒸留します。AI はその道で 10 年やってきた人のように対話します：突っ込みを入れ、断り、trade-off を語る — LinkedIn-bio 風のペルソナではなく。

[![Version: v0.4.0](https://img.shields.io/badge/version-v0.4.0-green)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)]()
[![Skill: Claude Code](https://img.shields.io/badge/skill-Claude%20Code-orange)]()

---

## クイックスタート

<p align="center">
  <img src="./usage.svg" alt="luban 使い方 3 ステップ：① シードを投入（critique reviews / postmortems / standards docs / interview banks / failure cases、またはシードなしで prospecting mode へ）→ ② luban が蒸留（5 段階パイプライン：taxonomy mining / anchor / 5:3:2 progressive spec / critique rubric / tools &amp; workflow を .claude/skills/&lt;role&gt;/ に出力）→ ③ /&lt;role&gt; で呼び出し、定型句ではない本物の専門家レビューを Claude Code で受け取る" width="1100" />
</p>

1. **Skill のインストール** — 本リポジトリ自体が dogfood レイアウト：Claude Code でこのリポジトリを開けば `.claude/skills/luban-skill/` から自動ロードされます。グローバルにインストールするには：

   ```bash
   # macOS / Linux
   git clone https://github.com/PlevanTem/luban-skill.git && \
     cp -r luban-skill/.claude/skills/luban-skill ~/.claude/skills/
   ```

   ```powershell
   # Windows PowerShell
   git clone https://github.com/PlevanTem/luban-skill.git
   Copy-Item -Recurse luban-skill/.claude/skills/luban-skill $HOME/.claude/skills/
   ```

2. **Claude Code で luban を呼び出す**（「luban」または「鲁班」を明示的に発話する必要があります — 汎用トリガーには引っかからない設計）：

   ```
   luban を使って B2B SaaS PM のロールを蒸留してください。30 件の design review 実録を持っています。
   ```

3. **シード素材がない？** こう言ってください：

   ```
   luban で刑事弁護士を蒸留したい。手元には何もない。
   ```

   luban は **シード prospecting モード** に入り、「この sub-specialty 向け critique corpora 候補リスト」を返します — 何を、どこで、どの順で探すかを教えます。

4. **成果物**：完全なロールディレクトリが `.claude/skills/<short-slug>/`（例：`infra-pm/`）に作成されます。`SOUL.md` + `SKILL.md` + `identity.json` + capability map + critique rubric + anti-patterns + 正直台帳 `GENERATION_REPORT.md` を含みます。Claude Code が自動発見し、`/<short-slug>` がフローティングパネルに表示されます。

5. **蒸留済みの全ロールを閲覧**：`/agents` と入力すると、family ごとにグループ化された構造化 roster がレンダリングされます。INDEX.md は luban が新ロール生成時に自動メンテナンスします。

---

## luban で何ができるか

- **PRD を書いていて**、ChatGPT の決まり文句ではなく、本物の B2B SaaS PM に突っ込みを入れて欲しい。
- **コンプライアンス自己点検をしていて**、「弁護士を演じる」prompt ではなく、本当にこの仕事をしてきた弁護士に rubric で歩いて欲しい。
- **コンポーネントライブラリを設計していて**、「ユーザビリティを高めましょう」のような無意味な提案ではなく、500 件のデザインレビューを見てきたディレクターに critique して欲しい。
- **事業計画書を書いていて**、200 件の案件を見てきた投資家に、本当の基準で問題を突きたい。
- **社内ナレッジを沈殿させたい**。特定の人がいるかどうかに依存せず、ある sub-specialty の判断方法を固定化したい。

luban はその専門領域の人を代替するものではありません。**「その手仕事が良く為されたかを判断する基準」をエンジニアリングして実行可能な Skill にするもの**です。

---

## Why luban (not another persona prompt)

世の中の「AI が専門家を演じる」プロジェクトの 90% は、以下の 2 点でつまずきます：

1. **Vibes persona**：`"You are a senior X with 20 years of experience"` のような記述的 prompt は LinkedIn-bio 風の voice を生み、それっぽいが本質を捉えません。
2. **LLM による空想 capability 生成**：LLM 自身に「シニア X が知っていることを記述させる」 — これは stereotype の再生産であり、大半の persona repo の根本的失敗です。

luban はこの両方を拒否します。**Capability は critique corpora / standards docs / failure cases から逆算されなければならず**、LLM の想像から引き出してはいけません。これは構造的スタンスであり、prompt チューニングでは補えません。

> **Nuwa distills people. luban distills disciplines.**

---

## 蒸留済み Skill の例

| Sub-specialty | Slash | 表示名 | 状態 | シード種別 |
|---|---|---|---|---|
| Agent infrastructure PM (0→1 PMF) | [`/infra-pm`](.claude/skills/infra-pm/) | Mira the PM | ✅ v0.3.0 ship | Anthropic BEA + senior Platform PM JDs |
| Game Art Director / Visual Lead (0→1 ビジュアル定調) | [`/game-art-director`](.claude/skills/game-art-director/) | Vera | ✅ v0.1.0 ship | Riot Spirit Blossom + GDC Vault + senior AD JD |
| Generalist UX Research Director | [`/ux-research-director`](.claude/skills/ux-research-director/) | Wren | ✅ v0.1.0 ship | Hall critique × Rohrer NN/g × ReOps 8 Pillars × Director JD |
| B2B SaaS Content Ops Director (cross-region) | [`/content-ops-director`](.claude/skills/content-ops-director/) | Lin | ✅ v0.1.0 ship | CMI/Averi/FullFunnel × LinkedIn B2B × 中国 5 プラットフォーム mechanics |

> v0.4.0 はメタツール方法論を実際に走らせ、4 つのロールを輩出しました — `infra-pm` / `game-art-director` / `ux-research-director` / `content-ops-director` はすべて luban 自身で蒸留されています。新しい sub-specialty の PR を歓迎します。

---

## 既存ソリューションとの違い

> **Nuwa distills people. luban distills disciplines.**

| アプローチ | 解決するもの | luban の違い |
|---|---|---|
| **普通の prompt /「シニア X を演じる」** | LLM を専門家っぽく見せる | luban は vibes persona を拒否 — 専門家を記述するのではなく、方法論で蒸留する |
| **[Nuwa](https://github.com/alchaincyf/nuwa-skill)** | 具体的な実在人物 (Munger / Naval / Musk) の mental model を蒸留 | luban は実在人物に紐付かず、**その sub-specialty の方法論そのもの**を蒸留する |
| **[OpenPersona](https://github.com/acnlabs/OpenPersona)** | ペルソナのライフサイクル管理（生成・制約・進化） | luban の関心は「専門的判断がどう形成されるか」であり、ペルソナの portability ではない |
| **soul.md 系列** (clawsouls / rokoss21 / aaronjmars) | AI agent ペルソナの portability | 同上 — luban はペルソナ層に対して直交する |
| **RAG / ベクトル DB** | LLM に領域知識を外付けする | luban が蒸留するのは **判断基準 + 意思決定ヒューリスティック + 自己点検 rubric** であり、ドキュメント検索ではない |

護城河（差別化）は 2 点：
1. **Sub-specialty の強制** — 「プロダクトマネージャー」のような汎入力は不可。「B2B SaaS PM」レベルまで絞り込む
2. **シード prospecting モード** — ユーザーにシードがない場合、luban は「この sub-specialty 向け critique corpora 候補リスト」を能動的に出力し、ユーザーに探させる

---

## How it works

### luban の 7 つのコアスタンス

完全定義は [`.claude/skills/luban-skill/references/generation-protocol.md` §0](.claude/skills/luban-skill/references/generation-protocol.md) にあります。要約：

1. **Vibes persona を拒否**：`"You are a world-class designer with 20 years of experience"` のような記述的 prompt を禁止 — 出力は LinkedIn-bio 風で、真の判断がない。
2. **LLM の空想生成を拒否**：LLM に「expert X を記述させる」ことで capability を生成するのは禁止 — これは stereotype の再生産であり、大半の「AI 専門家ロール」プロジェクトの根本的失敗。
3. **Sub-specialty を強制**：「senior designer」は広すぎ。「B2B SaaS UX designer」レベルまで分解する。
4. **データソースを signal density で順序付け**：critique corpora > interview banks > standards docs > failure cases > practitioner blogs。
5. **3 層ロード**（Tier-1 always loaded / Tier-2 per task family / Tier-3 retrieval on demand）— すべての能力を SKILL.md に詰め込むのではなく、使用頻度で階層化。
6. **5:3:2 progressive sampling**：Tier-1 内部で 5 コア + 3 隣接 + 2 遠縁の能力を選ぶ — LLM の stereotype 再生産に対抗する。
7. **6-check validation は非オプション**：内容品質 4 + 構造一貫性 2、全通過で初めて納品。

**最も意見が分かれるのはスタンス 2 と 7** — この 2 つは「高速生成体験」と「真剣な方法論」を対立軸に置きます。luban は後者を選びます。あなたのプロダクトビジョンが前者なら、luban は向きません。

### 蒸留パイプライン（5 段階）

```
入力：領域 + sub-specialty + シード
    ↓
[シード門閾チェック] — ゼロシードなら prospecting モードへ
    ↓
[Step 1] 領域 family 判定 → domain-families.md (5 family 骨格)
    ↓
[Step 2] 5 段階パイプライン：
    1. Capability Taxonomy Mining       → capability-map.md
    2. Anchor                           → identity.json
    3. Progressive Specification (5:3:2)→ SKILL.md + clusters
    4. Critique Rubric                  → critique-rubric.md
    5. Tools & Workflow                 → SKILL.md に埋め込み
    ↓
[Step 3] ペルソナ層 + 正直台帳 → SOUL.md + anti-patterns.md + evolution.jsonl
    ↓
[Step 4] 6-check validation（内容 4 + 構造 2）
    ↓
[Step 5] ディレクトリ納品 + GENERATION_REPORT.md
```

<details>
<summary>完全アーキテクチャ図を展開</summary>

```
┌─────────────────────────────────────────────────────────────────────┐
│                          ユーザー入力                                 │
│  領域 (例：PM)  +  Sub-specialty (例：B2B SaaS PM)  +  シード素材    │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
              ┌───────────────────────────────────┐
              │  シード門閾チェック                  │
              └───────────────────────────────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              ▼                                       ▼
     ┌─────────────────┐                  ┌──────────────────────┐
     │  ≥1 件のシード   │                  │  ゼロシード           │
     │  → 生成フローへ  │                  │  → prospecting モード │
     └─────────────────┘                  │  → SEED_PROSPECT.md  │
              │                            └──────────────────────┘
              │                                       │
              │                                       ▼
              │                            ┌──────────────────────┐
              │                            │ シードを携えて戻る    │
              │                            └──────────────────────┘
              │                                       │
              ▼                                       │
   ┌──────────────────────────────────┐              │
   │  Step 1: 領域 family 判定         │◀─────────────┘
   │  → domain-families.md            │
   │  → 5 family × 6-12 sub-specialty │
   └──────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 2: luban 5 Stage Pipeline 実行                          │
   │  Stage 1-5（taxonomy / anchor / spec / rubric / tools）       │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 3: ペルソナ層 + 正直台帳                                │
   │  SOUL.md / anti-patterns.md / evolution.jsonl                │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 4: Validation (6 check)                                │
   │  A 組 4 check + B 組 2 check                                  │
   └──────────────────────────────────────────────────────────────┘
              │
              ▼
   ┌──────────────────────────────────────────────────────────────┐
   │  Step 5: 納品                                                │
   │  <sub-specialty-slug>/ + GENERATION_REPORT.md                │
   └──────────────────────────────────────────────────────────────┘
```

</details>

---

## プロジェクト構造

本リポジトリ自体が Claude Code でプロジェクト単位ロード可能な dogfood レイアウト — すべての skill は `.claude/skills/` 下、Claude Code が自動発見します。グローバルにインストールするには対応サブディレクトリを `~/.claude/skills/` にコピーします。

```
./
├── README.md                         # 中国語（デフォルト）
├── README.en.md                      # 英語
├── README.ja.md                      # 日本語（本ファイル）
├── LICENSE                           # MIT
├── CHANGELOG.md                      # バージョン履歴
├── ARCHITECTURE_v0.2.md              # アーキテクチャ決定ドラフト
└── .claude/
    └── skills/                       # Claude Code ネイティブ skills パス
        ├── INDEX.md                  # 蒸留済み全ロールのレジストリ (v0.4)
        ├── luban-skill/              # メタツール（新ロール生成）
        ├── agents/                   # /agents メタ skill (v0.4)
        ├── infra-pm/                 # 最初の蒸留ロール：Mira the PM
        ├── game-art-director/        # Vera
        ├── ux-research-director/     # Wren
        └── content-ops-director/     # Lin
```

すべての `*-template.md` は**任意の足場**であり、強制テンプレートではありません。

---

## 立ち上げ動機

[colleague-skill](https://github.com/titanwings/colleague-skill) は「特定の人を蒸留する」が可能であることを示しました。[Nuwa](https://github.com/alchaincyf/nuwa-skill) はそれを極限まで押し進め、Munger / Naval / Musk のような大量の公開コーパスを持つ実在人物を蒸留しました。

しかし大半の専門家が直面しているのは「Musk とオンライン対話したい」ではなく、「シニア B2B SaaS PM のように私の PRD を審査してくれる判断者が欲しい」です。これは人の蒸留ではなく、**専門方法論の蒸留**です。

方法論蒸留の難しさは「LLM に専門家を演じさせる方法」ではありません — それは既知の解決済み問題で、効果も悪い（generation-protocol §0 スタンス 1-2 の vibes persona 批判を参照）。難しさは 2 つ：

1. **専門性がどう形成されるか**：critique corpora（ブログではなく）、standards docs（記述ではなく）、failure cases（成功談ではなく）の蓄積から
2. **専門性がどう検証されるか**：6 つの check 全通過によって（内容 4：stereotype / critique / refusal / trade-off + 構造 2：anchor consistency / family-specific check）、「正しそうだと感じる」ではなく

`luban-skill` はこの 2 点を実行可能な protocol にエンジニアリングしました。

**luban は道具を作る。しかし道具は空想から生まれず、職人が critique で失敗し、標準を蓄積し、失敗事例を記録する中で育つ。luban-skill はそのプロセスのメタツールです。**

---

## ステータス & Changelog

v0.3.0 がリリース済み — 方法論内部化リファクタが完了し、luban は単独で稼働します。完全なバージョン履歴は [CHANGELOG.md](CHANGELOG.md)、アーキテクチャ決定ドラフトは [ARCHITECTURE_v0.2.md](ARCHITECTURE_v0.2.md) を参照。

---

## 謝辞と参考

luban はゼロから生えてきたものではありません。以下の作品はそれぞれ「ロール / ペルソナ / 専門能力をどうエンジニアリングするか」の一部を解決しています。luban はその肩の上に立ち、別の路線（個人ではなく sub-specialty 方法論の蒸留、persona portability は扱わない）を選びました。

- **ペルソナ思想 — [DeepPersona: A Generative Engine for Scaling Deep Synthetic Personas](https://arxiv.org/abs/2511.07338)** (Wang et al., 2025)
  Taxonomy-first + progressive specification の 2 段階フレームワーク。luban の Stage 1「Capability Taxonomy Mining」→ Stage 3「Progressive Specification (5:3:2 sampling)」は同根。違いは luban が LLM による taxonomy 空想生成を拒否し、critique corpora からの逆算を要求する点。

- **蒸留のインスピレーション — [Nuwa-skill](https://github.com/alchaincyf/nuwa-skill)** (alchaincyf)
  「特定の人を蒸留する」をエンジニアリングレベルまで持っていき、distillation-as-skill の実現可能性を実証。luban はその直交補集合：Nuwa が個人の mental model を蒸留するのに対し、luban は sub-specialty の方法論を蒸留。両者の役割分担は [SKILL.md §6](.claude/skills/luban-skill/SKILL.md) で明文化。

- **SOUL 構造 — [soul-protocol](https://github.com/qbtrix/soul-protocol)** (qbtrix)
  ポータブル AI アイデンティティの完全規範（メタデータ / OCEAN 人格 / 5 層メモリ / 状態管理 / .soul アーカイブ形式）。luban の `SOUL.md` はその極小サブセット — 「ペルソナ / voice / スタンス」の 3 点のみを保持し、memory / portability / state は扱わない。luban の関心は「専門的判断」で、portability は主流路にないため。

- **SOUL 手法 — [OpenClaw `SOUL.md` 概念](https://docs.openclaw.ai/concepts/soul)**
  "Where your agent's voice lives" — SOUL.md をペルソナ層独立ファイルとして位置付け。luban はこの位置付けを直接採用し、capability layer (SKILL.md) / identity layer (identity.json) と厳密に分離。

あなたの仕事が luban と関連していて、ここに記載されたい場合は issue を立ててください。

---

## License

MIT
