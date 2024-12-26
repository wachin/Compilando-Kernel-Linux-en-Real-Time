
Actualizando el sistema
Actualice los repositorios:

sudo apt update
 
Actualice los paquetes: 

sudo apt upgrade

Instalando las dependencias

Instalar:

sudo apt-get install gcc build-essential libncurses5-dev fakeroot wget xz-utils \
	flex bison libssl-dev autoconf automake cmake dwarves openssl libelf-dev \
	libudev-dev libpci-dev libiberty-dev bc python3-sphinx lzop lzma \
	lzma-dev libmpc-dev u-boot-tools gettext rsync libncurses-dev \
	libelf-dev libudev-dev pkg-config

```
wachin@mx23:~/Dev/kernel-5.15.173-a-RT
$ 
wget -c https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.15.173.tar.xz
wget -c https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.15/patch-5.15.173-rt82.patch.xz
tar xJvf linux-5.15.173.tar.xz
cd linux-5.15.173
```
entonces estaremos en la siguiente ubucación:

wachin@mx23:~/Dev/kernel-5.15.173-a-RT/linux-5.15.173
$

allí aplique el parche:
```
xzcat ../patch-5.15.173-rt82.patch.xz | patch -p1
```
y edité el archivo .bashrc  que para abrirlo puede ser así:

gedit ~/.bashrc 

o puede buscarlo como archivo oculto con "Ctrl+H"y abrirlo con algún editor de texto

y allí debe de poner lo siguiente:

DEBEMAIL="your.email.address@example.org"
DEBFULLNAME="Firstname Lastname"
export DEBEMAIL DEBFULLNAME

y cambiar con sus datos, guardar y cerrar. Cerrar sesión y volver a entrar (esto es necesario para que sus datos queden ingresados y se puedan usar)


y lo siguiente es lo que he activado:

```
# Enabled CCONFIG_NO_HZ_IDLE
 -> General setup
  -> Timers subsystem
   -> Timer tick handling (Full dynticks system (tickless))
    (X) Idle dynticks system (tickless idle)

# Enabled CONFIG_HIGH_RES_TIMERS
 -> General setup
  -> Timers subsystem
   [*] High Resolution Timer Support

# Enabled CONFIG_PREEMPT_RT
 -> General setup
  -> Preemption Model (Fully Preemptible Kernel (Real-Time))
   (X) Fully Preemptible Kernel (Real-Time)
 
(Nota:los nombres pueden variar por la versión del Kernel)
 
 # Enabled CONFIG_HZ_1000 
 -> Processor type and features
  -> Timer frequency (1000 HZ)
   (X) 1000 HZ

# Enabled CPU_FREQ_DEFAULT_GOV_PERFORMANCE
 ->  Power management and ACPI options
  -> CPU Frequency scaling
    -> Default CPUFreq governor
     (X) performance 
```

Ya se compiló y me un error, y le pregunto a ChatGPT:

"He sacado los dos paquetes principales compilados, menos: linux-libc-dev_5.15.173-1_i386.deb y los he colocado en una carpeta llamada "deb":

/home/wachin/Dev/kernel-5.15.173-a-RT/deb/linux-headers-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb
/home/wachin/Dev/kernel-5.15.173-a-RT/deb/linux-image-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb

y allí los traté de instalar pero sale error:
 
