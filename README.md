<h1>仮想環境（Vagrant)を使った環境構築まとめ</h1>

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
  <li>ruby --version や rails --version でちゃんと動くか確かめる</li>
  ruby 2.6.2p47 (2019-03-13 revision 67232) [x86_64-linux]<br>
  Rails 5.1.7
  <li>動作確認</li>
  <code>git clone https://github.com/yasslab/sample_apps.git</code><br>
  <code>cd sample_apps/5_1_2/ch14</code><br>
  <code>bundle install</code><br>
  <span style="color: red; ">
    An error occurred while installing pg (0.18.4), and Bundler cannot continue.<br>
    Make sure that `gem install pg -v '0.18.4' --source 'https://rubygems.org/'` succeeds before bundling.<br>
  </span>

</ol>

