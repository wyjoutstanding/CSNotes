# nginx安装及使用

1. 因为nginx是编译好的文件，且位于`/etc/init.d`，所以`sudo /etc/init.d/nginx`可以输出相关操作方法

   ![1548747579026](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\1548747579026.png)

2.  使用示例

   + 启动：`sudo /etc/init.d/nginx start`；
   + 杀死进程：`sudo /etc/init.d/nginx stop`

   + 重启：`sudo /etc/init.d/nginx restart`

   + 测试配置：`sudo /etc/init.d/nginx configtest`
   + 查看网页：`curl localhost`

   + 查看进程：`ps -ef | grep nginx`，master对应的为进程号，可用``sudo kill 进程号`杀死此进程

     

3. 反向代理

   ```nginx
   server {
           listen       80;
           server_name  localhost;
   
           location /test/ {
                    proxy_pass http://www.baidu.com/;
           }
   }
   ```

   + 在浏览器输入网址：`localhost/test/index.html`，实际上搜索：`www.baidu.com/index.html`;

   + 如果`proxy_pass `最后没有`/`，那么输入如上网址，实际搜索：`www.baidu.com/test/index.html`

   + 在docker中启动nginx，localhost不等于本机localhost，需要用`ifconfig`找到网卡`ens33`对应的ip，在浏览器中输入即可

4. 一点常识

   + 虚拟机是一台独立的机器，与本地主机相互分隔。而每一台机子都拥有域名：`localhost`，对应ip为`127.0.0.1`，默认端口为80。所以在虚拟机和本地主机上使用`localhost/127.0.0.1`仅仅是访问自身对应的地址，像是有地理隔离的物种，无法沟通。
   + 本地主机比虚拟机多一个内网ip，可在cmd中输入`ipconfig`查看，IPv4对应的即是本机ip，如下图所示，我的主机ip为`192.168.101.8`，在虚拟机中可以通过此地址访问本地主机，只有ip是唯一的。如若是想在本地主机访问虚拟机中的nginx，需先查询对应ip才可在主机浏览器查询。![1549774071737](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\1549774071737.png)

5. `rewrite_by_lua`中不可以存在注释`#`，否则报错`500 Internal Server Error`