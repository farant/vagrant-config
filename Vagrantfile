# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
	config.vm.synced_folder ".", "/vagrant", disabled: false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
	#config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "/home/ubuntu/.ssh/id_rsa"
	#config.vm.provision "file", source: "~/.bitbucket", destination: "/home/ubuntu/.bitbucket"
	#config.vm.provision "file", source: "~/.vimrc", destination: "/home/ubuntu/.vimrc"

  # vim +PlugInstall +qall
	
  config.vm.provision "shell", inline: <<SCRIPT
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get update
sudo apt-get install python python-pip build-essential silversearcher-ag libssl-dev libffi-dev python-dev nodejs ranger libsqlite3-dev -y
export HOME="/home/ubuntu"
curl -o- -L https://yarnpkg.com/install.sh | bash
export HOME="/root"
chown -R ubuntu /home/ubuntu/.yarn
chown -R ubuntu /home/ubuntu/.cache
printf '\nif [ -z "$SSH_AUTH_SOCK" ] ; then\n  eval `ssh-agent -s `\n  ssh-add\nfi\n\n' >> /home/ubuntu/.bashrc
printf '\nexport PATH="$HOME/.yarn/bin:$PATH"\n' >> /home/ubuntu/.bashrc
echo "cd /vagrant" >> /home/ubuntu/.bashrc
pip install --upgrade pip
pip install bitbucket-cli
sudo pip install -r ./requirements.txt
cd ../
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce -y
sudo groupadd docker
sudo gpasswd -a ubuntu docker
pip install docker-compose
cd /vagrant
echo "source /home/ubuntu/ansible/hacking/env-setup &>/dev/null" >> /root/.bashrc
sudo chown -R ubuntu /usr/lib/node_modules
npm install -g http-server
npm install -g ngrok
sudo add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./"
curl -L https://cli-assets.heroku.com/apt/release.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install heroku -y
SCRIPT

  config.vm.provision "shell", privileged: false, inline: <<SCRIPT
git config --global user.name "Fran Arant"
git config --global user.email francis.arant@gmail.com
echo "INSTALLING ANSIBLE"
git clone git://github.com/ansible/ansible.git --recursive
cd ./ansible
git reset --hard e4494f85b632545ff885e4d4bf6dcab5b9ceca8a
source ./hacking/env-setup
echo "source /home/ubuntu/ansible/hacking/env-setup &>/dev/null" >> /home/ubuntu/.bashrc
ansible --version
echo "INSTALLING RUBY"
cd ~
sudo apt-get update
sudo apt-get install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev
echo "DEBUGGING"
echo "PWD"
pwd
echo "WHOAMI"
whoami
echo "HOME"
echo $HOME
echo "CLONING RBENV"
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo "UPDATING PATH"
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo "UPDATING .bashrc"
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
echo "SOURCE .bashrc"
cat ~/.bashrc
source ~/.bashrc
echo "NEW PATH"
export PATH="$HOME/.rbenv/bin:$PATH"
export PATH="$HOME/.rbenv/shims:$PATH"
echo $PATH
echo "INIT RBENV"
rbenv init -
echo "CLONING RUBY BUILD"
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo "RBENV INSTALL 2.3.0"
rbenv install 2.3.0
echo "RBENV GLOBAL 2.3.0"
rbenv global 2.3.0
rbenv rehash
echo "GEM UPDATE"
gem update --system 2.6.11
echo "UPDATE .GEMRC"
echo "gem: --no-document" > ~/.gemrc
echo "GEM INSTALL BUNDLER"
gem install bundler
gem env home
gem install rails -v 5.1.0.rc1
rbenv rehash
SCRIPT
end
