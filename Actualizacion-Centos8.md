
#Actualizacion Centos 7 a Centos 8

## PReparar Centos7

Descargar e instalar el repositorio EPEL:
~~~
sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
~~~

Descargar herramientas de yum:
~~~
sudo yum -y install rpmconf yum-utils
~~~

Verificar y mantener, por defecto, los paquetes rpm:
~~~
sudo rpmconf -a
~~~

Eliminar los paquetes que no se necesiten:
~~~
sudo package-cleanup --leaves
package-cleanup --orphans
~~~

Descargar el administrador de paquetes DNF, basado en RPM:
~~~
sudo yum -y install dnf
~~~

Borrar el administrador de paquetes yum:
~~~
sudo dnf -y remove yum yum-metadata-parser
~~~

Borrar el directorio de yum:
~~~
sudo rm -Rf /etc/yum
~~~

Actualizar el sistema con dnf:
~~~
sudo dnf -y upgrade
~~~

Instalación de la nueva versión:
~~~
sudo dnf -y upgrade http://mirror.bytemark.co.uk/centos/8/BaseOS/x86_64/os/Packages/centos-release-8.0-0.1905.0.9.el8.x86_64.rpm
~~~

Actualización de los repositorios:
~~~
sudo dnf -y upgrade https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
~~~

Limpiar ficheros temporales guardados en el repositorio:
~~~
sudo dnf clean all
~~~

Borrar el kernel:
~~~
sudo rpm -e `rpm -q kernel`
~~~

Borrar conflictos:
~~~
sudo rpm -e --nodeps sysvinit-tools
~~~

Lanzar la actualización:
~~~
sudo dnf -y --releasever=8 --allowerasing --setopt=deltarpm=false distro-sync
~~~

Verificar y mantener, por defecto, los paquetes rpm con la nueva configuración:
~~~
sudo rpmconf -a
~~~

Confirmar que el nuevo kernel es correcto:
~~~
sudo rpm -e kernel-core
sudo dnf -y install kernel-core
~~~

Confirmar que el grub está actualizado y en el lugar correcto:
~~~
ROOTDEV=`ls /dev/*da|head -1`;
sudo echo "Detected root as $ROOTDEV..."
sudo grub2-install $ROOTDEV
~~~

Instalación del paquete Minimal:
~~~
sudo dnf -y groupupdate "Core" "Minimal Install"
~~~

Verificar la versión de centos:
~~~
cat /etc/centos-release
~~~

Al finalizar se hace una actualización:
~~~
sudo dnf update
~~~

En la actualización puede surgir algunos problemas de dependencias de paquetes, para ello hay que eliminar algunos paquetes:
~~~
sudo dnf remove libzip
sudo rpm --nodeps -e gdbm
sudo dnf -y upgrade --best --allowerasing
~~~



