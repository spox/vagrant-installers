# -*- mode: ruby -*-
# vi: set ft=ruby :

build_boxes = [
  'centos-5.11',
  'centos-5.11-i386',
  'osx-10.9',
  'ubuntu-10.04',
  'ubuntu-10.04-i386',
  'windows-7'
]
box_prefix = 'spox'
script_base = 'https://raw.githubusercontent.com/spox/substrate-bootstrap/master/'

Vagrant.configure("2") do |config|
  build_boxes.each do |box_basename|
    config.vm.define(box_basename) do |box_config|
      script_name = box_basename.split('-').first
      script_ext = script_name.include?('windows') ? 'ps1' : 'sh'
      provision_script = [script_base, "#{script_name}.#{script_ext}"].join('/')
      provision_command = "curl -L '#{provision_script}' | sh"

      box_config.vm.box = "#{box_prefix}/#{box_basename}"
      box_config.vm.provision "shell", :inline => provision_command

      ["vmware_fusion", "vmware_workstation"].each do |p|
        config.vm.provider "p" do |v|
          v.vmx["memsize"] = "2048"
          v.vmx["numvcpus"] = "2"
          v.vmx["cpuid.coresPerSocket"] = "1"
        end
      end
    end
  end
end
