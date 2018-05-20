# SSHappend

> SSHappend hack session

## Setup

```
git clone https://github.com/mbigras/sshappend
cd sshappend
ssh-keygen -q -b 2048 -t rsa -N '' -f id_rsa
ssh-keygen -q -b 2048 -t rsa -N '' -f motd/id_rsa
```

## Example

```
vagrant up

for ip in 192.168.0.{2,3,4}; do ping -c 1 $ip | grep 'packet loss'; done
for ip in 192.168.0.{2,3,4}; do ssh-keyscan $ip >> ~/.ssh/known_hosts; done
for ip in 192.168.0.{2,3,4}; do ssh -i id_rsa vagrant@$ip echo hello world; done
for ip in 192.168.0.{2,3,4}; do ssh -i id_rsa vagrant@$ip grep ansible /etc/passwd; done
```

```
scp -i id_rsa -r motd vagrant@192.168.0.2:~
ssh -i id_rsa vagrant@192.168.0.2 sudo /bin/bash <<SHELL
	rm -rf /home/ansible/motd
	mv /home/vagrant/motd /home/ansible/
	chown -R ansible:ansible /home/ansible/motd
SHELL

ssh -i id_rsa vagrant@192.168.0.2

sudo su - ansible

mkdir -p .ssh
for host in $(cat /home/ansible/motd/hosts); do
	ssh-keyscan $host >> /home/ansible/.ssh/known_hosts
done

cd motd
ansible all -m ping
```

```
./sshappend vagrant id_rsa motd/hosts motd/id_rsa.pub ansible
```

```
ansible all -m ping
ssh -i id_rsa $(awk 'NR==1' hosts)
exit

ansible-playbook main.yml

ssh -i id_rsa $(awk 'NR==1' hosts)
exit
```