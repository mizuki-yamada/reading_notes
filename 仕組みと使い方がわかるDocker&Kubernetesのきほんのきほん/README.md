# 仕組みと使い方がわかるDocker&Kubernetesのきほんのきほん


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [仕組みと使い方がわかるDocker&Kubernetesのきほんのきほん](#仕組みと使い方がわかるdockerkubernetesのきほんのきほん)
  - [01 Dockerとは何だろう](#01-dockerとは何だろう)
    - [01 Dockerって何だろう](#01-dockerって何だろう)
    - [02 サーバとDocker](#02-サーバとdocker)
  - [02 Dockerが動く仕組み](#02-dockerが動く仕組み)
    - [01 Dockerが動く仕組み](#01-dockerが動く仕組み)
    - [02 Docker Hubとイメージとコンテナ](#02-docker-hubとイメージとコンテナ)
    - [03 Dockerコンテナのライフサイクルとデータの保存](#03-dockerコンテナのライフサイクルとデータの保存)
    - [04 Dockerのメリットとデメリット](#04-dockerのメリットとデメリット)
  - [03 Dockerを使ってみよう](#03-dockerを使ってみよう)
    - [01 Dockerを使うには](#01-dockerを使うには)
    - [02 Dockerのインストール](#02-dockerのインストール)
      - [03 Dockerの操作方法とコマンドプロンプト／ターミナルの起動](#03-dockerの操作方法とコマンドプロンプトターミナルの起動)
  - [04 Dockerにコンテナを入れて動かしてみよう](#04-dockerにコンテナを入れて動かしてみよう)
    - [01 Docker Engineの起動と終了](#01-docker-engineの起動と終了)
    - [02 コンテナ操作の基本](#02-コンテナ操作の基本)
    - [03 コンテナの作成・削除と、起動・停止](#03-コンテナの作成削除と-起動停止)
    - [04 コンテナと通信](#04-コンテナと通信)
    - [05 コンテナ作成に慣れよう](#05-コンテナ作成に慣れよう)
    - [06 イメージの削除](#06-イメージの削除)

<!-- /code_chunk_output -->

## 01 Dockerとは何だろう

### 01 Dockerって何だろう
- 「データやプログラムを隔離できる」仕組みのこと
  - データ、プログラムだけでなく、OS（厳密にはOSではない）ごと隔離できる
- データやプログラムは「コンテナ」に入れることで独立できる
  - コンテナは物置みたいなもの
  - コンテナを使える仕組みがDocker
-  Dockerを動かすにはDocker Engineのインストールが必要
   -  コンテナでデータやプログラムを管理したい→Dockerが必要→Docker Engineが必要、という流れだと理解
-  コンテナはイメージから作る
   -  「〇〇のコンテナを作りたい」となったら「〇〇のイメージを使う」という流れになる、これはたぶんおまじない
   -  イメージはISOファイルに近い（CD、DVDをファイルに出力したもの。これがあれば元のCDやDVDを復元できる）。イメージがあればコンテナを復元できる、ということ
-  Dockerを動かすにはLinux OSが必要
-  データやプログラムを隔離したい理由
   -  プログラムは実行環境、ライブラリに依存して動いているため
   -  一部の都合で変更すると、他のものが動かなくなってしまう、ということがありうる
      -  サーバに何かのプログラムを同居させる場合、共有部分でトラブることがよくある
   -  この同居問題を解決してくれるのがDockerコンテナ

### 02 サーバとDocker
- サーバ…サービスを提供するもの
  - webサーバ：web機能を提供する
  - メールサーバ：メール機能を提供する
  - 複数の人がアクセスして使うのが、個人のノートPCと異なる
  - **サーバを作る＝あるソフトを入れて、その機能を持たせること**
- サーバには2種類ある
  - 機能としてのサーバ
    - webサーバ、メールサーバ、ファイルサーバ、etc...

サーバ | 特徴 
---------|----------
 webサーバ | webサイトの機能を提供するサーバ。HTMLファイル、画像ファイル、プログラムなどを置いておく。クライアントのブラウザがアクセスしてくると、それらのファイルを提供する
 メールサーバ | メールの送受信を担当するSMTPサーバと、クライアントにメールを受信させるPOPサーバの2つを合わせてメールサーバと呼ぶことが多い
 データベースサーバ | データを保存したり、検索したりするためのデータベースを置くサーバ。代表的なソフトはMySQL、PostgreSQLなど
 ファイルサーバ| ファイルを保存して、共有するサーバ
 DNSサーバ|IPアドレスと、ドメインを結びつけるDNS機能をもつサーバ
 DHCPサーバ|IPアドレスを自動的に振る機能を持つサーバ
 FTPサーバ|FTPプロトコルを使って、ファイルの送受信を行うサーバ。Webサーバと同居させることが多く、ファイルの設置に使う
 プロキシサーバ|通信を中継する役割を持つおサーバの総称。社内LANなど、インターネットから隔離された場所からインターネット上のサーバに接続するときに使う
 認証サーバ|ユーザを認証するためのサーバ。Windowsネットワークにログインするための「Active Directory」が有名

  - 物理的なマシンとしてのサーバ
    - EC2とかだよね、オンプレだと文字通り物理のサーバ、デスクトップのPCもそう
    - 機能としてのサーバを1つの物理的なマシンに複数同居させることが可能
- サーバのOSはLinuxが圧倒的優位
- コンテナを使えば、複数のサーバ機能を安全に同居させることができる
  - サーバマシン代の節約にもなる
- コンテナは持ち運べる
  - 同じ状態にチューニングしたコンテナを全員に配布し、開発環境をチームメンバ全員で統一するのも簡単
    - 各々にDockerが入っていさえすればいい
- Dockerは仮想化技術とイコールではない
  - 仮想化技術：VirtualBoxとか
    - 実質物理的なマシン、CPUとかも中身に含まれている
  - DockerはあくまでOSぽいものしかコンテナ上にない
    - OSの機能のいくつかは、ホストになっている（＝エンジンをインストールしている？）物理的なマシンに依存している
  - EC2も仮想化環境（VirtualBoxと同じ）。インスタンスはAMIというイメージから作るので、配布方法は似ている
- Dockerとホスティングサービス
  -  AWSのECSが有名どころ
  -  サーバを作ることなく、コンテナイメージを運用できる

## 02 Dockerが動く仕組み

### 01 Dockerが動く仕組み
- Dockerを使う時は、OSの上にDocker Engineを載せ、その上でコンテナを動かす
- コンテナの中に入っているのは「OSっぽいもの」
  - コンテナの初期状態は空っぽではなく、Linux OSぽいものが入った状態
    - 居酒屋で何も言わなくてもお通しが出てくるイメージ
  - OS：ソフトウェアなどプログラムの命令をハードウェアに伝える役割
    - 人間語を機械語に翻訳してくれるイメージ
    - カーネルとカーネルの周辺部分から構成されるのがOS
  - コンテナは完全に独立しているので、コンテナの中にあるプログラムの命令を土台（Docker Engineを入れているマシンのOS）がそのままでは受け取ることができない。受け取るために、コンテナの中にも専用のカーネル周辺部分を入れて、その子が土台のカーネルにコンテナ内部の命令を伝える役割を果たす。
    - コンテナ→Docker Engine→Linux OS→物理サーバ
      - コンテナのプログラムを物理サーバが実行するために、コンテナの初期状態としてカーネルの周辺部分がデフォルトで入っている
- DockerはLinux OSが土台であることが前提で成り立つ仕組み。コンテナの中に入れるソフトウェアもLinux用じゃないとだめ
  - Linuxはたいていサーバの文脈で出てくる→Dockerもサーバの文脈で話題に上がることが多い

### 02 Docker Hubとイメージとコンテナ
- イメージはコンテナの設計図
  - イメージからコンテナを作る
    - イメージは型。1つのイメージから複数のコンテナを生成できる
  - コンテナからもイメージを作成できる
    - 元のイメージの進化形を作れる
  - イメージ→コンテナの一方向しかない場合、複数コンテナを作った場合の改変が面倒（コンテナの数だけ作業が必要）
  - コンテナ→イメージの矢印があることで、イメージ1→コンテナ1→コンテナ1-1→イメージ1-1みたいなことができて、イメージ1-1から作ったコンテナ1-1をたくさん作れる
    - コンテナ→イメージの矢印がないと、イメージ1→コンテナ1→コンテナ1-1の作業をn回しないといけない
- イメージはDockerレジストリから入手するのが一般的（自作はしない）
  - Docker Hub, ECR, etc...
  - Docker HubはApple Storeみたいなもの、イメージがたくさん置いてある
  - イメージには、OSっぽいものが入っただけのものから複数のソフトウェアが入っているものまで、バラエティに富んでいる
- 1コンテナ1アプリ
  - セキュリティやメンテナンスのしやすさの観点から

### 03 Dockerコンテナのライフサイクルとデータの保存
- 基本は作っては捨てる
  - ×：1つのコンテナをアップデートしながら使い続ける
  - ○：アップデートされた新しいコンテナを使う
    - ＝新しいイメージを参照するようにする
  - 金継ぎではなく、スクラップアンドビルド
- コンテナのライフサイクル
  - 作る→起動する→停止する→破棄する→作る→・・・
- データの保存
  - コンテナは使ったら捨てるので、素直に考えればそのコンテナ内のデータも一緒に消えてなくなる
  - ので、Dockerをインストールしている物理的なマシン（土台）のディスクにマウントして、データを保存する
    - PCが壊れた時のために、ハードディスクにバックアップを取るのと同じイメージ

### 04 Dockerのメリットとデメリット
- メリット
  - 隔離できる
    - 独立している
    - イメージにできるので、レジストリから持ってくるだけでコンテナを使うことができる
      - イメージ化のその他のメリット：アップデートしやすい、入れ替えしやすい（イマイチと思ったら別のに乗り換えられる）、配布しやすい
    - コンテナに「カーネル」は不要
  - 1台の物理的なマシンに複数のサーバをのせられる
  - サーバの管理がしやすい
  - コマンド1つでサーバの構築ができる
- デメリット
  - コンテナの土台になっている物理的なマシンが死んだら、コンテナも死ぬ
- 使い道
  - 開発チームの環境を全員で揃えたい
  - 新バージョンを実験したい
  - 複数の同じサーバが必要になった


## 03 Dockerを使ってみよう

### 01 Dockerを使うには
- Docker Engineをインストールすれば、Dockerは使える
- Dockerを使う方法は3つ
  - Linuxマシンで使う
  - 仮想マシンやレンタル環境にDockerを入れて、WindowsやMacで操作する
    - AWSだとEC2
      - AWSでDockerというとECS, EKSの名前が上がるが、これらはオーケストレーションツールなので、これらを使う場合もDocker自体はEC2に構築することになる
  - WindowsやMac用のDockerを使う
    - ユーザからは見えない仮想のLinux環境を作ってそこでDocker Engineを動かす仕組み
    - 仮想マシンを使う方法と異なるのは、仮想マシンはユーザがソフトウェアをインストール→Linuxをインストールする必要があるが、WindowsやMac用のDockerはそれが不要
    - **仮想化やLinuxを意識する必要がない**

### 02 Dockerのインストール
- Docker Desktopをインストールした

#### 03 Dockerの操作方法とコマンドプロンプト／ターミナルの起動
- `docker run/stop {コンテナ名}`でコンテナの起動・停止ができる
- ターミナルの終了≠Docker Engineの終了。サーバでDockerを使う場合、Docker Engineを止めると、コンテナを全てシャットダウンすることになるので、基本は止めない
- 自分のPCではないLinuxサーバにインストールされたDocker Engineを動かしたい場合、そのサーバに対してSSHで接続する

## 04 Dockerにコンテナを入れて動かしてみよう

### 01 Docker Engineの起動と終了
- Docker Engineを止める＝サーバも止める、になるので、Engineは基本起動しっぱなしにしておく。
  - 自動起動の設定もオンにしておくのが無難
  - Docker Engineも止めると、コンテナも止まる。コンテナは自動起動しないので、Engineが止まったらコンテナはコンテナで再起動が必要

### 02 コンテナ操作の基本
- コンテナ操作の基本は`docker`コマンド
  - docker コマンド オプション 対象 引数
- `docker version`の結果
```bash
❯ docker version
Client:
 Cloud integration: v1.0.35+desktop.13
 Version:           26.0.0
 API version:       1.45
 Go version:        go1.21.8
 Git commit:        2ae903e
 Built:             Wed Mar 20 15:14:46 2024
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.29.0 (145265)
 Engine:
  Version:          26.0.0
  API version:      1.45 (minimum version 1.24)
  Go version:       go1.21.8
  Git commit:       8b79278
  Built:            Wed Mar 20 15:18:02 2024
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.6.28
  GitCommit:        ae07eda36dd25f8a1b98dfbf587313b99c0190bb
 runc:
  Version:          1.1.12
  GitCommit:        v1.1.12-0-g51d5e94
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
- 代表的なコマンド
![major commnad](./images/major_commnad.png)
  - 上位コマンドは省略もできる（昔の形式と互換性を保つため、今は`上位コマンド＋副コマンド`の形式に変更になっている）
- コマンドの種類
  - コンテナ操作関連...docker container {副コマンド} {オプション}
    - start, stop, create, run, rm, exec, ls, cp, commit
  - イメージ操作関連...docker image {副コマンド} {オプション}
    - pull, rm, ls, build
  - ボリューム操作関連...docker volume {副コマンド} {オプション} 
    - create, inspect, ls, prune, rm
  - ネットワーク操作関連...docker network {副コマンド} {オプション}
    - connect, disconnect, create, inspect, ls, prune, rm 
  - その他の上位コマンド
    - checkpoint, node, plugin, secret, service, stack, swarm, system
  - 単独コマンド
    - login, logout, search, version

### 03 コンテナの作成・削除と、起動・停止
- docker pull + docker create + docker start = docker run
  - イメージのダウンロード＋コンテナの作成＋コンテナの起動
  - ![run](./images/run.png)
- docker stop コンテナ名
- docker rm コンテナ名
- デーモン：UNIXやLinuxで動くプログラムのうち、常に待ち受けて、裏で動き続けるプログラム。メーラーデーモン。
  - バックグラウンドで常に実行しているのが、デーモン（d）っで覚えとく
- docker ps
  - コンテナの起動状態を確認できる
- コンテナでApacheを起動してみよう
```bash
# runしてみる
# runの実行時にimageがない場合、自動でimageのダウンロードもしてくれる
❯ docker run --name apa000ex1 -d httpd
Unable to find image 'httpd:latest' locally
latest: Pulling from library/httpd
22d97f6a5d13: Pull complete 
855b568ab02c: Pull complete 
4f4fb700ef54: Pull complete 
c521cc2c4e8a: Pull complete 
98c8bced82bd: Pull complete 
bae159c85a0b: Pull complete 
Digest: sha256:36c8c79f900108f0f09fd4148ad35ade57cba0dc19d13f3d15be24ce94e6a639

# コンテナの稼働状況を確認
❯ docker ps                           
CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS         PORTS     NAMES
c6da48446045   httpd     "httpd-foreground"   2 minutes ago   Up 2 minutes   80/tcp    apa000ex1

# stop
❯ docker stop apa000ex1
apa000ex1

# docker ps
❯ docker ps            
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

❯ docker ps -a
CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS                      PORTS     NAMES
c6da48446045   httpd     "httpd-foreground"   4 minutes ago   Exited (0) 42 seconds ago             apa000ex1
# Exitedになっているので、コンテナは停止中なことがわかる

# rm
❯ docker rm apa000ex1                 
apa000ex1

❯ docker ps -a                         
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
# コンテナは何もなくなっている（削除済）
```

### 04 コンテナと通信
- ApacheはWebサーバを提供するソフトウェアなので、Apacheが動いているサーバにhtmlファイルなどを置くと、ブラウザからのアクセスが可能になる
- コンテナ外からのアクセスを可能にするために、ボート番号の指定が必要
- ホスト（コンテナが存在する物理的なマシン）のポートと、コンテナのポートを指定する
![communication](./images/communication.png)
- やってみよう
```bash
# ポート番号を指定して、docker run
❯ docker run --name apa000ex2 -d -p 8080:80 httpd
d4628ae52e9a18e426b1620018736cce55777df0c1889beab1d6893d795509a2 # コンテナIDが返ってくる

❯ docker ps                                      
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS                  NAMES
d4628ae52e9a   httpd     "httpd-foreground"   31 seconds ago   Up 31 seconds   0.0.0.0:8080->80/tcp   apa000ex2
```
- `http://localhost:8080/`にアクセスして、Apacheの初期画面が出てきたのでOK

### 05 コンテナ作成に慣れよう
- Webサーバ用のコンテナは、通信することが前提なので、ポート番号の指定が必須
  - コンテナを複数並列で立てるときは、物理マシンのポート番号は重複できないので、ずらす必要がある。コンテナ側のポート番号は重複していい。
  - Apache, nginxは覚えておこう
- データベース用のコンテナは、ルールバスワードの指定が必要
- プログラムの実行環境用コンテナもある
  - javaならopenjdk, pythonならpython、など
```bash
# 複数コンテナの起動、停止、削除はこうやってもできた
❯ docker stop apa000ex3  apa000ex4 apa000ex5
apa000ex3
apa000ex4
apa000ex5

❯ docker start apa000ex3  apa000ex4 apa000ex5
apa000ex3
apa000ex4
apa000ex5

❯ docker rm apa000ex3  apa000ex4 apa000ex5  
apa000ex3
apa000ex4
apa000ex5
```
- MySQLのコンテナを作ってみよう
  - ちゃんと動かすには引数の指定が必要
  - ここではルートパスワードのみを設定し、ちゃんと動いているかの確認はしない（docker psコマンドでの確認のみ）
```bash
❯ docker run --name mysql000ex7 -dit -e MYSQL_ROOT_PASSWORD=myrootpass mysql # -dit: バックグラウンド実行＆キーボード操作を可能にする
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
c6a0976a2dbe: Pull complete 
ae691a8c1b16: Pull complete 
8fe011e50abc: Pull complete 
c5c88c8fbaa8: Pull complete 
25d028b2b1f1: Pull complete 
bc284ef09f94: Pull complete 
00367d65ff82: Pull complete 
d7d4b7ff3dc2: Pull complete 
bafe7641a560: Pull complete 
d9b85ac37bb0: Pull complete 
Digest: sha256:f7a8e140a7d6d1e6e0c99eeb0489c50a186ee4ac44ff55323a176529b9a43d33
```

### 06 イメージの削除
- `docker image rm {image}`
```bash
❯ docker image rm httpd #imageのIDを指定して消すこともできる
Untagged: httpd:latest
Untagged: httpd@sha256:36c8c79f900108f0f09fd4148ad35ade57cba0dc19d13f3d15be24ce94e6a639
Deleted: sha256:73acb239a8eb7a9ef5b6848a5b01bb26cc03d2262056705876bf7ac6ad781f02
Deleted: sha256:37e83df10816d228adc72bc7fe72914bb229da49297eb015ecc7a491ac5673f2
Deleted: sha256:eb13937d3e0d1130837a90aaad81cd716a70cc3448e65f9c465c5ca0a9c47fe6
Deleted: sha256:630013052b48fbb0dfc8a01db6c815b16f4852860db09b018ae644d6654aa183
Deleted: sha256:6884f690ae00745ce1fa942438738fee1a94ce458265385f8d0c46a14e40392e
Deleted: sha256:02619b801b0cb5f3214954e026d3f73b2938cfb690dd6805a0657efe1b35d7f8

# 存在するイメージ一覧を確認
❯ docker image ls      
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
mysql        latest    5fc9b4335a45   4 days ago    635MB
nginx        latest    786a14303c96   11 days ago   193MB
```
- imageを消す前に、そのimageを使っているコンテナがないか、ある場合はそのコンテナを止めてからimageを消すこと