# kodbox
* 可道云kodbox，现在版本1.13，发布于: 2020.10.10，此镜像支持https访问，http自动跳转https。
* 更新日志，https://doc.kodcloud.com/v2/#/help/changeLog
* 运行：docker run -d --name kodexplorer --restart always -p 80:80 -p 443:443 -v $your_dir:/var/www/html/data -v $your_ssl_dir:/ssl/ 33naisi/kodbox
* 持久性数据：/var/www/html/ /ssl/。
* 端口：80 443。
* 变量： SERVERNAME="localhost.localdomain" PEM="ssl.pem" KEY="ssl.key"
* 如果不想要自动跳转，可以在容器/etc/apache2/sites-available/000-default.conf文件中删除“RewriteEngine on   RewriteCond %{HTTPS} !=on   RewriteRule ^(.*) https://%{SERVER_NAME}$1 [L,R]”，也可以在Dockerfile中删除“ && sed -i "/<\/VirtualHost>/i\ \tRewriteEngine on\n\tRewriteCond %{HTTPS} !=on\n\tRewriteRule ^(.*) https://%{SERVER_NAME}$1 [L,R]" /etc/apache2/sites-available/000-default.conf \
   && a2enmod rewrite \”这两行，然后重新构建镜像。
* 如果要使用自己的证书，请使用"-v $yourDir/:/ssl/"指定ssl证书文件目录，使用"-e PEM=$yourPEMname -e KEY=$yourKEYname"指定你证书的名字，如果不适用-e参数，请把证书改名为默认ssl.pem和ssl.key。并在启动前请确认两个证书文件的目录和文件名,否则容器会因为找不到证书文件而启动时失败。使用自己的域名请使用"-e SERVERNAME=$yourDomain"。 如果ssl证书不可信或者证书和访问域名不一致，浏览器会提示证书异常。