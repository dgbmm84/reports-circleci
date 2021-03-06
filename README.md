---
Despliegue usando CircleCI Contra un Repositorio GitHub usando un docker-compose PROD
---

Configuration CircleCI
    
    - El archivo de configuración está dentro del directorio .circleci/config.yml
    - Configuración de las variables de entorno:
        > Pipelines (dentro del proyecto - Arriba - Dcha) > Project Settings > EnvironmentVariables
    - Configuración SSH:
        > Since CircleCI cannot decrypt SSH keys, every new key must have an empty passphrase. CircleCI also will not accept OpenSSH’s default file format - use ssh-keygen -m pem if you are using OpenSSH to generate your key.
        > Pipelines (dentro del proyecto - Arriba - Dcha) > Project Settings > SSH Keys > Additional SSH Keys
        > Guardamos las id_rsa openssh que teníamos de otros proyectos y generamos una nueva RSA para este proyecto
            > Creamos una llave con la privada y lo añadimos al proyecto de CircleCI
                Esto nos da un firgerprint que usaremos en nuestra plantilla
        > Añadimos al repositorio del que bebe CircleCI (GitHub) la nueva ssh key public al apartado de Settings para poder hacer git push de los cambios del proyecto
        > Como estamos usando ngrok ya teníamos añadida la SSH con formato OPENSSH , debemos añadir la nueva RSA ahora tambien para que permita la conexión 
        > Por ultimo, al crear una nueva ssh rsa hemos de añadirlo al authorized_keys la id_rsa.pub del host destino.
        
Host de despliegue 

    - Es local haciendo uso de ngrok para configurar un tunnel por tcp dad una ip pública:
    - No hay necesidad de configurar una VM o VPS
    - Ngrok
        - ngrok tcp 22
        - Actualizar fabfile con el puerto otorgado por ngrok
        
Configuración del host destino

    - Instalación de Docker / Docker-Compose
    - Ruta de despliegue configurada en fabfile.py
        - En dicha ruta haber realizado previamente "git init" para inicialiar repo
        - Configuración del .gti/config
        
Despliegue

    - Git push origin master 
    - Arranca los servicios de docker en la máquina destino
        - docker exec -it app_prod bash
        - Las variables de la aplicación están declaradas en el apartado "Pipelines (dentro del proyecto - Arriba - Dcha) > Project Settings > EnvironmentVariables" y se injectan automáticatimante.  
            - Fabric usa para task arguments un tratamiento especial en los caracteres que hay que aplicar a las variables de entorno 

Comandos

    - docker ps
        Mostrará tres servicios levantados (app / nginx / mysql)
        Para para los contenedores y eliminar volumenes docker-compose -f docker-compose-prod.yml down (-v)
        