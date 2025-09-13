# azprofile-cli

Azure CLI プロファイルマネージャー - 複数のAzureアカウントを簡単に切り替え

## インストール

```bash
npm install -g azprofile-cli
```

またはソースからインストール：
```bash
git clone https://github.com/O6lvl4/azprofile.git
cd azprofile
npm install -g .
```

## クイックセットアップ（推奨）

**1回限りのセットアップ** でシームレスなプロファイル切り替えが可能になります：

```bash
# シェル統合の設定
azprofile setup ~/.zshrc    # zsh ユーザー向け
azprofile setup ~/.bashrc   # bash ユーザー向け

# シェルを再起動するか、以下を実行：
source ~/.zshrc
```

セットアップ後は、`azprofile use <profile>` を **source コマンドなし** で使用できます！

## 使い方

### 1. Azureプロファイルの追加

```bash
azprofile add work      # 仕事用プロファイルを追加
azprofile add personal  # 個人用プロファイルを追加
```

各プロファイルでAzureへのログインとサブスクリプション/テナントの選択が促されます。

### 2. プロファイルの切り替え（セットアップ後）

```bash
# 簡単なプロファイル切り替え - sourceコマンド不要！
azprofile use work
az account show      # 仕事用アカウント情報を表示

azprofile use personal  
az account show      # 個人用アカウント情報を表示
azd up              # 個人用プロファイルを使用
```

### 3. その他のコマンド

```bash
# 全プロファイルを一覧表示
azprofile list

# 現在のプロファイル状態を表示
azprofile current

# 特定のプロファイルで単一コマンドを実行
azprofile exec work -- az account show
azprofile exec personal -- azd deploy

# プロファイルの削除
azprofile remove work
```

## 手動使用（セットアップなし）

セットアップを実行していない場合は、`source` を使用する必要があります：

```bash
source azprofile use work
az account show  # 仕事用プロファイルを使用
```

## 使用例のワークフロー

```bash
# 1. 1回限りのセットアップ
azprofile setup ~/.zshrc
source ~/.zshrc

# 2. 異なるAzureアカウント用のプロファイルを追加
azprofile add work
# 仕事用アカウントでログイン、仕事用サブスクリプションを選択

azprofile add personal
# 個人用アカウントでログイン、個人用サブスクリプションを選択

# 3. プロファイルの簡単な切り替え
azprofile use work
az resource list      # 仕事用リソースを一覧表示

azprofile use personal
azd up               # 個人用サブスクリプションにデプロイ

# 4. 現在の状態を確認
azprofile list       # 全プロファイルを表示
azprofile current    # アクティブなプロファイル情報を表示
```

## 機能

- ✅ **実行時にNode.jsへの依存なし**（純粋なシェルスクリプト）
- ✅ **シェル統合によるシームレスなプロファイル切り替え**
- ✅ **分離された設定ディレクトリによる複数Azureアカウント管理**
- ✅ **`az` と `azd` の両コマンドに対応**
- ✅ **自動ログインプロンプト付きの対話的プロファイル作成**
- ✅ **永続的および一時的なプロファイル切り替え**
- ✅ **クリーンでカラフルなCLIインターフェース**
- ✅ **クロスシェル対応**（bash、zsh）

## 仕組み

azprofile-cliは `AZURE_CONFIG_DIR` 環境変数を使用して、各プロファイル用に独立したAzure CLI設定ディレクトリを管理します。これにより、完全に分離されたAzure認証コンテキストを持つことができます。

### プロファイルストレージ

- プロファイルは `~/.azure-profiles/<profile-name>/` に保存されます
- 各プロファイルは独立したAzure CLI設定を含みます
- 設定は `~/.azure-profiles/config` に保存されます

### シェル統合

`azprofile setup` コマンドは、シェル設定にラッパー関数を追加します：
- `azprofile use <profile>` コマンドを傍受
- `AZURE_CONFIG_DIR` 環境変数を自動設定
- その他のコマンドは元のazprofileバイナリに渡します

## 要件

- **Bash >= 3.0** または **Zsh**
- **Azure CLI**（`az`）がインストール済み
- **Azure Developer CLI**（`azd`）- azdコマンド用（オプション）

## トラブルシューティング

### プロファイル切り替えが動作しない

セットアップを実行したことを確認してください：
```bash
azprofile setup ~/.zshrc  # または ~/.bashrc
source ~/.zshrc           # シェルを再起動
```

### シェルでの解析エラー

再インストールしてみてください：
```bash
npm uninstall -g azprofile-cli
npm install -g azprofile-cli
azprofile setup ~/.zshrc
```

### 環境変数が設定されない

セットアップが成功したかを確認：
```bash
type azprofile              # 関数として表示されるはず
azprofile use <profile>
echo $AZURE_CONFIG_DIR     # プロファイルパスが表示されるはず
```

## ライセンス

MIT