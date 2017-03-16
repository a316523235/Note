## nginx
  * apt-get nginx
  * nginx -t 检测并查看配置文件是否正确，并提示配置文件路径
  * vi newsite.conf 在配置文件同级目录下，创建新站点的配置文件
    文件内容
    server {
	  listen 80;
	  server_name longlij.com www.longlij.com booyu.com.cn www.booyu.com.cn;
	  location / {
	    proxy_pass http://127.0.0.1:3000;
	  }
	}
  * 在nginx.conf 找到 # Virtual Host Configs，加入新站点配置
    include /etc/nginx/light.conf;
  * nginx -t
  * nginx -c 启动
  * nginx -s stop 停止
