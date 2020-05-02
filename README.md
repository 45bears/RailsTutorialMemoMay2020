<h1>Railsチュートリアルにおける、仮想環境（Vagrant)を使った環境構築まとめ</h1>

1.2.1 開発環境　から　仮想環境 (Vagrant) を使った環境構築
https://github.com/yasslab/railstutorial.jp_starter_kit
に飛んだ。

本ツールを使った環境構築の手順　ということで、
<ol>
  <li>まずは下記リンクからスターターキットをダウンロードし、Zip ファイルを展開
  https://github.com/yasslab/railstutorial.jp_starter_kit/archive/master.zip</li>
  <li>VirtualBoxをインストールする</li>
  <li>Vagrantをインストールする</li>
  <li>Vagrantfile があるディレクトリに移動して、vagrant upを実行する</li>
  <li>vagrant ssh でゲストOSにログインする</li>
  <li><code>ruby --version</code> や <code>rails --version</code> でちゃんと動くか確かめる<br>
  ruby 2.6.2p47 (2019-03-13 revision 67232) [x86_64-linux]<br>
  Rails 5.1.7
  </li>
  <li>動作確認<br>
    <code>[vagrant@localhost ~]$ git clone https://github.com/yasslab/sample_apps.git</code><br>
    <code>[vagrant@localhost ~]$ cd sample_apps/5_1_2/ch14</code><br>
    <code>[vagrant@localhost ch14]$ bundle install</code><br>
  </li>
  <li>第１エラー発生　→　postgresqlが必要<br>
    <span style="color: red; ">
      An error occurred while installing pg (0.18.4), and Bundler cannot continue.<br>
      Make sure that `gem install pg -v '0.18.4' --source 'https://rubygems.org/'` succeeds before    bundling.<br>
    </span>
  【解決法】<br>
    <code>[vagrant@localhost ch14]$ cd</code><br>
    <code>[vagrant@localhost ~]$ sudo su</code><br>
    <code>[root@localhost vagrant]# yum install postgresql-devel</code><br>
    <code>[root@localhost ~]# exit</code><br>
  【参考サイト】`gem install pg` が失敗するときの対処法https://qiita.com/tdrk/items/812e7ea763080e147757
  </li>
  <li>動作確認続き<br>
    <code>[vagrant@localhost ~]$ cd sample_apps/5_1_2/ch14</code><br>
    <code>[vagrant@localhost ch14]$ bundle install</code><br>
    <code>[vagrant@localhost ch14]$ rails db:migrate</code><br>
    <code>[vagrant@localhost ch14]$ rails test</code><br>
    <code>[vagrant@localhost ch14]$ rails server</code><br>
  </li>
</ol>

次に、Railsのバージョンを5.1.7から5.1.6にダウングレードした。
<ol>
  <li>
    <code>[vagrant@localhost ch14]$ cd</code><br>
    <code>[vagrant@localhost ~]$ rails --version</code><br>
    <code>[vagrant@localhost ~]$ gem uninstall rails</code><br>
    <code>[vagrant@localhost ~]$ gem uninstall railties</code><br>
    <code>[vagrant@localhost ~]$ gem install rails -v 5.1.6</code><br>
    <code>[vagrant@localhost ~]$ rails -v</code><br>
    【参考サイト】How to downgrade my rails version?
    https://stackoverflow.com/questions/28082120/how-to-downgrade-my-rails-version
  </li>
</ol>

次に、GitHubのアカウント作成後、非公開型レポジトリhello_appを作成し、そこにプッシュを試みた。
<ol>
  <li>
    <code>[vagrant@localhost hello_app]$ git push -u origin --all</code><br>
  </li>
  <li>
    第２エラー発生　→ 公開鍵の設定が必要<br>
    Permission denied (publickey).<br>
    fatal: The remote end hung up unexpectedly<br>
  【解決法】<br>
    <code></code><br>
    <code></code><br>
    <code></code><br>
    <code></code><br>
    <code></code><br>
  【参考サイト】GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~https://qiita.com/shizuma/items/2b2f873a0034839e47ce
  <li>
</ol>

