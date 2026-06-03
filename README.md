# LAB_azure-hub-and-spoke-architecture

Implementación de Arquitectura de Red Resiliente y Aislamiento de Entornos mediante Topología Hub-and-Spoke en Azure
Introducción
Este proyecto demuestra el despliegue de una infraestructura de red segura y altamente estructurada en la nube bajo los pilares de segmentación perimetral, control de flujo de tráfico y aislamiento de entornos. Utilizando los servicios de Microsoft Azure, se configuró una topología híbrida de tipo Hub-and-Spoke orientada al sector empresarial. Para validar el comportamiento dinámico y las políticas de enrutamiento del sistema, se realizaron pruebas de conectividad ICMP y saltos lógicos de administración vía SSH, forzando la verificación del principio de no-transitividad de los enlaces para garantizar que los entornos críticos de producción y desarrollo permanezcan completamente aislados de forma eficiente y transparente.

* Azure Resource Groups
* Azure Virtual Networks
* Azure VNet Peerings
* Azure Virtual Machines
* Network Security Groups

Topología
<img width="968" height="713" alt="image" src="https://github.com/user-attachments/assets/dbf820d7-44b4-400b-a786-4c5c4d4ac81e" />

Paso-A-Paso

Creación e inicialización del Grupo de Recursos principal denominado RG-HBANDSPK-LAB en la región East US. Este contenedor alojara de forma centralizada todos los componentes de cómputo y conectividad lógica del proyecto
<img width="1684" height="872" alt="image" src="https://github.com/user-attachments/assets/1bd00daf-3948-42fc-bc10-cb8a73fd6643" />

Despliegue y diseño jerárquico de las tres redes de la topología dentro del mismo grupo de recursos para garantizar el orden de la arquitectura.
<img width="817" height="702" alt="image" src="https://github.com/user-attachments/assets/3debeec5-31d7-4456-8a93-88e1e2d3ae7c" />

Subnet-Core: Configuración del direccionamiento base IPv4 10.0.1.0/24 asociado a la red central VNet-Hub.
<img width="1101" height="421" alt="image" src="https://github.com/user-attachments/assets/a4cf3a7b-e31b-4b57-959c-4034f5efd3f7" />

Subnet-Prod: Segmentación de la red del entorno de producción VNet-Spoke-Prod utilizando el rango IPv4 10.1.1.0/24.
<img width="1099" height="490" alt="image" src="https://github.com/user-attachments/assets/a443c67d-3b4e-4bab-a41f-9190d8b117c3" />

Subnet-Dev: Aislamiento del entorno de desarrollo VNet-Spoke-Dev mediante la asignación del rango IPv4 10.2.1.0/24.
<img width="1113" height="425" alt="image" src="https://github.com/user-attachments/assets/fa45728e-899d-4f5c-9be8-504c14b8ab0f" />

Consolidación y auditoría visual de los espacios de red creados bajo la misma suscripción activa de Azure.
<img width="1252" height="201" alt="image" src="https://github.com/user-attachments/assets/d226a164-570e-457a-a737-c73270f86dad" />

Configuración de los enlaces lógicos de comunicación privada desde el nodo central VNet-Hub. El motor de Azure genera automáticamente el enrutamiento bidireccional correspondiente.
<img width="1689" height="866" alt="image" src="https://github.com/user-attachments/assets/2984ace7-8816-4b1b-948e-1f90366ebc1a" />

Hub-to-Prod: Acoplamiento directo de la red central con el segmento de producción.
<img width="677" height="728" alt="image" src="https://github.com/user-attachments/assets/28036df9-fb90-4d25-980f-b12d7b3854c4" />

Hub-to-Dev: Acoplamiento directo de la red central con el segmento de desarrollo.
<img width="655" height="726" alt="image" src="https://github.com/user-attachments/assets/ec9ac492-6903-465a-9667-3b2bf8ece756" />

