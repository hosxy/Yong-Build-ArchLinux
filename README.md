# 关于小小(yong)输入法的在 ArchLinux 下的编译(只编译 64 位)
先贴上官方的编译方法：http://yong.dgod.net/read.php?tid=2247&fid=5

如果只是想要 QT 的输入法插件的话，我已经编译好了，可以直接下载使用：<br>
[qt 输入法插件](https://github.com/hosxy/yong-Build-ArchLinux/releases)

在 Arch Linux 下编译可能需要注意以下几点：
1. 编译QT输入法插件先在 im/qt5-im/ 下创建一个 l64-qt5 目录，用于存放编译好的插件。
2. 编译QT输入法插件的时候，编译脚本有个地方的路径写死了，Arch 的路径不在那里，需要修改：
具体位置是 im/qt5-im目录下的 build.txt 的大概位于第 22 行左右的变量 `MOC` 将其修改为： `var MOC=['/usr/bin/moc'];`

3. 由于只编译 64 位(主要是 Arch Linux不再支持 32 位程序了)，但是最后复制的时候会同时复制 32 位和 64 位，由于没有编译32位，所以就会报错，所以简单修改下(用的时候把 copy 换成 copy64 就行了)。具体位置是在 install 目录下的 build.txt 的最后一段， 
修改前：
```
if(target=="copy"){
	copy_data();
	copy_build("l32");
	copy_build("l64");
} else if(target=="dist"){
	set_ver();
	exec("7za a yong-lin-$(VER).7z yong");	
} else if(target=="rpm"){
	exec('rpmbuild -bb yong.spec --target=i686 --define "_rpmdir `pwd`"');
}

}
```
修改后就是这样：
```
if(target=="copy"){
	copy_data();
	copy_build("l32");
	copy_build("l64");
} else if(target=="copy64"){
	copy_data();
	copy_build("l64");
}else if(target=="dist"){
	set_ver();
	exec("7za a yong-lin-$(VER).7z yong");	
} else if(target=="rpm"){
	exec('rpmbuild -bb yong.spec --target=i686 --define "_rpmdir `pwd`"');
}
```
修改这两处后基本就可以正常编译通过了。