```
wachin@mx23:~/Dev/kernel-5.15.173-a-RT/deb
$ sudo dpkg -i *.deb
[sudo] contraseña para wachin:     
Seleccionando el paquete linux-headers-5.15.173-rt82-debian12-novm-pae previamente no seleccionado.
(Leyendo la base de datos ... 307783 ficheros o directorios instalados actualmente.)
Preparando para desempaquetar linux-headers-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb ...
Desempaquetando linux-headers-5.15.173-rt82-debian12-novm-pae (5.15.173-1) ...
Seleccionando el paquete linux-image-5.15.173-rt82-debian12-novm-pae previamente no seleccionado.
Preparando para desempaquetar linux-image-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb ...
Desempaquetando linux-image-5.15.173-rt82-debian12-novm-pae (5.15.173-1) ...
Configurando linux-headers-5.15.173-rt82-debian12-novm-pae (5.15.173-1) ...
Configurando linux-image-5.15.173-rt82-debian12-novm-pae (5.15.173-1) ...
dkms: running auto installation service for kernel 5.15.173-rt82-debian12-novm-pae.
/usr/sbin/dkms.mx autoinstall --kernelver 5.15.173-rt82-debian12-novm-pae
Sign command: /lib/modules/5.15.173-rt82-debian12-novm-pae/build/scripts/sign-file
Signing key: /var/lib/dkms/mok.key
Public certificate (MOK): /var/lib/dkms/mok.pub

Building module:
Cleaning build area...
./dkms-make.sh...(bad exit status: 2)
Error! Bad return status for module build on kernel: 5.15.173-rt82-debian12-novm-pae (i686)
Consult /var/lib/dkms/8812au/5.13.6/build/make.log for more information.
Sign command: /lib/modules/5.15.173-rt82-debian12-novm-pae/build/scripts/sign-file
Signing key: /var/lib/dkms/mok.key
Public certificate (MOK): /var/lib/dkms/mok.pub

Building module:
Cleaning build area...
make -j2 KERNELRELEASE=5.15.173-rt82-debian12-novm-pae KVER=5.15.173-rt82-debian12-novm-pae.....
Signing module /var/lib/dkms/broadcom-sta/6.30.223.271/build/wl.ko
Cleaning build area...

wl.ko:
Running module version sanity check.
 - Original module
   - No original module exists within this kernel
 - Installation
   - Installing to /lib/modules/5.15.173-rt82-debian12-novm-pae/updates/dkms/
depmod...
Sign command: /lib/modules/5.15.173-rt82-debian12-novm-pae/build/scripts/sign-file
Signing key: /var/lib/dkms/mok.key
Public certificate (MOK): /var/lib/dkms/mok.pub

Building module:
Cleaning build area...
'make' all KVER=5.15.173-rt82-debian12-novm-pae...(bad exit status: 2)
Error! Bad return status for module build on kernel: 5.15.173-rt82-debian12-novm-pae (i686)
Consult /var/lib/dkms/rtl8821cu/5.12.0/build/make.log for more information.
Error! One or more modules failed to install during autoinstall.
Refer to previous errors for more information.
dkms: autoinstall for kernel: 5.15.173-rt82-debian12-novm-pae failed!
run-parts: /etc/kernel/postinst.d/dkms exited with return code 11
update-initramfs: Generating /boot/initrd.img-5.15.173-rt82-debian12-novm-pae
I: The initramfs will attempt to resume from /dev/sda5
I: (UUID=840361ae-03de-484c-89f3-3c4c3648f730)
I: Set the RESUME variable to override this.
Generating grub configuration file ...
Found theme: /boot/grub/themes/mx_linux/theme.txt
Found linux image: /boot/vmlinuz-6.1.0-28-686-pae
Found initrd image: /boot/initrd.img-6.1.0-28-686-pae
Found linux image: /boot/vmlinuz-6.1.0-25-686-pae
Found initrd image: /boot/initrd.img-6.1.0-25-686-pae
Found linux image: /boot/vmlinuz-5.15.173-rt82-debian12-novm-pae
Found initrd image: /boot/initrd.img-5.15.173-rt82-debian12-novm-pae
Found Windows Recovery Environment on /dev/sda1
Found Windows 7 on /dev/sda2
Found Debian GNU/Linux 12 (bookworm) on /dev/sdb1
done
dpkg: error al procesar el paquete linux-image-5.15.173-rt82-debian12-novm-pae (--install):
 el subproceso instalado paquete linux-image-5.15.173-rt82-debian12-novm-pae script post-installation devolvió el código de salida de error 1
Se encontraron errores al procesar:
 linux-image-5.15.173-rt82-debian12-novm-pae
```
por cierto al kernel le desabilité la máquinavirtual:


-> Processor type and features -> [ ] Linux Guest Support ---

-> [ ] Virtualization --- 

Pero eso no se si habrá influido.

y antes de compilarlo cuando cerré menuconfig no había ningún mensaje de error:

```
$ make menuconfig
  HOSTCC  scripts/kconfig/mconf.o
  HOSTCC  scripts/kconfig/lxdialog/checklist.o
  HOSTCC  scripts/kconfig/lxdialog/inputbox.o
  HOSTCC  scripts/kconfig/lxdialog/menubox.o
  HOSTCC  scripts/kconfig/lxdialog/textbox.o
  HOSTCC  scripts/kconfig/lxdialog/util.o
  HOSTCC  scripts/kconfig/lxdialog/yesno.o
  HOSTCC  scripts/kconfig/confdata.o
  HOSTCC  scripts/kconfig/expr.o
  LEX     scripts/kconfig/lexer.lex.c
  YACC    scripts/kconfig/parser.tab.[ch]
  HOSTCC  scripts/kconfig/lexer.lex.o
  HOSTCC  scripts/kconfig/menu.o
  HOSTCC  scripts/kconfig/parser.tab.o
  HOSTCC  scripts/kconfig/preprocess.o
  HOSTCC  scripts/kconfig/symbol.o
  HOSTCC  scripts/kconfig/util.o
  HOSTLD  scripts/kconfig/mconf


*** End of the configuration.
*** Execute 'make' to start the build or try 'make help'.
```
 
 
