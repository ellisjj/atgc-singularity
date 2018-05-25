Bootstrap: docker
From: centos:7.4.1708

%labels
MAINTAINER  Jonathan Ellis <jonathan.j.ellis@gmail.com>


%environment
source /etc/profile
module use /app/modules/all
export EASYBUILD_PREFIX=/app
export EASYBUILD_CONFIGFILES=/app/eb_local/config.cfg
	
%post
yum update -y
yum groupinstall -y "Development Tools"
yum install -y epel-release
yum install -y which curl git python-pip python-devel Lmod perl-core libibverbs-devel openssl-devel

# need this for --package to work to generate RPM
# yum install ruby ruby-devel rubygems -y &&
# gem install fpm &&

pip install --upgrade pip
pip install setuptools
# pip install easybuild

# need this for git integration with eb
#pip install GitPython python-graph-dot graphviz keyring keyrings.alt

mkdir -p /app/modules
mkdir -p /app/software
mkdir -p /app/sources
mkdir -p /scratch/tmp
useradd easybuild
chown easybuild:easybuild -R /app
chown easybuild:easybuild -R /scratch
su - easybuild

mkdir -p ~/.ssh
ssh-keyscan -H bitbucket.org >> ~/.ssh/known_hosts
ssh-keyscan -H github.com >> ~/.ssh/known_hosts

export EASYBUILD_PREFIX=/app
export EASYBUILD_CONFIGFILES=/app/eb_local/config.cfg
cd $EASYBUILD_PREFIX
git clone https://bitbucket.org/ellisjj/easybuild-local.git eb_local
curl -O https://raw.githubusercontent.com/easybuilders/easybuild-framework/develop/easybuild/scripts/bootstrap_eb.py
python bootstrap_eb.py $EASYBUILD_PREFIX
