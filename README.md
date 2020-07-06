---
Despliegue usando CircleCI Contra un Repositorio GitHub usando un docker-compose PROD
---

Configuration CircleCI
    
    - El archivo de configuración está dentro del directorio .circleci/config.yml
    - Configuración de las variables de entorno:
        > Pipelines (dentro del proyecto - Arriba - Dcha) > Project Settings > EnvironmentVariables
    - Configuración SSH:
        > 
 
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
        