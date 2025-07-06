# Guía para instalar KDE Plasma en Linux Mint Cinnamon sin eliminar Cinnamon
[Versión en inglés](https://github.com/MrHyde7191/kde-plasma-on-mint-en "English version")

![Linux Mint Cinnamon   KDE Plasma](https://github.com/user-attachments/assets/12f3108c-bd79-47b7-b4f1-c7f550d65cc1)
![Prueba de la Guia](https://github.com/user-attachments/assets/99d96778-1d7b-483d-9f07-20d92f95e93f)
![dpkg-reconfigure sddm](https://github.com/user-attachments/assets/a6455a3b-7337-4212-b96e-2e5d832020b3)

## 📌 Introducción

```
Muchas personas creen que no es posible tener KDE Plasma funcionando en Linux Mint Cinnamon, o que
cambiar de entorno implica eliminar Cinnamon por completo. Esto no es cierto. Con algunos ajustes
cuidadosos, puedes instalar y usar KDE Plasma como entorno principal, manteniendo Cinnamon instalado
y desactivado para evitar conflictos.
```
## 📢 Sobre esta guía

```
He leído varias guías que explican cómo instalar KDE Plasma en Linux Mint Cinnamon, pero ninguna
detallaba cómo mantener Cinnamon instalado y desactivado sin eliminarlo. Por eso decidí documentar
este método reversible y limpio que permite usar KDE sin interferencias, conservando Cinnamon
disponible por si se quiere volver. Me ha parecido apropiado aportar mi granito de arena,
y espero que esta guía, que he procurado que sea clara y sin errores, le sirva a más personas.
```
## 📚 Qué aprenderás

- Instalar KDE Plasma en Mint Cinnamon.  
- Cambiar el gestor de sesiones de LightDM a SDDM.  
- Desactivar los servicios y autoarranques de Cinnamon para evitar interferencias.  
- Mantener Cinnamon instalado y totalmente reversible si es necesario.

 ⚠️ Nota importante:
 
 Antes de realizar ningún cambio en entornos de escritorio, es recomendable crear una instantánea con Timeshift o
 realizar una copia de seguridad de tus datos y configuraciones. Así podrás restaurar tu sistema 
 en caso de problemas.
  
## 🖥️ Paso a paso

### 1️⃣ Instalar KDE Plasma

```bash
sudo apt install kde-plasma-desktop       # Minimal
sudo apt install kde-standard             # Recommended
sudo apt install kde-full                 # Full suite (bloatware)

Instala el entorno KDE Plasma mínimo, estándar o completo.
```
### 2️⃣ Configurar SDDM como gestor de sesiones
```
sudo dpkg-reconfigure sddm

Selecciona sddm como gestor de inicio de sesión.
```
### 3️⃣ Habilitar y arrancar SDDM
```bash
sudo systemctl enable sddm
sudo systemctl start sddm
```
### 4️⃣ Desactivar autostarts de Cinnamon (user)
```bash
mkdir -p ~/.config/autostart/disabled
mv ~/.config/autostart/cinnamon-settings-daemon-*.desktop ~/.config/autostart/disabled/

Mueve los autostarts de Cinnamon a una carpeta desactivada.
```
### 5️⃣ Revisar autostarts globales
```bash
ls /etc/xdg/autostart/ | grep cinnamon

Muestra los servicios de Cinnamon configurados para arrancar globalmente.
```
### 6️⃣ Verificar servicios Plasma
```bash
systemctl --user list-units | grep plasma
```
### 7️⃣ Verificar procesos de Cinnamon
```bash
ps aux | grep -i cinnamon
```
### 8️⃣ Revisar autostarts del usuario
```bash
ls -l ~/.config/autostart/
```
### 9️⃣ (Opcional) Desactivar MintUpdate en Plasma
```bash
mv ~/.config/autostart/mintupdate.desktop ~/.config/autostart/disabled/
```
### 🔟 Revisar logs de sesión
```bash
journalctl --user -b | tail -30
```
## 📌 Resultado final
```
- El sistema sigue siendo Linux Mint Cinnamon (paquetes y actualizaciones vía APT y Discover).
- KDE Plasma funciona como entorno de escritorio principal.
- SDDM gestiona los inicios de sesión.
- Todos los autostarts y servicios de Cinnamon desactivados en Plasma.
- Las herramientas de Cinnamon siguen instaladas y actualizables.
- MintUpdate disponible bajo demanda o si lo llama otra aplicación.
```
## 🔄 Cómo volver a Cinnamon

```bash
Si quieres restaurar Cinnamon como tu entorno de escritorio por defecto, puedes revertir los cambios fácilmente:

sudo dpkg-reconfigure lightdm   # Selecciona LightDM como gestor de pantalla
sudo systemctl enable lightdm
sudo systemctl disable sddm

mv ~/.config/autostart/disabled/* ~/.config/autostart/  # Mover los autoarranques de Cinnamon a la carpeta original.

```
## 📎 Notas

```bash
Si alguna aplicación abre una ventana de configuración de Mint o Cinnamon, es 
normal porque sus binarios siguen presentes. No indica conflicto.

Además, aunque esta guía busca desactivar de forma limpia los componentes 
de Cinnamon al usar KDE, en algunos casos muy concretos (por ejemplo, ciertos 
Flatpak, actualizaciones de Discover o autostarts de terceros) podría ejecutarse 
alguna herramienta o servicio de Cinnamon. Esto no provoca inestabilidad, pero 
se recomienda revisar los logs ocasionalmente.

