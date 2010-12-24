require 'rubygems'
require 'net/ssh'

REMOTE_ADDRESS = 'server.mhfs.com.br'
REMOTE_PATH    = '/home/mhfs.com.br'
REPO_ADDRESS   = 'git://github.com/mhfs/mhfs.github.com.git'
BRANCH         = 'master'
DEPLOY_USER    = 'mhfs_com_br'

desc "deploy to remote server"
task :deploy do
  commands = []
  Net::SSH.start( REMOTE_ADDRESS, DEPLOY_USER ) do |ssh|
    commands << "cd #{REMOTE_PATH}/git_repo"
    commands << "rm -rf _site"
    commands << "git checkout -f #{BRANCH}"
    commands << "git pull origin #{BRANCH}"
    commands << "jekyll #{REMOTE_PATH}/www"
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

