---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認してドキュメント標準を追加し、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] には、必要なコードだけでなく、その構成方法も記述できます。フィルター機能を実装したので、このレッスンでは Copilot が利用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた instruction file がエージェントにどのように渡るかを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数は、`db` 引数を注入できる場合はその点も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro component は `Props` の契約を説明し、関連するコードを変更した場合はコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

instruction file を使い、これらの開発プラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## Instruction file

Custom instruction を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

Instruction file には次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送られるファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの instruction 形式とそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[custom instruction のサポート情報][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでに instruction file が含まれています。変更を加える前に、既存の内容を確認して効果を比べられるようにします。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディングガイダンスを確認します。この指示は、リポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob を設定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクトファイルへのリンクを確認します。これにより、大きな instruction を小さな再利用可能なファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルにはよい出発点となる情報がありますが、まだ不足があります。新たに生成する TypeScript に TSDoc コメントが追加されるよう、中心となる `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. セクションの最後に、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトし、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   会話の選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスに応じた instruction file がエージェントにどのように渡るかを確認した。
- コーディング標準に従うよう instruction file を更新した。
- instruction file がコードに与える影響を確認した。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用の custom instruction の追加][instruction-files]
- [Custom instruction のサポート情報][custom-instructions-support]
- [Custom instruction を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
