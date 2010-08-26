require 'rubygems'
require 'net/ssh'

REMOTE_ADDRESS = 'server.mhfs.com.br'
REMOTE_PATH    = '/home/www/mhfs.com.br'
REPO_ADDRESS   = 'git://github.com/mhfs/mhfs.github.com.git'
BRANCH         = 'master'
DEPLOY_USER    = 'mhfs'

desc "deploy to remote server"
task :deploy do
  commands = []
  Net::SSH.start( REMOTE_ADDRESS, DEPLOY_USER ) do |ssh|
    commands << "cd #{REMOTE_PATH}/git_repo"
    commands << "rm -rf _site"
    commands << "git checkout -f #{BRANCH}"
    commands << "git pull origin #{BRANCH}"
    commands << "source /usr/local/lib/rvm" # bashrc is not sourced via Net::SSH
    commands << "jekyll"
    ssh.exec commands.join('; ')
  end
end

task :clone do
  commands = []
  Net::SSH.start( REMOTE_ADDRESS, DEPLOY_USER ) do |ssh|
    commands << "cd #{REMOTE_PATH}"
    commands << "git clone #{REPO_ADDRESS} git_repo"
    commands << "cd git_repo"
    commands << "git checkout #{BRANCH}" unless BRANCH == 'master'
    ssh.exec commands.join('; ')
  end
end

task :setup do
  commands = []
  Net::SSH.start( REMOTE_ADDRESS, DEPLOY_USER ) do |ssh|
    commands << "sudo gem install --no-rdoc --no-ri jekyll RedCloth"
    commands << "sudo easy_install Pygments"
    ssh.exec commands.join('; ')
  end
end
