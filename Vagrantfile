
Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.network "forwarded_port", guest: 80, host: 8888,host_ip: "127.0.0.1"
    config.vm.network "forwarded_port", guest: 443, host: 8443, host_ip: "127.0.0.1"
    
    config.vm.provision "shell", run: "always",  inline: <<-SHELL
# temporarily shut down SELinux
        setenforce 0
# disable selinux
        sed -ie 's/^SELINUX=.*$/SELINUX=disabled/' /etc/selinux/config
        mkdir -p /var/www/website.com/html/
        mkdir -p /etc/httpd/conf.d/
# receive simple .html file from ssh
        cp -rav /vagrant/www-content/* /var/www/website.com/html/
# receive localhost configuration
        cp -rav /vagrant/conf/* /etc/httpd/conf.d/
# directory for private key
        mkdir /etc/ssl/privatekey
# restrict acces for root
        chmod 700 /etc/ssl/privatekey
        openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/apache-selfsigned.key -out /etc/pki/tls/certs/apache-selfsigned.crt -subj "/C=UA/ST=Lvivska/L=Lviv/O=ITStep/OU=University/CN=127.0.0.1"
# install extra packages
        yum install -y epel-release
# configure Apache2
        yum install -y httpd
# install module which supports TLC for apache
        yum install -y mod_ssl
# Hyper Text Transfer Protocol Daemon
        httpd -t
        systemctl start httpd
        systemctl enable httpd
                
    SHELL

end
