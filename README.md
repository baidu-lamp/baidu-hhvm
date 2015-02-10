## 背景
由于官方提供的HHVM编译过于复杂，国内也没有一整套的方便运维解决方案，那么我们提供一版baidu的免编译版本供国内对于HHVM的用户进行体验使用，并进行持续更新。

## 环境

此免依赖安装包，免依赖包和系统无依赖，目前内部使用支持centos6+和redhat4（外部aws可以使用），其他linux版本用户可以进行试用，内核需要linux 2.6.32以上

## 安装

Baidu hhvm 下载地址：


### 1.解压hhvm.tar.gz

        tar zxvf hhvm.tar.gz
  	cd hhvm
  
### 2.通过文本编辑器打开init.sh文件

	vi tools/exec/init.sh
  
修改如下的3个参数：

	HHVM_SOURCE_ROOT=php的源文件路径如（/home/www/app），需要绝对地址,这个是需要自定义的
	#hhvm服务的端口
	SERVER_PORT=8091
	#hhvm 管理端口
	ADMIN_PORT=8092
	
### 3.然后运行sh tools/exec/init.sh

主要是替换配置文件的路径、noau监控端口、还有设置ld-linux-x86-64.so.2的路径

注：

如果初始化完毕后，如果要是更换hhvm路径，那么需要重新运行init.sh，因为ld-linux-x86-64.so.2需要绝对路径

## 测试
### 运行脚本

编写测试脚本test.php：

	<?php
	   echo "hello world hhvm!\n";
	?>

存放在/home/work/data/www下（因为我们上面举例配置sourceroot为这个目录）

然后我们运行脚本:

  bin/hhvm_control start

fastcgi 模式启动：

需要通过nginx或者apache 转发到8091端口上，然后通过访问nginx端口访问hhvm的php，如nginx端口是8080,那么则访问http://127.0.0.1:8080/test.php 即可通过8080转发到8091访问我们的php页面；

也可以用http模式运行，修改hhvm.conf中fastcgi为Server.Type=libevent即可，这样就可以直接访问端口；

停止和重启运行脚本restart和stop即可；
hhvm 简单运行

如果我们不想运行脚本，仅仅测试hhvm,那么我们也可以利用上面写好的脚本，运行：

  bin/hhvm -c conf/hhvm.conf -m server

### 目录结构

<table>
<thead>
<tr>
  <th>目录名称	</th>
  <th>目录解释</th>
</tr>
</thead>
<tbody>
<tr>
  <td>bin</td>
  <td>存放hhvm可执行文件还有执行脚本</td>
</tr>
<tr>
  <td>cache</td>
  <td>存放hhbc文件的目录</td>
</tr>
<tr>
  <td>conf</td>
  <td>存放hhvm配置文件的目录</td>
</tr>
<tr>
  <td>ext</td>
  <td>存放扩展so的目录</td>
</tr>
<tr>
  <td>lib</td>
  <td>存放hhvm依赖的lib的目录</td>
</tr>
<tr>
  <td>lib64</td>
  <td>存放hhvm依赖库的lib64目录</td>
</tr>
<tr>
  <td>log</td>
  <td>log存放的位置，运行后会生成目录</td>
</tr>
<tr>
  <td>monitor</td>
  <td>监控脚本目录</td>
</tr>
<tr>
  <td>tools</td>
  <td>存放初始化文件和配置模板</td>
</tr>
<tr>
  <td>var</td>
  <td>存放hhvm.pid的目录</td>
</tr>
</tbody>
</table>
