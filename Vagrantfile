Vagrant.configure("2") do |config|
  config.vm.box = 'punktde/proserver'
  config.vm.synced_folder '.', '/vagrant', id: 'vagrant-root', disabled: true
  config.vm.network 'private_network', ip: '172.17.28.28'
  config.ssh.forward_agent = true
  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
    vb.cpus = '1'
    vb.name = 'typo3'
  end
  config.vm.provision 'shell', keep_color: true, inline: <<-SHELL
    sudo -u proserver rm -f /var/www/typo3/index.html
    sudo -u proserver git clone https://github.com/punktDe/proserver-typo3-box.git /var/www/typo3
    cd /var/www/typo3
    sudo -u proserver composer install
    sudo -u proserver touch /var/www/typo3/web/FIRST_INSTALL
    sudo sed -i conf 's/welcome/typo3/' /usr/local/etc/nginx/vhosts/ssl.conf
    service nginx reload
    echo "################################################################################"
    echo "Go to https://172.17.28.28 and follow the TYPO3 installation process."
    echo "These are your database credentials:"
    echo "User: root"
    echo "Password: $(sudo cat /usr/local/etc/mysql-password)"
    echo "################################################################################"
  SHELL
end
