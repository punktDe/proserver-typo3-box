Vagrant.configure("2") do |config|
  config.vm.box = 'punktde/proserver-developer-box'
  config.vm.box_url = "http://packages.pluspunkthosting.de/proserver-boxes/proserver-box.json"
  config.vm.synced_folder '.', '/vagrant', id: 'vagrant-root', disabled: true
  config.vm.network 'private_network', ip: '172.17.28.28'
  config.ssh.forward_agent = true
  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
    vb.cpus = '1'
    vb.name = 'typo3'
  end
  config.vm.provision 'shell', inline: <<-SHELL
    sudo -u proserver rm -f /var/www/typo3/index.html
    sudo -u proserver git clone https://ry28@gitlab.pluspunkthosting.de/opensource/proserver-typo3-box.git /var/www/typo3
    cd /var/www/typo3
    sudo -u proserver composer install
    sudo -u proserver touch /var/www/typo3/FIRST_INSTALL
    echo "################################################################################"
    echo "Go to https://172.17.28.28 and follow the TYPO3 installation process."
    echo "These are your database credentials:"
    echo "User: root"
    echo "Password: $(sudo cat /usr/local/etc/mysql-password)"
    echo "################################################################################"
  SHELL
end
