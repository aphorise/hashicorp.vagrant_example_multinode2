Vagrant.configure("2") do |config|
  # // NUMBER OF HTTP INSTANCES
  iN = 2
  # // ^^ UP TO 9 <= iN > 0
  config.vm.provider "virtualbox"

  # // HTTP NGiNX Servers
  (1..iN).each do |iX|
    config.vm.define vm_name="nginx#{iX}-mysql" do |node_http|
      node_http.vm.box_version = "0.0.1"
      node_http.vm.box = "aphorise/ubuntu16-nginx"
      node_http.vm.hostname = vm_name
      node_http.vm.network "forwarded_port", guest: 80, host: "5818#{iX}", id: "nginx-mysql#{iX}"
   end
  end

  # // MySQL DB Servers
  config.vm.define vm_name="mysql-nginx" do |node_sql|
    node_sql.vm.box = "brownell/xenial64lemp"
    node_sql.vm.hostname = vm_name
    node_sql.vm.network "forwarded_port", guest: 3306, host: 53306, id: "mysql"
  end

end
