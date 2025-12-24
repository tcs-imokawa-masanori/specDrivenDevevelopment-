# CLAUDE.md - Spec-Driven Development Project
# 仕様駆動開発プロジェクト

## Project Overview / プロジェクト概要

This repository is a **Spec-Driven Development (SDD) methodology guide and template** for migrating .NET applications to Next.js/React frontend with Java backend.

このリポジトリは、.NETアプリケーションをNext.js/ReactフロントエンドとJavaバックエンドに移行するための**仕様駆動開発（SDD）方法論ガイドとテンプレート**です。

## Technology Stack / 技術スタック

- **Source:** .NET (C#, ASP.NET Core)
- **Target Frontend:** Next.js 14+, React 18+, TypeScript
- **Target Backend:** Java 17+, Spring Boot 3.x
- **Development Tools:** Visual Studio, Visual Studio Code

## Directory Structure / ディレクトリ構造

```
.
├── CLAUDE.md                    # This file / このファイル
├── README.md                    # Quick reference / クイックリファレンス
├── SPEC-DRIVEN-APPROACH.md      # Core SDD methodology / SDDコア方法論
├── DOTNET-NEXTJS-MIGRATION-SPEC.md  # Migration-specific guide / 移行ガイド
├── .specs/                      # Specification files / 仕様ファイル
│   ├── templates/               # Reusable templates / 再利用テンプレート
│   │   ├── migration-spec.md    # Migration spec template
│   │   └── tech-mapping.md      # Technology mapping template
│   └── migration/               # Active migration specs
└── .claude/                     # Claude Code configuration
    └── commands/                # Custom slash commands
        ├── analyze-dotnet.md
        ├── create-migration-spec.md
        ├── generate-from-spec.md
        └── sync-api-types.md
```

## Available Commands / 利用可能なコマンド

| Command | Description | 説明 |
|---------|-------------|------|
| `/analyze-dotnet [path]` | Analyze .NET source code | .NETソースコードを分析 |
| `/create-migration-spec [name]` | Create new migration spec | 新しい移行仕様を作成 |
| `/generate-from-spec [id]` | Generate code from spec | 仕様からコードを生成 |
| `/sync-api-types [path]` | Sync types between FE/BE | FE/BE間の型を同期 |

## Workflow / ワークフロー

### 1. Analyze (.NET Source) / 分析
```
/analyze-dotnet path/to/controller.cs
```

### 2. Specify (Create Spec) / 仕様作成
```
/create-migration-spec auth-module
```

### 3. Plan (Design Architecture) / 計画
- Complete the plan.md in the spec folder
- Review API contracts
- Validate technology mappings

### 4. Execute (Generate Code) / 実行
```
/generate-from-spec MIG-001
```

## Patterns & Conventions / パターンと規約

### Naming Conventions / 命名規則

| Layer | .NET | Next.js | Java |
|-------|------|---------|------|
| Controller | `XxxController` | `route.ts` (App Router) | `XxxController` |
| Service | `IXxxService` | `useXxx` (hook) | `XxxService` |
| Repository | `IXxxRepository` | - | `XxxRepository` |
| DTO | `XxxDto` | `interface Xxx` | `record XxxDto` |

### File Organization / ファイル編成

- Keep specs in `.specs/` directory
- One spec folder per migration feature
- Use numbered prefixes: `001-`, `002-`, etc.
- Always include: spec.md, plan.md, tasks.md, mapping.md

## Best Practices / ベストプラクティス

1. **Spec First / 仕様優先**
   - Never code without a spec
   - Review specs before implementation

2. **API Contract / API契約**
   - Define OpenAPI specs early
   - Generate types from specs
   - Validate contracts with tests

3. **Incremental Migration / 段階的移行**
   - Migrate one feature at a time
   - Maintain backwards compatibility
   - Test thoroughly at each step

## References / 参考資料

- [Spec-Driven Approach Guide](./SPEC-DRIVEN-APPROACH.md)
- [Migration Guide](./DOTNET-NEXTJS-MIGRATION-SPEC.md)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