Validación de la conectividad en el portal: La tabla de interconexiones confirma un estado síncrono óptimo de Connected para ambos enlaces lógicos.
<img width="1285" height="731" alt="image" src="https://github.com/user-attachments/assets/69ff7da2-eeb7-4fe9-8998-9e7f35a21fc9" />

Despliegue de la máquina virtual central denominada VM-Hub utilizando una imagen base Linux (Ubuntu Server 24.04 LTS)
<img width="952" height="802" alt="image" src="https://github.com/user-attachments/assets/a9c1c0c9-196e-43a8-8b8f-882ed2248181" />

Configuración de red de la VM central asignándole una IP pública temporal para que funcione de manera controlada como un Jumpbox de administración. 
<img width="966" height="830" alt="image" src="https://github.com/user-attachments/assets/849094f1-d681-4be9-b5da-370f87f6d79d" />

Despliegue y aprovisionamiento de la instancia aislada VM-Prod dentro de su respectiva subred productiva sin exposición directa a internet (Public IP: None).
<img width="721" height="723" alt="image" src="https://github.com/user-attachments/assets/bce7cbb4-ef0e-4e34-91db-e974ecd20ba0" />

Despliegue y aprovisionamiento de la instancia aislada VM-Dev dentro de su subred de desarrollo, manteniendo un esquema de seguridad estricto sin IP pública.
<img width="1299" height="864" alt="image" src="https://github.com/user-attachments/assets/0b9ef0c1-30d2-48ed-93fa-be26c1385447" />

Validación en el panel de cómputo de la ejecución simultánea de los tres nodos (Running) bajo un tamaño optimizado de cómputo.
<img width="1059" height="132" alt="image" src="https://github.com/user-attachments/assets/bc87cf9f-efad-4971-9da5-547758f9ec61" />

Acceso remoto seguro al nodo pasarela VM-Hub vía SSH utilizando su interfaz de direccionamiento público y las credenciales del administrador.  ssh usuario@iPpublica
<img width="930" height="169" alt="image" src="https://github.com/user-attachments/assets/df904828-b1be-4ce9-81de-057dc7766010" />

Auditoría de direccionamiento interno desde el portal para mapear la IP privada de destino de la zona de producción 10.1.1.4.
<img width="1218" height="876" alt="image" src="https://github.com/user-attachments/assets/dc4ce78b-534c-4579-9ba8-d702d84e6d82" />

Ejecución de la primera prueba ICMP exitosa 0% packet loss verificando que el tráfico fluye de forma nativa desde el Hub hacia la red de producción. 
<img width="602" height="225" alt="image" src="https://github.com/user-attachments/assets/4d95ab38-c487-4b4b-b22f-4fe4dfeb4d50" />

Mapeo complementario de la interfaz privada de desarrollo 10.2.1.5 y validación del tráfico exitoso desde el Hub hacia desarrollo, confirmando la salud de ambos Peerings corporativos.
<img width="1209" height="612" alt="image" src="https://github.com/user-attachments/assets/f15f9e6f-e58d-4b55-bd60-5201bec4104a" />

Se puede ver como desde el hub tenemos ping hacia ambas maquinas, tanto dev como prod.
<img width="612" height="418" alt="image" src="https://github.com/user-attachments/assets/0f2fe3cd-e233-4453-b277-1291ecafb471" />

Ejecución de un salto lógico SSH interno desde la terminal del Hub para autenticarse directamente en la consola privada de la máquina
Validación del Aislamiento Total: Inyección de tráfico ICMP desde la máquina de producción hacia la IP privada de desarrollo. La instrucción arroja un fallo crítico absoluto por Timeout (100% packet loss). Esto constata de forma empírica que el Peering de Azure no es transitivo, demostrando que los entornos de negocio están blindados y aislados con éxito. 
<img width="730" height="316" alt="image" src="https://github.com/user-attachments/assets/b171f9d1-c82f-4f74-b67c-470673e5d914" />
