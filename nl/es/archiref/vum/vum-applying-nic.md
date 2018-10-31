---

copyright:

  years:  2016, 2018

lastupdated: "2018-10-05"

---

# Aplicación de controladores NIC nativos

ESXi 6.5 contiene muchos controladores nativos nuevos que sustituyen los controladores vmklinux anteriores. Mientras que la mayoría de los nuevos controladores nativos están habilitados de forma predeterminada después de la instalación o la actualización, tres de los nuevos controladores nativos están inhabilitados de forma predeterminada, ya que no dan soporte completo a las funciones de los controladores vmklinux correspondientes.

ixgben es un controlador nativo que sustituye al controlador vmklinux net-ixgbe, pero no admite SR-IOV y SW FcOE. La automatización de ICVS no debería haber habilitado este controlador cuando se suministró el host ESXi de vSphere. Se recomienda habilitar este controlador por los beneficios de rendimientos que aporta. El procedimiento siguiente que se describe en este apéndice muestra cómo habilitar e inhabilitar los controladores nativos mediante el mandato vSphere Command-Line (vCLI).

Antes de iniciar esta tarea, recupere todos los hosts físicos de direcciones IP de IPMI, los ID de inicio de sesión y las contraseñas del [portal de infraestructura de IBM Cloud](https://control.softlayer.com/devices). Esto es necesario en una copia de seguridad o para comprobar el progreso de una actualización, donde no existe ningún acceso de red directa al host.

Para cada host, de forma sucesiva:
1. Utilice el cliente web de vSphere para colocar el host de vSphere ESXi en modalidad de mantenimiento, seleccionando **Inicio** > **Hosts y clústeres**. En el panel Navegador, seleccione el host de ESXi de vSphere y pulse con el botón derecho del ratón en el host y seleccione **Modalidad de mantenimiento** > **Entrar en modalidad de mantenimiento**. Como el host forma parte de un clúster DRS automatizado, las máquinas virtuales se migran a hosts diferentes cuando el host entra en modalidad de mantenimiento.
2. Ejecute SSH en el host de vSphere ESXi con los detalles de la consola de IC4VS.
3. Ejecute el mandato vCLI siguiente para habilite los controladores nativos ixgben:  `esxcli system module set --enabled=true --module=ixgben`
4. Ejecute el siguiente mandato vCLI para rearrancar el host de vSphere ESXi: `system shutdown reboot -- reason "Install ixgben driver"`
5. Después de que se haya rearrancado el host de vSphere ESXI, mediante el registro SSH de nuevo en el host. Emita el siguiente mandato vCLI y compruebe que el controlador de ixgben esté "cargado" (la primera columna) y "habilitado" (segunda columna): `esxcli system module list |grep ixg`
6. Si los controladores están habilitados, con el cliente web de vSphere seleccione el host en el panel Navegador, pulse con el botón derecho del ratón y seleccione **Modalidad de mantenimiento** > **Salir de la modalidad de mantenimiento**. Seleccione el siguiente host y habilite los controladores hasta que se hayan realizado todos los hosts.
7. Si el cambio no funciona, ejecute el mandato siguiente para revertirlo: ` esxcli system module set -- enabled=false -- module=ixgben `

8. Si no puede conectar con el host a través de la red, ejecute el mandato anterior desde la consola de IPMI mediante la ventana de control de IBM Cloud.
9. Después de rearrancar el host ESXi de vSphere, ahora observará el controlador ixgbe predeterminado que se ha cargado y habilitado.

Si tiene que revertirlo y no puede hacer SSH en el host de vSphere ESXi, deberá iniciar la sesión en la consola de KVM para el host que necesita la reversión mediante la ventana de control de IBM Cloud.

Utilice el ID y la contraseña que aparecen en la lista de la ventana de control de IBM Cloud con la dirección IP de IPMI para iniciar sesión en la interfaz web de IPMI. Debe estar conectado al centro de datos en el que se encuentra el host mediante una VPN. Para obtener más información, consulte [Guía de iniciación a la VPN de IBM Cloud](https://console.bluemix.net/docs/infrastructure/iaas-vpn/getting-started.html#getting-started-with-virtual-private-networking-vpn).

1. Vaya a los detalles del dispositivo, página Gestión remota del host ESXi de vSphere y seleccione **Acciones** > **Consola de KVM**. Se abrirá otra ventana para que especifique el usuario y la contraseña de IPMI.
2. Seleccione **Control remoto** > **iKVM/HTML5** y pulse **iKVM/HTML5** para volver a iniciar la sesión. Ahora podrá acceder a la consola del host ESXi de vSphere.
3. Si el host está respondiendo a los mandatos, utilice **ALT-F1** en la consola para acceder a la consola de host ESXi. Utilice las credenciales del host para iniciar la sesión.
4. Si el host no responde, utilice los menús de IPMI para encender y apagar el host.
5. Observe detenidamente la consola HTML5 cuando se reinicie el host. Solo tiene unos pocos segundos para ir a la modalidad de recuperación cuando ESXi empieza a reiniciarse.
6. Pulse las teclas **CMD + R** simultáneamente para entrar en la modalidad de recuperación.
7. Escriba ** "Y" ** para especificar la modalidad de recuperación y para arrancar el servidor ESXi con la versión anterior.
8. Supervisar su progreso mediante la consola. Esto puede tardar entre 10 y 20 minutos.

### Enlaces relacionados

* [VMware HCX on IBM Cloud Solution](https://www.ibm.com/cloud/garage/files/HCX_Architecture_Design.pdf)
* [VMware Solutions on IBM Cloud Digital Technical Engagement](https://ibm-dte.mybluemix.net/ibm-vmware) (demos)