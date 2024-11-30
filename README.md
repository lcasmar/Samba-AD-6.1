# Actividade 6.1 - Integración de equipos Windows en Samba AD con Ubuntu Server

Índice
- Configuración máquinas
- Unir el cliente windowsSambaClient al dominio
- Acceso a recursos compartidos desde nuestro cliente Windows
- Segmentar la red con VLANs

## Configuración máquinas
### Instalación máquina windows cliente Samba
Creamos una nueva máquina en VBox a la que vamos a llamar *windowsSambaClient*  
![](img/windows1.png)  

Instalamos el sistema operativo Windows 10

![](img/windows2.png)  

Una vez windows 10 esta instalado en la máquina le damos a esta configuración de red con un IP fija dentro de la misma red en la que se encuentra el servidor y el otro cliente samba *ubuntuClient*.  
![](img/windows3.png)  
![](img/windowsip.png)  
Le cambiamos el nombre al equipo por uno más intuitivo, en este caso *pc1*  
![](img/windows4.png)  

## Unir el cliente windowsSambaClient al dominio 
Comprobamos como desde el cliente resuelve para lcmaso.local 
![](img/ws.png)  

Unimos el cliente al dominio  
![](img/ws2.png)  

Introducimos las credenciales de administrador de samba y comprobamos que se une el cliente correctamente  
![](img/ws3.png)  

Reiniciamos el equipo 

Accedemos a la máquina con un usuario de dominio  
![](img/ws4.png)   
![](img/ws5.png)   

## Acceso a recursos compartidos desde nuestro cliente Windows
Para que el cliente pueda acceder a los recursos compartidos generados en la práctica anterior creamos en el servidor un directorio /nas/ y movemos ahí las carpetas compartidas: g1, g2, g3  
![](img/comp1.png)  
Ahora modificamos el archivo smb.conf para ajustar las rutas de los recursos compartidos que hemos movido  
![](img/comp2.png)   
Otorgamos permisos de lecto/escritura a los correspondientes recursos   
~~~
sudo chmod -R 755 /nas
sudo chown -R root:"domain users" /nas
~~~

Comprobamos como podemos acceder a los recursos compartidos  
![](img/comp3.png)  

Accedemos con el usuario usu2 y comprobamos las restricciones a las carpetas para este usuario   
![](img/comp4.png)  
Comprobamos también como podemos acceder a los recursos compartidos a través del nombre de dominio *usdc.lcmaso.local*
![](img/comp5dns.png)  


## Segmentar la red con VLANs
Para poder segmentar nuestra red con dos VLAN lo primero tenemos que hacer es configurar nuestro servidor como servidor dhcp, para ello primero modificamos la configuración de red en los adaptadores que van a configurar las VLAN.  
![](img/vlan1.png)  

Instalamos los paquetes necesarios para un servidor *dhcp*
~~~
sudo apt install openvswitch-switch-dpdk
sudo apt install isc-dhcp-server
~~~

Indicamos al servidor dhcp en que interfaces va a funcionar este mediante el archivo */etc/default/isc-dhcp-server*

![](img/vlan2.png)  

Modificamos el archivo dhcp.conf y definimos las VLAN como subredes   
![](img/vlan3.png)  
![](img/vlan4.png)  

Comprobamos el servicio dhcp esta activo  
![](img/vlan5.png)  

Ahora en los clientes cambiamos la configuración de red fija que les habíamos dado por una automática de forma que nuestro servidor dhcp les de ip  

![](img/vlan6.png)  
![](img/vlan8.png)  
Comprobamos como a cada máquina se le asigna una ip perteneciente a las correspondientes subredes de cada VLAN  
![](img/vlan7.png)  
![](img/vlan9.png)  




