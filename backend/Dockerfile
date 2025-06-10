# Python 3.12.10 + Alpine 3.21 ベース
FROM python:3.12.10-alpine3.21

# uvをインストール
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# タイムゾーンを東京に設定
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 必要パッケージのインストール
RUN apk update && apk add --no-cache \
    iproute2 \
    lsblk \
    procps \
    build-base \
    libffi-dev \
    musl-dev \
    linux-headers \
    curl \
    bash

# ポート公開
EXPOSE 3000

# アプリケーションディレクトリの設定
WORKDIR /backend
# アプリ本体を後でコピー
COPY . /backend
# # エントリポイントをコピーして権限を付与
RUN chmod +x ./docker-entrypoint.sh

# 非rootユーザーを作成
RUN adduser -D appuser && \
    chown -R appuser /backend
# 非rootユーザーに切り替え
USER appuser

# pyproject.toml, uv.lockがある前提
# 依存関係をインストール
RUN [ -d .venv ] || uv sync --frozen

# 起動コマンド
ENTRYPOINT ["./docker-entrypoint.sh"]
