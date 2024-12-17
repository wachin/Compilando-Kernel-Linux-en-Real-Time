
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
 
 


# Respuesta de ChatGPT:

El error principal que estás viendo durante la instalación del kernel es debido a que **`dkms`** (Dynamic Kernel Module Support) está intentando reconstruir módulos externos, y algunos de ellos están fallando porque **no son compatibles** con tu nuevo kernel RT o la configuración actual.

---

### **1. Identificación del problema**
Las líneas clave son estas:

```
Error! Bad return status for module build on kernel: 5.15.173-rt82-debian12-novm-pae (i686)
Consult /var/lib/dkms/8812au/5.13.6/build/make.log for more information.
...
Error! Bad return status for module build on kernel: 5.15.173-rt82-debian12-novm-pae (i686)
Consult /var/lib/dkms/rtl8821cu/5.12.0/build/make.log for more information.
Error! One or more modules failed to install during autoinstall.
```

- **`8812au` y `rtl8821cu`** son módulos de controladores de red inalámbrica gestionados por `dkms`.
- Al instalar el nuevo kernel, `dkms` intenta recompilar estos módulos para el kernel `5.15.173-rt82-debian12-novm-pae`.
- La compilación falla, lo cual causa que el script `postinst` del paquete `linux-image` devuelva un error.

---

### **2. ¿Por qué ocurre?**
1. **Incompatibilidad del código fuente** de los módulos `8812au` y `rtl8821cu` con el nuevo kernel RT (probablemente cambios en la API del kernel).
2. Deshabilitar "Linux Guest Support" y "Virtualization" no influye directamente en este error, así que puedes descartarlo.

---

### **3. Solución paso a paso**

**¿Cómo se instalaron los módulos 8812au y rtl8821cu en tu sistema?**

En MX Linux (o Debian), los módulos como **`8812au`** y **`rtl8821cu`** pueden haber sido instalados de varias formas:

1. **Instalación automática durante la instalación del sistema operativo**:
   - Algunos controladores adicionales vienen preinstalados en MX Linux si se detecta hardware compatible.
   - MX Linux incluye muchos módulos de `dkms` y controladores de red en su instalación por defecto.

2. **Instalación mediante `apt update` y `apt upgrade`**:
   - Si tu sistema detectó hardware inalámbrico (como una tarjeta Wi-Fi USB que necesita `8812au` o `rtl8821cu`), `apt` puede haber instalado automáticamente los paquetes correspondientes.
   - Los paquetes `dkms` se actualizan automáticamente al ejecutar `sudo apt upgrade`.

3. **Instalación manual (posible)**:
   - Si alguna vez instalaste controladores Wi-Fi desde GitHub, AUR (en sistemas Arch), o mediante compilación manual, estos módulos podrían haber quedado registrados en `dkms`.

---

### **¿Cómo verificar qué instaló los módulos `8812au` y `rtl8821cu`?**

1. Usa **`dkms status`** para listar todos los módulos `dkms` instalados:
   ```bash
   dkms status
   ```
   Esto mostrará las versiones de los módulos y para qué kernels están instalados.

2. Verifica si los módulos están instalados a través de APT:
   ```bash
   apt list --installed | grep -E '8812au|rtl8821cu'
   ```
   Esto mostrará si estos módulos fueron instalados como paquetes `.deb`.

3. Si no aparecen en APT, pero sí en `dkms`, entonces probablemente se instalaron manualmente desde una fuente externa.

---

### **Recomendaciones finales**

1. **Si los módulos no son críticos** (no usas dispositivos Wi-Fi que requieran `8812au` o `rtl8821cu`):
   - Elimínalos temporalmente de `dkms`:
     ```bash
     sudo dkms remove 8812au/5.13.6 --all
     sudo dkms remove rtl8821cu/5.12.0 --all
     ```
   - Luego reinstala el kernel:
     ```bash
     sudo dpkg -i linux-image-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb \
                 linux-headers-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb
     ```

2. **Si necesitas los módulos (usos de red Wi-Fi)**:
   - Descarga una versión actualizada y compatible de los controladores desde repositorios en **GitHub**:
     - **rtl8812au**:  
       ```bash
       git clone https://github.com/aircrack-ng/rtl8812au.git
       cd rtl8812au
       sudo dkms add .
       sudo dkms build 8812au/5.13.6
       sudo dkms install 8812au/5.13.6
       ```
     - **rtl8821cu**:  
       Busca repositorios en GitHub compatibles con el kernel 5.15 RT y sigue un proceso similar.

