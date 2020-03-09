# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
    GUEST_RUBY_VERSION = '2.5.7'
    config.vm.box = "centos/7"
    config.vm.box_check_update = false
    config.vm.network "forwarded_port", guest: 3000, host: 3000
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.synced_folder "./", "/home/vagrant/work"
    config.ssh.forward_agent = true

    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
    end

    config.vm.provision "shell", inline: <<-SHELL
        sudo yum -y install gcc-c++
        sudo yum -y install gettext-devel openssl-devel readline-devel
        echo '### installing nodejs ###'
        curl -sL https://rpm.nodesource.com/setup_12.x | sudo bash -
        sudo yum install -y nodejs
    SHELL

    config.vm.provision "shell", privileged: false, inline: <<-SHELL
        echo '### installing ImageMagick ###'
        sudo yum install -y openjpeg-devel libjpeg-turbo-devel libtiff-devel libgeotiff-devel libpng-devel giflib-devel libexif-devel libexif libwmf-devel libwmf libtool-ltdl-devel libtool-ltdl lcms-devel
        curl -OL https://www.imagemagick.org/download/releases/ImageMagick-7.0.8-68.tar.gz
        tar -vxf ImageMagick-7.0.8-68.tar.gz
        cd ImageMagick-7.0.8-68/
        ./configure
        make
        sudo make install

        sudo yum install -y sqlite sqlite-devel
        echo '### installing rbenv ###'
        git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
        git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
        echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
        echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
        source ~/.bash_profile
        rbenv install #{GUEST_RUBY_VERSION}
        rbenv global #{GUEST_RUBY_VERSION}
        rbenv rehash
        gem install bundler
        gem install rails -v 5.2.3
        gem install sassc
        echo 'You are now on Rails!'
        echo '        o o o o o o o . . .    ______________________________ _____=======_||____'
        echo '    o      _____            |                            | |                 |'
        echo '  .][__n_n_|DD[  ====_____  |                            | |                 |'
        echo ' >(________|__|_[_________]_|____________________________|_|_________________|'
        echo ' _/oo OOOOO oo`  ooo   ooo  `o!o!o                  o!o!o` `o!o         o!o`  '
        echo ' -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-'
    SHELL
end
User:vagrant user$

