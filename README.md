### Railsチュートリアルの、Macの場合の仮想環境（Vagrant）及びGitHubを使った環境構築、の自分用補足メモ（2020年４月）

#### まず、Railsチュートリアル[1.2.1 開発環境](https://railstutorial.jp/chapters/beginning?version=5.1#sec-development_environment)から[YassLab Inc.](https://github.com/yasslab)の[仮想環境 (Vagrant) を使った環境構築](https://github.com/yasslab/railstutorial.jp_starter_kit)に飛んだ。
1. [本ツールを使った環境構築の手順](https://github.com/yasslab/railstutorial.jp_starter_kit#本ツールを使った環境構築の手順)から[スターターキット](https://github.com/yasslab/railstutorial.jp_starter_kit/archive/master.zip)をダウンロードし、Zip ファイルを展開
2. VirtualBoxをインストールする
3. Vagrantをインストールする
4. Vagrantfile があるディレクトリに移動して、`vagrant up`を実行する
5. `vagrant ssh`でゲストOSにログインする
6. `ruby --version`や`rails --version`でちゃんと動くか確かめる
```
  ruby 2.6.2p47 (2019-03-13 revision 67232) [x86_64-linux]
  Rails 5.1.7
```
7. 動作確認
```
[vagrant@localhost ~]$ git clone https://github.com/yasslab/sample_apps.git
[vagrant@localhost ~]$ cd sample_apps/5_1_2/ch14
[vagrant@localhost ch14]$ bundle install
```
#### ここで、１個目のエラーが発生
```
An error occurred while installing pg (0.18.4), and Bundler cannot continue.
Make sure that `gem install pg -v '0.18.4' --source 'https://rubygems.org/'` succeeds before bundling.
```
→ postgresqlのインストールが必要<br>
【解決法】
【参考サイト】[Qiita: `gem install pg` が失敗するときの対処法 @tdrk](https://qiita.com/tdrk/items/812e7ea763080e147757)
```
[vagrant@localhost ch14]$ cd
[vagrant@localhost ~]$ sudo su
[root@localhost vagrant]# yum install postgresql-devel
[root@localhost ~]# exit
```
7. 動作確認続き
```
[vagrant@localhost ~]$ cd sample_apps/5_1_2/ch14
[vagrant@localhost ch14]$ bundle install
[vagrant@localhost ch14]$ rails db:migrate
[vagrant@localhost ch14]$ rails test
[vagrant@localhost ch14]$ rails server
```

#### 次に、Railsのバージョンを5.1.7から5.1.6にダウングレードした。
【参考サイト】[StackOverflow: How to downgrade my rails version?](https://stackoverflow.com/questions/28082120/how-to-downgrade-my-rails-version)
```
[vagrant@localhost ch14]$ cd
[vagrant@localhost ~]$ rails --version
[vagrant@localhost ~]$ gem uninstall rails
[vagrant@localhost ~]$ gem uninstall railties
[vagrant@localhost ~]$ gem install rails -v 5.1.6
[vagrant@localhost ~]$ rails -v
```

#### 次に、Railsチュートリアル[1.3 最初のアプリケーション](https://railstutorial.jp/chapters/beginning?version=5.1#sec-the_hello_application)に進んだ。

#### 次に、Railsチュートリアル[1.4 Gitによるバージョン管理](https://railstutorial.jp/chapters/beginning?version=5.1#sec-version_control)に進み、GitHubでアカウント作成後、非公開型レポジトリhello_appを作成し、そこにプッシュを試みた。
```
[vagrant@localhost hello_app]$ git remote add origin git@github.com:ユーザー名/hello_app.git
[vagrant@localhost hello_app]$ git push -u origin --all
```

#### ここで、２個目のエラーが発生
```
**Permission denied (publickey).**
fatal: The remote end hung up unexpectedly.
```
→ GitHubで、公開鍵の設定が必要<br>
【解決法】【参考サイト】[Qiita: GitHubでssh接続する手順〜公開鍵・秘密鍵の生成から〜 @shizuma](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)
```
[vagrant@localhost hello_app]$ cd
```

#### 次に、Railsチュートリアル[1.5 デプロイする](https://railstutorial.jp/chapters/beginning?version=5.1#sec-deploying)に進み、Herokuのバージョンを確認した。
```
[vagrant@localhost hello_app]$ heroku --version
```

#### ここで、３個目のエラーが発生
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
【参考サイト】[DEV Community: How to Upgrade and update Nodejs via NPM Centos 6 by TeddyZugana](https://dev.to/kevinmel2000/how-to-upgrade-and-update-nodejs-via-npm-centos-6-3omm)
```
[vagrant@localhost hello_app]$ cd
[vagrant@localhost ~]$ sudo su
[root@localhost vagrant]# yum install gcc gcc-c++ make
[root@localhost vagrant]# yum install epel-release
[root@localhost vagrant]# yum install nodejs
[root@localhost vagrant]# strings /usr/lib64/libstdc++.so.6 | grep GLIBC
[root@localhost vagrant]# sudo npm cache clean -f
[root@localhost vagrant]# sudo npm install -g n
```
#### ここで、さらに４個目のエラーが発生
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
→ https のレジストリに対するSSL鍵のバリデーション機能を外す必要<br>
【解決法】【参考サイト】[@YuG1224 BLOG: npm install で SSL Error になった時の対処法。](https://blog.yug1224.com/archives/563d9b67bf652a600632d01e/)
```
[root@localhost vagrant]# npm config set strict-ssl false
[root@localhost vagrant]# npm install -g n
[root@localhost vagrant]# npm config set strict-ssl true
```
もう一度、気を取り直して、Node.jsのバージョンアップを継続。
```
[root@localhost vagrant]# sudo n stable
[root@localhost vagrant]# sudo ln -sf /usr/local/n/versions/node/11.8.0/bin/node /usr/bin/node
[root@localhost vagrant]# node -v
```
#### ここで、さらに５個目のエラーが発生
```
bash: /usr/bin/node: そのようなファイルやディレクトリはありません
```
→ `[root@localhost vagrant]# sudo n stable`で`mkdir`されたフォルダ名と
`[root@localhost vagrant]# sudo ln -sf /usr/local/n/versions/node/11.8.0/bin/node /usr/bin/node`のフォルダ名の一致が必要
```
[root@localhost vagrant]# sudo ln -sf /usr/local/n/versions/node/12.16.3/bin/node /usr/bin/node
[root@localhost vagrant]# node -v
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