次に、Herokuのバージョンを確認した。
<ol>
  <li>
    <code>[vagrant@localhost hello_app]$ heroku --version</code><br>
  </li>
  <li>
    第３エラー発生　→　nodeのバージョンアップが必要<br>
    /usr/local/lib/heroku/node_modules/@oclif/command/lib/index.js:3<br>
    const path = require("path");<br>
    ^^^^^<br>
    SyntaxError: Use of const in strict mode.<br>
        at Module._compile (module.js:439:25)<br>
        at Object.Module._extensions..js (module.js:474:10)<br>
        at Module.load (module.js:356:32)<br>
        at Function.Module._load (module.js:312:12)<br>
        at Module.require (module.js:364:17)<br>
        at require (module.js:380:17)<br>
        at Object.<anonymous> (/usr/local/lib/heroku/bin/run:5:1)<br>
        at Module._compile (module.js:456:26)<br>
        at Object.Module._extensions..js (module.js:474:10)<br>
        at Module.load (module.js:356:32)<br>
  【解決法】<br>
    <code>[vagrant@localhost hello_app]$ node -v</code><br>
    <code>[vagrant@localhost hello_app]$ npm install -g n</code><br>
  【参考サイト】electron-packager コマンドでの Use of const in strict mode. エラーhttps://qiita.com/shy_azusa/items/b1ae6ab099e50c679774<br>
  </li>
  <li>
    第４エラー発生　→　https のレジストリに対するSSL 鍵のバリデーション機能を外す必要<br>
    npm http GET https://registry.npmjs.org/n<br>
    npm http GET https://registry.npmjs.org/n<br>
    npm http GET https://registry.npmjs.org/n<br>
    npm ERR! Error: CERT_UNTRUSTED<br>
    npm ERR!     at SecurePair.<anonymous> (tls.js:1430:32)<br>
    npm ERR!     at SecurePair.emit (events.js:92:17)<br>
    npm ERR!     at SecurePair.maybeInitFinished (tls.js:1029:10)<br>
    npm ERR!     at CleartextStream.read [as _read] (tls.js:521:13)<br>
    npm ERR!     at CleartextStream.Readable.read (_stream_readable.js:341:10)<br>
    npm ERR!     at EncryptedStream.write [as _write] (tls.js:418:25)<br>
    npm ERR!     at doWrite (_stream_writable.js:226:10)<br>
    npm ERR!     at writeOrBuffer (_stream_writable.js:216:5)<br>
    npm ERR!     at EncryptedStream.Writable.write (_stream_writable.js:183:11)<br>
    npm ERR!     at write (_stream_readable.js:602:24)<br>
    npm ERR! If you need help, you may report this log at:<br>
    npm ERR!     <http://github.com/isaacs/npm/issues><br>
    npm ERR! or email it to:<br>
    npm ERR!     <npm-@googlegroups.com><br>
<br>
    npm ERR! System Linux 2.6.32-754.29.1.el6.x86_64<br>
    npm ERR! command "node" "/usr/bin/npm" "install" "-g" "n"<br>
    npm ERR! cwd /home/vagrant/environment/hello_app<br>
    npm ERR! node -v v0.10.48<br>
    npm ERR! npm -v 1.3.6<br>
    npm ERR! <br>
    npm ERR! Additional logging details can be found in:<br>
    npm ERR!     /home/vagrant/environment/hello_app/npm-debug.log<br>
    npm ERR! not ok code 0<br>
  【解決法】<br>
    <code>[vagrant@localhost hello_app]$ cd</code><br>
    <code>[vagrant@localhost ~]$ sudo su</code><br>
    <code>[root@localhost vagrant]# sudo npm cache clean -f</code><br>
    <code>[root@localhost vagrant]# npm config set strict-ssl false</code><br>
    <code>[root@localhost vagrant]# sudo npm install -g n</code><br>
    <code>[root@localhost vagrant]# npm config set strict-ssl true</code><br>
    <code>[root@localhost vagrant]# sudo n stable</code><br>
    <code>[root@localhost vagrant]# sudo ln -sf /usr/local/n/versions/node/12.16.3/bin/node /usr/bin/node</code><br>
   【参考サイト】npm install で SSL Error になった時の対処法。https://blog.yug1224.com/archives/563d9b67bf652a600632d01e/<br>
  <li>
</ol>
