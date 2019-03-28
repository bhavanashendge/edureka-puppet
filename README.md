# puppet

-- Steps to remove old puppet installs
dpkg -l | grep puppet
rm -rf puppetlabs
rm -rf /etc/puppetlabs/puppet/ssl


-- Steps to install puppetserver (Leader/Master node)
dpkg -i puppet6-release-bionic.deb
apt-get update
puppetserver ca setup
systemctl start puppetserver
Add entry in /ect/hosts file for the puppet node
<ip address of the master node> <name of the host> puppet

-- Steps to install puppet agent 
dpkg -i puppet6-release-bionic.deb
apt-get update
apt-get install puppet-agent
Add entry in /ect/hosts file for the puppet node
<ip address of the master node> <name of the host> puppet

--- Remove old certs from the agent
rm -rf /etc/puppetlabs/puppet/ssl/
-- Run from each agent to request for cert sign to puppet server
puppet agent -t

-- Once all or any of the agent requested for cert sign, run following on the puppet server
--- List all the certs requested/signed/revoked/
puppetserver ca list --all

puppetserver ca sign --certname master
puppetserver ca sign --certname slave

-- Steps to create module
cd /etc/puppetlabs/code/environments/production
mkdir -p modules/newmodule/manifests
vi modules/newmodule/manifests/init.pp

root@edureka-VirtualBox:/etc/puppetlabs/code/environments# mkdir -p production/modules/newmodule/manifests
root@edureka-VirtualBox:/etc/puppetlabs/code/environments# vi production/modules/newmodule/manifests/init.pp
        class newmodule {
         notify { 'This is new module': }
        }
root@edureka-VirtualBox:/etc/puppetlabs/code/environments# vi production/manifests/site.pp
node default {
        class { 'newmodule': }
}
