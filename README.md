### Railsチュートリアルの、Macの場合の仮想環境（Vagrant）及びGitHubを使った環境構築、の自分用補足メモ（2020年５月）

#### 前提: ドットインストールの次のレッスンの受講
* [ローカル開発環境の構築 [macOS編]](https://dotinstall.com/lessons/basic_localdev_mac_v2)

* [【旧版】ローカル開発環境の構築 » #10 PostgreSQLを導入してみよう](https://dotinstall.com/lessons/basic_local_development_v2/24813)
```
[vagrant@localhost ~]$ sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-6-x86_64/pgdg-redhat-repo-latest.noarch.rpm
[vagrant@localhost ~]$ sudo yum install postgresql12
[vagrant@localhost ~]$ sudo yum install postgresql12-server
[vagrant@localhost ~]$ sudo service postgresql-12 initdb
[vagrant@localhost ~]$ sudo chkconfig postgresql-12 on
[vagrant@localhost ~]$ sudo service postgresql-12 start
[vagrant@localhost ~]$ psql --version
```
* [Ruby on Rails 5入門 » #01 Ruby on Railsを使ってみよう](https://dotinstall.com/lessons/basic_rails_v3/41801)
```
[vagrant@localhost ~]$ cd ~/.rbenv
[vagrant@localhost .rbenv]$ git pull origin master
[vagrant@localhost .rbenv]$ cd ~/.rbenv/plugins/ruby-build 
[vagrant@localhost ruby-build]$ git pull origin master
[vagrant@localhost ruby-build]$ rbenv install 2.6.5
[vagrant@localhost ruby-build]$ rbenv global 2.6.5
[vagrant@localhost ruby-build]$ rbenv rehash
[vagrant@localhost ruby-build]$ ruby -v
[vagrant@localhost ruby-build]$ gem install rails -v 5.1.3 --no-document
[vagrant@localhost ruby-build]$ rails --version
```
* [Heroku入門 » #04 Heroku Toolbeltをインストールしよう](https://dotinstall.com/lessons/basic_heroku/28804)
```
[vagrant@localhost ruby-build]$ wget https://cli-assets.heroku.com/branches/stable/heroku-linux-amd64.tar.gz -O heroku.tar.gz
[vagrant@localhost ruby-build]$ mkdir -p /usr/local/lib
[vagrant@localhost ruby-build]$ sudo tar -xvzf heroku.tar.gz -C /usr/local/lib
[vagrant@localhost ruby-build]$ sudo curl https://cli-assets.heroku.com/install.sh | sh
```

#### 最初のエラーが発生
```
This script requires superuser access.
You will be prompted for your password by sudo.
Your path is missing /usr/local/bin, you need to add this to use this installer.
```
→ ルートユーザー権限とパス設定が必要<br>
【解決法参考サイト】[Qiita: Vagrant環境でHeroku CLIがインストールできない @karley](https://qiita.com/karlley/items/c423d02eee2292dab1f9)
```
[vagrant@localhost ruby-build]$ sudo su
[root@localhost ruby-build]# export PATH=/usr/local/bin:$PATH
[root@localhost ruby-build]# curl https://cli-assets.heroku.com/install.sh | sh
```
たくさんエラー文が出るが後ほど解決するので、とりあえずルートユーザーから外れる。
```
[root@localhost ruby-build]# exit
```

#### 次に、Railsチュートリアル[1.2.1 開発環境](https://railstutorial.jp/chapters/beginning?version=5.1#sec-development_environment)から[YassLab Inc.](https://github.com/yasslab)の[仮想環境 (Vagrant) を使った環境構築](https://github.com/yasslab/railstutorial.jp_starter_kit)に飛んだ。
1. [本ツールを使った環境構築の手順](https://github.com/yasslab/railstutorial.jp_starter_kit#本ツールを使った環境構築の手順)から[スターターキット](https://github.com/yasslab/railstutorial.jp_starter_kit/archive/master.zip)をダウンロードし、Zip ファイルを展開
2. 動作確認
```
[vagrant@localhost ruby-build]$ git clone https://github.com/yasslab/sample_apps.git
[vagrant@localhost ruby-build]$ cd sample_apps/5_1_2/ch14
[vagrant@localhost ch14]$ bundle install
```
#### ここで、２個目のエラーが発生
```
An error occurred while installing pg (0.18.4), and Bundler cannot continue.
Make sure that `gem install pg -v '0.18.4' --source 'https://rubygems.org/'` succeeds before bundling.
```
→ postgresqlのインストールが必要<br>
【解決法参考サイト】[Qiita: `gem install pg` が失敗するときの対処法 @tdrk](https://qiita.com/tdrk/items/812e7ea763080e147757)
```
[vagrant@localhost ch14]$ sudo su yum install postgresql-devel
[vagrant@localhost ch14]$ bundle install
```
7. 動作確認続き
```
[vagrant@localhost ch14]$ rails db:migrate
[vagrant@localhost ch14]$ rails test
[vagrant@localhost ch14]$ rails server
```

#### 次に、Railsのバージョンを5.1.3から5.1.6にアップグレード
```
[vagrant@localhost ch14]$ rails -v
[vagrant@localhost ch14]$ gem uninstall rails
[vagrant@localhost ch14]$ gem uninstall railties
[vagrant@localhost ch14]$ gem install rails -v 5.1.6 --no-document
[vagrant@localhost ch14]$ bundle install
[vagrant@localhost ch14]$ rails -v
```

#### 次に、Railsチュートリアル[1.3 最初のアプリケーション](https://railstutorial.jp/chapters/beginning?version=5.1#sec-the_hello_application)に進んだ。

#### 次に、Railsチュートリアル[1.4 Gitによるバージョン管理](https://railstutorial.jp/chapters/beginning?version=5.1#sec-version_control)に進み、GitHubでアカウント作成後、非公開型レポジトリhello_appを作成し、そこにプッシュを試みた。
```
[vagrant@localhost hello_app]$ git remote add origin git@github.com:ユーザー名/hello_app.git
[vagrant@localhost hello_app]$ git push -u origin --all
```

#### ここで、３個目のエラーが発生
```
Permission denied (publickey).
fatal: The remote end hung up unexpectedly.
```
→ GitHubで、公開鍵の設定が必要<br>
【解決法参考サイト】[Qiita: GitHubでssh接続する手順〜公開鍵・秘密鍵の生成から〜 @shizuma](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

```
[vagrant@localhost hello_app]$ cd ~/.ssh
[vagrant@localhost .ssh]$ ssh-keygen -t rsa
```

#### GitHub内で公開鍵の設定を行った後、リポジトリ追加とリポジトリへのプッシュ
```
[vagrant@localhost .ssh]$ cd ~/environment/hello_app
[vagrant@localhost hello_app]$ git push -u origin --all
```

#### Railsチュートリアル[1.5 デプロイする](https://railstutorial.jp/chapters/beginning?version=5.1#sec-deploying)に進み、Herokuのバージョンを確認した。
```
[vagrant@localhost hello_app]$ heroku --version
```

#### ここで、４個目のエラーが発生
```
/usr/local/lib/heroku/node_modules/@oclif/command/lib/index.js:3
const path = require("path");
^^^^^<
SyntaxError: Use of const in strict mode.
    at Module._compile (module.js:439:25)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
    at Module.require (module.js:364:17)
    at require (module.js:380:17)
    at Object.<anonymous> (/usr/local/lib/heroku/bin/run:5:1)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
```
→ Node.jsのバージョンアップが必要<br>
【解決法参考サイト】[DEV Community: How to Upgrade and update Nodejs via NPM Centos 6 by TeddyZugana](https://dev.to/kevinmel2000/how-to-upgrade-and-update-nodejs-via-npm-centos-6-3omm)
```
[vagrant@localhost hello_app]$ sudo yum install gcc gcc-c++ make
[vagrant@localhost hello_app]$ sudo yum install epel-release
[vagrant@localhost hello_app]$ sudo yum install nodejs
[vagrant@localhost hello_app]$ strings /usr/lib64/libstdc++.so.6 | grep GLIBC
[vagrant@localhost hello_app]$ npm cache clean -f
[vagrant@localhost hello_app]$ npm install -g n
```

#### ここで、さらに５個目のエラーが発生
```
npm http GET https://registry.npmjs.org/n
npm http GET https://registry.npmjs.org/n
npm http GET https://registry.npmjs.org/n
npm ERR! Error: CERT_UNTRUSTED
npm ERR!     at SecurePair.<anonymous> (tls.js:1430:32)
npm ERR!     at SecurePair.emit (events.js:92:17)
npm ERR!     at SecurePair.maybeInitFinished (tls.js:1029:10)
npm ERR!     at CleartextStream.read [as _read] (tls.js:521:13)
npm ERR!     at CleartextStream.Readable.read (_stream_readable.js:341:10)
npm ERR!     at EncryptedStream.write [as _write] (tls.js:418:25)
npm ERR!     at doWrite (_stream_writable.js:226:10)
npm ERR!     at writeOrBuffer (_stream_writable.js:216:5)
npm ERR!     at EncryptedStream.Writable.write (_stream_writable.js:183:11)
npm ERR!     at write (_stream_readable.js:602:24)
npm ERR! If you need help, you may report this log at:
npm ERR!     <http://github.com/isaacs/npm/issues>
npm ERR! or email it to:
npm ERR!     <npm-@googlegroups.com>

npm ERR! System Linux 2.6.32-754.29.1.el6.x86_64
npm ERR! command "node" "/usr/bin/npm" "install" "-g" "n"
npm ERR! cwd /home/vagrant/environment/hello_app
npm ERR! node -v v0.10.48
npm ERR! npm -v 1.3.6
npm ERR! 
npm ERR! Additional logging details can be found in:
npm ERR!     /home/vagrant/environment/hello_app/npm-debug.log
npm ERR! not ok code 0
```
→ https のレジストリに対するSSL鍵のバリデーション機能の無効化が必要<br>
【解決法参考サイト】[@YuG1224 BLOG: npm install で SSL Error になった時の対処法。](https://blog.yug1224.com/archives/563d9b67bf652a600632d01e/)
```
[vagrant@localhost hello_app]$ sudo npm config set strict-ssl false
[vagrant@localhost hello_app]$ sudo npm install -g n
[vagrant@localhost hello_app]$ sudo npm config set strict-ssl true
```
もう一度、気を取り直して、Node.jsのバージョンアップを継続。
```
[vagrant@localhost hello_app]$ sudo n stable
```
次のコードのフォルダ名は、`[vagrant@localhost hello_app]$ sudo n stable`で`mkdir`されたフォルダ名に合わせる。
```
[vagrant@localhost hello_app]$ sudo ln -sf /usr/local/n/versions/node/12.16.3/bin/node /usr/bin/node
[vagrant@localhost hello_app]# node -v
```

#### ここで、６個目のエラーが発生
```
node: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.14' not found (required by node)
node: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.18' not found (required by node)
node: /usr/lib64/libstdc++.so.6: version `CXXABI_1.3.5' not found (required by node)
node: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.15' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.16' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.17' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by node)
```
→ CentOS6で用意されている「gcc」はバージョンが古いため、「gcc」のソースからのビルド、新しいバージョンへの更新、ライブラリの用意が必要<br>
【解決法参考サイト】[Minecraft.server-memo.net: gccの更新](https://minecraft.server-memo.net/mc-javaedition-install/#gcc)
```
[vagrant@localhost hello_app]$ sudo yum install gcc
[vagrant@localhost hello_app]$ sudo yum install gcc-c++
[vagrant@localhost hello_app]$ curl -LO http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-9.3.0/gcc-9.3.0.tar.gz
[vagrant@localhost hello_app]$ curl -LO http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-9.3.0/sha512.sum
[vagrant@localhost hello_app]$ sha512sum --check sha512.sum
[vagrant@localhost hello_app]$ sudo tar xzfv gcc-9.3.0.tar.gz -C /usr/local/src
[vagrant@localhost hello_app]$ cd /usr/local/src/gcc-9.3.0/
[vagrant@localhost gcc-9.3.0]$ sudo ./contrib/download_prerequisites
[vagrant@localhost gcc-9.3.0]$ sudo mkdir build
[vagrant@localhost gcc-9.3.0]$ cd build
[vagrant@localhost build]$ sudo ../configure --enable-languages=c,c++ --prefix=/usr/local --disable-bootstrap --disable-multilib
[vagrant@localhost build]$ sudo make
[vagrant@localhost build]$ make　install
[vagrant@localhost build]$ gcc --version
[vagrant@localhost build]$ cat /etc/ld.so.conf
[vagrant@localhost build]$ cd /etc/ld.so.conf.d
[vagrant@localhost ld.so.conf.d]$ sudo su
[root@localhost ld.so.conf.d]# vi usr_local_lib64.conf
[root@localhost ld.so.conf.d]# ldconfig
```

#### ここで、７個目のエラーが発生
```
ldconfig: /usr/local/lib64/libstdc++.so.6.0.28-gdb.py はELFファイルではありません - 間違ったmagicバイトをファイルの先頭に持っています.
```
→ 【解決法参考サイト】に記載のとおり、
> 該当ファイルの名前を変更して対応します。
```
[vagrant@localhost ld.so.conf.d]$ sudo mv /usr/local/lib64/libstdc++.so.6.0.25-gdb.py  /usr/local/lib64/back_libstdc++.so.6.0.25-gdb.py
```

#### ここで、８個目のエラーが発生
```
mv: cannot stat `/usr/local/lib64/libstdc++.so.6.0.25-gdb.py': そのようなファイルやディレクトリはありません
```
→ ７個目のエラーの時のファイル名（so.6.0.28）と８個目のエラーのファイル名の一致が必要
```
[vagrant@localhost ld.so.conf.d]$ sudo mv /usr/local/lib64/libstdc++.so.6.0.28-gdb.py  /usr/local/lib64/back_libstdc++.so.6.0.28-gdb.py
[vagrant@localhost ld.so.conf.d]$ sudo ldconfig
```

#### 改めて、Herokuのバージョンを確認
```
[vagrant@localhost ld.so.conf.d]$ heroku --version
```

#### 少しエラーが減った。あと少し。
```
node: /lib64/libc.so.6: version `GLIBC_2.16' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.17' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by node)
```
→ glibcのアップデートが必要
【解決法参考サイト】<br>
[teratail: nodeが古いバージョンになってしまいアップデートが効かない by yosuke_narumi](https://teratail.com/questions/231008)<br>
[GitHubGist: update glibc to 2.17 for CentOS 6 by harv/glibc-2.17_centos6.sh](https://gist.github.com/harv/f86690fcad94f655906ee9e37c85b174#file-glibc-2-17_centos6-sh)
```
[vagrant@localhost ld.so.conf.d]$ sudo wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-2.17-55.el6.x86_64.rpm
[vagrant@localhost ld.so.conf.d]$ sudo wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-common-2.17-55.el6.x86_64.rpm
[vagrant@localhost ld.so.conf.d]$ sudo wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-devel-2.17-55.el6.x86_64.rpm
[vagrant@localhost ld.so.conf.d]$ sudo wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-headers-2.17-55.el6.x86_64.rpm
[vagrant@localhost ld.so.conf.d]$ sudo rpm -Uvh glibc-2.17-55.el6.x86_64.rpm glibc-common-2.17-55.el6.x86_64.rpm glibc-devel-2.17-55.el6.x86_64.rpm glibc-headers-2.17-55.el6.x86_64.rpm
```

#### 改めて、Herokuのバージョンを確認
```
[vagrant@localhost ~]$ heroku --version
```
```
heroku/7.40.0 linux-x64 node-v12.16.3
```
準備が整いました。出発進行！
