# jammy-dev — Docker 環境

Ubuntu 22.04 LTS (jammy) の開発環境を Docker で複製したもの。

---

## ディレクトリ構成

```
jammy-dev/
├── docker/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── workspace/        # コンテナと共有する作業ディレクトリ
└── README.md
```

---

## 環境スペック

| 項目 | 値 |
|------|-----|
| ベースイメージ | `ubuntu:22.04` |
| ホスト名 | `jammy-dev` |
| ユーザ | `vagrant` / パスワード: `vagrant` |
| SSH ポート | `2222` (ホスト) → `22` (コンテナ) |

---

## インストール済みパッケージ

| カテゴリ | パッケージ |
|---------|-----------|
| 開発ツール | build-essential, gcc/g++ 10〜12, make, cmake, clang-12, rake, xxd |
| ネットワーク / SSH | curl, wget, git, netcat-openbsd, openssh-server, ufw |
| 言語ランタイム | Python 3.10, Ruby 3.0, Node.js 12, PHP 8.1 |
| GUI (X11) | xorg, gnome-terminal, chromium-browser, firefox |
| pip | norminette, c-formatter-42, GitPython, rich, supervisor, halo, tqdm, termcolor, PyYAML |

---

## 使い方

### 起動

```bash
cd docker
docker compose up -d --build
```

### SSH ログイン

```bash
ssh -p 2222 vagrant@localhost
```

X11フォワーディング（GUI）を使う場合：

```bash
ssh -X -p 2222 vagrant@localhost
```

### コンテナに直接入る

```bash
docker compose exec dev bash
```

### 停止 / 削除

```bash
docker compose down        # 停止
docker compose down -v     # 停止 + ボリューム削除
```

---

## GUI を使う場合

コンテナはVirtualBox仮想ディスプレイ非対応のため、X11フォワーディングで代替。

| OS | 手順 |
|----|------|
| macOS | XQuartz をインストール → `xhost +localhost` |
| Linux | `xhost +local:docker` |
| Windows | VcXsrv または WSLg 経由 |

---

## 注意事項

- `workspace/` ディレクトリはホストとコンテナ間で同期されます
- snap パッケージ（chromium / firefox）はコンテナ非対応のため `apt` 版で代替しています
- SSH の root ログインは有効になっています（パスワード: `vagrant`）