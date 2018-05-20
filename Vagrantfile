Vagrant.configure '2' do |config|
  config.vm.define 'ansible' do |h|
    h.vm.box = 'bento/ubuntu-16.04'
    h.vm.hostname = 'ansible'
    h.vm.network 'private_network', ip: '192.168.0.2'
    h.ssh.private_key_path = ['id_rsa', '~/.vagrant.d/insecure_private_key']
    h.ssh.insert_key = false
    h.vm.provision 'file', source: 'id_rsa.pub', destination: '~/.ssh/authorized_keys'
    h.vm.provision 'shell', inline: <<~EOF
      adduser --disabled-password --gecos '' ansible
      echo 'ansible ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/ansible

      apt-add-repository -y ppa:ansible/ansible
      apt-get update -y
      apt-get install -y ansible
    EOF
  end

  (1..2).each do |i|
    config.vm.define "web#{i}" do |h|
      h.vm.box = 'bento/ubuntu-16.04'
      h.vm.hostname = "web#{i}"
      h.vm.network 'private_network', ip: "192.168.0.#{2+i}"
      h.ssh.private_key_path = ['id_rsa', '~/.vagrant.d/insecure_private_key']
      h.ssh.insert_key = false
      h.vm.provision 'file', source: 'id_rsa.pub', destination: '~/.ssh/authorized_keys'
      h.vm.provision 'shell', inline: <<~EOF
        adduser --disabled-password --gecos '' ansible
        echo 'ansible ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/ansible
      EOF
    end
  end
end