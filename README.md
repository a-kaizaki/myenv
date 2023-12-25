# myenv
Note to set up my development environment

# macOS
## Time Machine
### asimov
* 開発関係のファイルを除外するために、asimovを入れておく  
  https://github.com/stevegrunwell/asimov
* これをやっておかないと、pnpm使用時の node_modules 配下のバックアップにめちゃくちゃ時間がかかり、永遠にバックアップが終わらなくなる。
* `install.sh` により、launchctlで定期実行させておく
#### 手順
1. homebrewからインストールするとうまく行かないので、Manual installする (git clone)
2. sudoでないとパーミッションの問題が発生することがあるので、 `install.sh` を修正する  
参考: https://qiita.com/ritumutaka/items/e74d2e1785c38dc265da
```diff
--- a/install.sh
+++ b/install.sh
@@ -10,6 +10,7 @@ PLIST="com.stevegrunwell.asimov.plist"
 
 # Verify that Asimov is executable.
 chmod +x "${DIR}/asimov"
+chown root "${DIR}/${PLIST}"
 
 # Symlink Asimov into /usr/local/bin.
 echo -e "\\033[0;36mSymlinking ${DIR} to /usr/local/bin/asimov\\033[0m"
```
3. launchctlにロードするplistの修正
    * 6時間おきに実行するようにする
    * 実行できるか確認するため、ログ出力を追加
```diff
--- a/com.stevegrunwell.asimov.plist
+++ b/com.stevegrunwell.asimov.plist
@@ -7,7 +7,11 @@
         <key>Program</key>
         <string>/usr/local/bin/asimov</string>
         <key>StartInterval</key>
-        <!-- 24 hours = 60 * 60 * 24 -->
-        <integer>86400</integer>
+        <!-- 6 hour = 60 * 60 * 6 -->
+        <integer>21600</integer>
+        <key>StandardOutPath</key> <!-- 標準出力の出力先 -->
+        <string>/usr/local/var/log/asimov.log</string>
+        <key>StandardErrorPath</key> <!-- 標準エラーの出力先 -->
+        <string>/usr/local/var/log/asimov.log</string>
     </dict>
 </plist>

```
4. `% sudo ./install.sh`
5. `% sudo launchctl list | grep asimov` に出てくることを確認 (sudoでloadしたもののみが出るはず。)  
参考: https://srcw.net/wiki/?launchctl
6. asimovを6時間ごとに動かす関係上、Time Machine側のバックアップ間隔は1日にしておく。
#### その他
* Readmeにある `Retrieving excluded files` の方法では確認できなくなっている。
  * 確認したい場合は `tmutil isexcluded` コマンドで、対象パス(ディレクトリも可)が除外されているか確認できる。
