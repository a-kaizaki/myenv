# myenv
Note to set up my development environment

# macOS
## Time Machine
* 開発関係のファイルを除外するために、asimovを入れておく  
  https://github.com/stevegrunwell/asimov
  * これをやっておかないと、pnpm使用時の node_modules 配下のバックアップにめちゃくちゃ時間がかかり、永遠にバックアップが終わらなくなる。
  * 注意点
    * homebrewからインストールするとうまく行かないので、Manual installする
    * Readmeにある `Retrieving excluded files` の方法では確認できなくなっている。
      * 確認したい場合は `tmutil isexcluded` コマンドで、対象パス(ディレクトリも可)が除外されているか確認できる。
    * `./install.sh` するとasimovが定期実行されるようになるが、デフォルトだと1日1回で少し心もとない気がするので、以下のようにしてから、インストールする。
    * asimovを6時間ごとに動かす関係上、Time Machine側のバックアップ間隔は1日にしておく。
```diff
--- a/com.stevegrunwell.asimov.plist
+++ b/com.stevegrunwell.asimov.plist
@@ -7,7 +7,7 @@
         <key>Program</key>
         <string>/usr/local/bin/asimov</string>
         <key>StartInterval</key>
-        <!-- 24 hours = 60 * 60 * 24 -->
-        <integer>86400</integer>
+        <!-- 6 hour = 60 * 60 * 6 -->
+        <integer>21600</integer>
     </dict>
 </plist>
```

インストールも1回じゃうまく行かない。(sudoしないと `ln` できない。逆にsudoすると `launchctl load` できないため。)

```bash
% sudo ./install.sh
(デーモン登録できなくてコケる)
% ./install.sh
replace /usr/local/bin/asimov? n
not replaced
```