3. **Si prefieres el kernel 6.1**:
   - Descarga el código fuente del kernel **6.1** desde Debian o MX Linux.
   - Aplica el parche RT correspondiente desde [kernel.org](https://www.kernel.org/pub/linux/kernel/projects/rt/6.1/).
   - Compila el kernel con tus configuraciones optimizadas.



> me devuelve lo siguiente: "wachin@mx23:~/Dev/kernel-5.15.173-a-RT/linux-5.15.173
$ dkms status
/usr/sbin/dkms.mx status
8812au/5.13.6, 6.1.0-25-686-pae, i686: installed
8812au/5.13.6, 6.1.0-28-686-pae, i686: installed
broadcom-sta/6.30.223.271, 5.15.173-rt82-debian12-novm-pae, i686: installed
broadcom-sta/6.30.223.271, 6.1.0-25-686-pae, i686: installed
broadcom-sta/6.30.223.271, 6.1.0-28-686-pae, i686: installed
rtl8821cu/5.12.0, 6.1.0-25-686-pae, i686: installed
rtl8821cu/5.12.0, 6.1.0-28-686-pae, i686: installed" pero este ordenador no usa Wi-Fi, solo está conectado por cable así que los voy a desinstalar así como me dices, pero pregunto, si los desinstalo y luego instalo otra vez el kernel esos modulos se volverán a instalar o ya no


Si desinstalas los módulos **`8812au`** y **`rtl8821cu`** de `dkms`, **no se reinstalarán automáticamente** cuando instales otro kernel, a menos que:

1. **Los módulos estén instalados como paquetes APT** (en cuyo caso, APT intentará reconstruirlos durante la instalación del kernel).
2. **Tengas un script o configuración adicional** que fuerce su reconstrucción con `dkms`.

---

### **Verificación antes de desinstalar**
Primero asegúrate de que los módulos no vienen de un paquete APT. Ejecuta:
```bash
apt list --installed | grep -E '8812au|rtl8821cu|broadcom-sta'
```

- Si **aparecen en la lista**, significa que se instalaron a través de APT. En este caso, tendrás que eliminarlos con `apt`:
   ```bash
   sudo apt remove --purge rtl8821cu-dkms 8812au-dkms broadcom-sta-dkms
   ```

- Si **no aparecen**, entonces los módulos se añadieron manualmente a `dkms` y puedes eliminarlos con:
   ```bash
   sudo dkms remove 8812au/5.13.6 --all
   sudo dkms remove rtl8821cu/5.12.0 --all
   sudo dkms remove broadcom-sta/6.30.223.271 --all
   ```

---

### **Instalar otro kernel después de desinstalar**
Una vez que hayas eliminado los módulos de `dkms`, estos **no se volverán a instalar automáticamente** al instalar otro kernel porque:

1. `dkms` no tendrá registros de esos módulos.
2. Si los módulos venían de paquetes APT y los desinstalaste, no podrán reconstruirse.

---

### **Opcional: Evitar futuras instalaciones automáticas de módulos no deseados**
Si quieres asegurarte de que no se vuelvan a instalar estos módulos por error en el futuro:

1. **Bloquea los paquetes APT** (si existían):
   ```bash
   sudo apt-mark hold rtl8821cu-dkms 8812au-dkms broadcom-sta-dkms
   ```

2. **Limpia registros de `dkms`**:
   Asegúrate de que no queden rastros en el sistema:
   ```bash
   sudo rm -rf /var/lib/dkms/8812au
   sudo rm -rf /var/lib/dkms/rtl8821cu
   sudo rm -rf /var/lib/dkms/broadcom-sta
   ```





> aquí está lo que aparece: "wachin@mx23:~/Dev/kernel-5.15.173-a-RT/linux-5.15.173
$ apt list --installed | grep -E '8812au|rtl8821cu|broadcom-sta'

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

broadcom-sta-dkms/stable,now 6.30.223.271-23 all [instalado]
rtl8812au-dkms/mx,now 5.13.6+git20230720-1~mx23+1 all [instalado]
rtl8821cu-dkms/mx,now 5.12.0+git20230215-1~mx23+3 all [instalado]"



Perfecto, ahora queda claro que los módulos **broadcom-sta-dkms**, **rtl8812au-dkms** y **rtl8821cu-dkms** **fueron instalados a través de APT**. Esto significa que durante la instalación de cualquier kernel, **`dkms`** intenta automáticamente reconstruir esos módulos.

---

### **¿Cómo solucionarlo?**

Dado que no usas Wi-Fi y estos módulos no son necesarios, puedes desinstalarlos con APT. Esto evitará que `dkms` los intente compilar cada vez que instales un nuevo kernel.

1. **Desinstala los módulos innecesarios**:
   ```bash
   sudo apt remove --purge broadcom-sta-dkms rtl8812au-dkms rtl8821cu-dkms
   ```

2. **Limpia los registros de `dkms`**:
   Después de desinstalarlos, verifica que no queden rastros en `dkms`:
   ```bash
   sudo dkms status
   ```

   Si todavía aparecen, elimínalos manualmente:
   ```bash
   sudo dkms remove 8812au/5.13.6 --all
   sudo dkms remove rtl8821cu/5.12.0 --all
   sudo dkms remove broadcom-sta/6.30.223.271 --all
   ```

3. **Instala el kernel compilado**:
   Ahora puedes instalar tu kernel RT sin que `dkms` intente reconstruir módulos innecesarios:
   ```bash
   sudo dpkg -i linux-image-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb \
               linux-headers-5.15.173-rt82-debian12-novm-pae_5.15.173-1_i386.deb
   ```

4. **Verificación final**:
   - Asegúrate de que el kernel se haya instalado correctamente:
     ```bash
     dpkg -l | grep linux-image
     ```
   - Reinicia y selecciona el kernel RT en el menú GRUB.

---

### **¿Qué pasa si instalo otro kernel más adelante?**
Como desinstalaste los paquetes `broadcom-sta-dkms`, `rtl8812au-dkms` y `rtl8821cu-dkms`, **`dkms` ya no intentará reconstruirlos** automáticamente cuando instales otro kernel.

Si en el futuro necesitas estos controladores (por ejemplo, para usar una tarjeta Wi-Fi), puedes reinstalarlos con:
```bash
sudo apt install broadcom-sta-dkms rtl8812au-dkms rtl8821cu-dkms
```

---

Si necesitas más ayuda o quieres verificar paso a paso, avísame. 😊
