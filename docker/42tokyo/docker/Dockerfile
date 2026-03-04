# ============================================================
#  Dockerfile — Ubuntu 22.04 LTS (jammy) 開発環境複製
#  ベース: ubuntu:22.04
#  ※ GUIはコンテナの性質上、X11フォワーディングで対応
# ============================================================

FROM ubuntu:22.04

# タイムゾーン設定（対話プロンプトを抑制）
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

LABEL maintainer="kinamura"
LABEL description="Ubuntu 22.04 dev environment (42School)"

# ------------------------------------------------------------
#  [1/7] システムアップデート
# ------------------------------------------------------------
RUN apt-get update -y && apt-get upgrade -y

# ------------------------------------------------------------
#  [2/7] 開発ツール (build-essential / GCC / Clang / CMake)
# ------------------------------------------------------------
RUN apt-get install -y \
	build-essential \
	gcc gcc-10 gcc-11 gcc-12 \
	g++ g++-10 g++-11 g++-12 \
	make \
	cmake \
	clang-12 clang-tidy-12 clang-tools-12 \
	xxd \
	rake \
 && apt-get clean

# ------------------------------------------------------------
#  [3/7] ネットワーク / SSH ツール
# ------------------------------------------------------------
RUN apt-get install -y \
	curl \
	wget \
	git \
	netcat-openbsd \
	openssh-client \
	openssh-server \
	ufw \
 && apt-get clean

# ------------------------------------------------------------
#  [4/7] 言語ランタイム (Python / Ruby / Node.js)
# ------------------------------------------------------------
RUN apt-get install -y \
	python3 \
	python3-pip \
	python3-dev \
	ruby \
	nodejs \
	npm \
 && apt-get clean

# ------------------------------------------------------------
#  [5/7] PHP 8.1
# ------------------------------------------------------------
RUN apt-get install -y \
	php8.1 \
	php8.1-curl \
	php-curl \
 && apt-get clean

# ------------------------------------------------------------
#  [6/7] GUI サポート (X11 フォワーディング用)
#  ※ VirtualBox仮想ディスプレイはコンテナ非対応のため
#	X11フォワーディングで代替 (ssh -X / DISPLAY環境変数)
# ------------------------------------------------------------
RUN apt-get install -y \
	xorg \
	x11-apps \
	gnome-terminal \
	firefox \
	chromium-browser \
	dbus-x11 \
 && apt-get clean

# ------------------------------------------------------------
#  [7/7] pip パッケージ (42School ツール)
# ------------------------------------------------------------
RUN pip3 install --break-system-packages \
	norminette==3.3.58 \
	c-formatter-42 \
	GitPython \
	speedtest-cli \
	rich \
	supervisor \
	halo \
	tqdm \
	termcolor \
	PyYAML

# ------------------------------------------------------------
#  SSH サーバ設定
# ------------------------------------------------------------
RUN mkdir /var/run/sshd \
 && echo 'root:vagrant' | chpasswd \
 && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
 && sed -i 's/#X11Forwarding yes/X11Forwarding yes/' /etc/ssh/sshd_config

# ------------------------------------------------------------
#  作業ユーザ作成 (vagrant に合わせて)
# ------------------------------------------------------------
RUN useradd -m -s /bin/bash vagrant \
 && echo 'vagrant:vagrant' | chpasswd \
 && usermod -aG sudo vagrant

USER vagrant
WORKDIR /home/vagrant

# ------------------------------------------------------------
#  ポート
# ------------------------------------------------------------
EXPOSE 22

# ------------------------------------------------------------
#  起動
# ------------------------------------------------------------
CMD ["/usr/sbin/sshd", "-D"]