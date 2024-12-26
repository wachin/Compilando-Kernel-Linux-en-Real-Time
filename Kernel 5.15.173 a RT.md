Estos son los pasos que he seguido para compilar el Kernel 5.15.173 con el parche Real Time

Requisitos
- Compilar el Kernel en una máquina virtual

Actualizando el sistema
Actualice los repositorios:
```
sudo apt update
 ```
Actualice los paquetes: 
```
sudo apt upgrade
```
Instalando las dependencias

Instalar:
```
sudo apt-get install gcc build-essential libncurses5-dev fakeroot wget xz-utils \
	flex bison libssl-dev autoconf automake cmake dwarves openssl libelf-dev \
	libudev-dev libpci-dev libiberty-dev bc python3-sphinx lzop lzma \
	lzma-dev libmpc-dev u-boot-tools gettext rsync libncurses-dev \
	libelf-dev libudev-dev pkg-config
```

Descargando el código fuente y parche del Kernel:

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

Además al kernel le desabilité la máquinavirtual:

-> Processor type and features -> [ ] Linux Guest Support ---

-> [ ] Virtualization --- 


Ya se compiló y me un error, y le pregunto a ChatGPT:

"He sacado los dos paquetes principales compilados, menos: linux-libc-dev_5.15.173-1_i386.deb y los he colocado en una carpeta llamada "deb":

/home/wachin/Dev/kernel-5.15.173-a-RT/deb/linux-headers-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb
/home/wachin/Dev/kernel-5.15.173-a-RT/deb/linux-image-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb

