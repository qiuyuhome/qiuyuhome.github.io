

# 用的是 mac osx，编辑 phpstorm 的启动配置文件，其他平台根据情况选择:
sudo vim /Applications/PhpStorm.app/Contents/bin/phpstorm.vmoptions

# 修改参数，根据具体需要修改即可，一般修改  -Xmx(即最大可占内存)即可
-Xms512m
-Xmx2048m
-XX:MaxPermSize=350m
-XX:ReservedCodeCacheSize=240m
-XX:+UseCompressedOops

