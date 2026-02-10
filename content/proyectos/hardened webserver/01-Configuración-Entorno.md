Estoy usando WSL2 ([Windows Subsystem Linux](https://learn.microsoft.com/es-es/windows/wsl/))  como **control node**, es equivalente a la máquina de trabajo en un entorno real de Linux. Concretamente estoy utilizando la distribución *Ubuntu-24.04*.

Utilizo *Ubuntu-24.04* por mi experiencia de uso. Tiene soporte LTS largo y compatibilidad nativa con [CIS Benchmarks modernos](https://www.cisecurity.org/benchmark/ubuntu_linux).

Para la máquina a configurar, he utilizado Vagrant para levantarla de manera rápida.

```ruby
Vagrant.configure("2") do |config|
  # Usamos Ubuntu 22.04 LTS
  config.vm.box = "ubuntu/jammy64"

  # IP Privada para que WSL2 pueda comunicarse con la VM
  config.vm.network "private_network", ip: "192.168.56.10"

  config.vm.provider "virtualbox" do |vb|

    vb.memory = "1024"
    vb.cpus = 1
    vb.name = "webserver-portfolio-lab"

  end

  # Deshabilitamos la configuración por defecto de SSH insegura

  config.ssh.insert_key = true

end
```
