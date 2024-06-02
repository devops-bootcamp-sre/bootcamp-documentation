# DevOps Bootcamp

Se puede encontrar la documentacion en la [Wiki](https://github.com/devops-bootcamp-sre/bootcamp-devops-gitops/wiki/DevOps-Bootcamp) del proyecto 

## Menú de Navegación

- [Introducción](#introducción)
- [Tecnologías y Herramientas Utilizadas](#tecnologías-y-herramientas-utilizadas)
- [Estructura del Repositorio](#estructura-del-repositorio)
- [Definición de Infraestructura con Terraform](#definición-de-infraestructura-con-terraform)
- [Proceso de Despliegue](#proceso-de-despliegue)
    


## Introducción
Este proyecto utiliza un flujo de GitOps para gestionar y desplegar aplicaciones en un clúster de Kubernetes en AWS. Utilizamos varias herramientas y tecnologías para garantizar un ciclo de vida continuo de integración y entrega (CI/CD), monitoreo y gestión de la infraestructura.

## Tecnologías y Herramientas Utilizadas

- **AWS (Amazon Web Services)**
- **Kubernetes**
- **Terraform**
- **GitHub Actions**
- **SonarQube**
- **Docker**
- **Kustomization**
- **ArgoCD**
- **Prometheus**
- **Grafana**

## Estructura del Repositorio
El repositorio principal del proyecto es `bootcamp-devops-gitops`, que actúa como la fuente de verdad. Aquí se almacenan todos los servicios con los manifiestos necesarios para crear el deployment en Kubernetes. La estructura del repositorio es la siguiente:

```plaintext
bootcamp-devops-gitops/
│
├── service_name/
│   ├── production/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── kustomization.yaml
│   └── development/
│       ├── deployment.yaml
│       ├── service.yaml
│       └── kustomization.yaml
│
└── pipelines/
    └── service_name
```
## Definición de Infraestructura con Terraform (WIP)
Terraform se utiliza para definir y provisionar el clúster de Kubernetes y los componentes necesarios en AWS.

## Proceso de Despliegue

### Gestión de Código con GitHub
El código de la aplicación y las configuraciones se almacenan en repositorios de GitHub.

- El codigo para el backend de la applicacion vive en el el repo: [bootcamp-devops-api](https://github.com/devops-bootcamp-sre/bootcamp-devops-api).
- El codigo para el frontend de la applicacion vive en el el repo: [bootcamp-devops-web](https://github.com/devops-bootcamp-sre/bootcamp-devops-web).
- Lo manifiestos de kubernetes para la configuracion de los servicios vive en elrepo: [bootcamp-devops-gitops](https://github.com/devops-bootcamp-sre/bootcamp-devops-gitops).

### Integración Continua (CI) con GitHub Actions
En este proyecto, hemos utilizado GitHub Actions para implementar un flujo de Integración Continua (CI). A continuación se describen los pasos principales del flujo de trabajo:

**1. Construcción de la Imagen Docker**

El primer paso en nuestro flujo de CI es construir una imagen Docker. Esto se realiza utilizando un archivo Dockerfile presente en el repositorio. El proceso de construcción incluye:

- Configuración del entorno necesario.
- Instalación de dependencias.
- Copia del código fuente al contenedor.
- Compilación o configuración adicional según sea necesario.

**2. Etiquetado de la Imagen (Tagging)**

Una vez que la imagen Docker ha sido construida correctamente, el siguiente paso es etiquetar (taggear) la imagen. El etiquetado permite versionar la imagen y mantener un registro claro de las versiones construidas. Generalmente, se utilizan etiquetas que incluyen:

- El número de versión del proyecto.
- El identificador del commit o la fecha de construcción.

**3. Subida de la Imagen a Docker Hub**

Después de etiquetar la imagen, el siguiente paso es subirla a un registro de imágenes Docker, en este caso, Docker Hub. Esto facilita la distribución y despliegue de la imagen, ya que Docker Hub actúa como un repositorio centralizado accesible desde cualquier entorno que necesite ejecutar la imagen.

**4. Análisis de Vulnerabilidades con SonarQube**

Como parte de nuestro proceso de CI, hemos integrado SonarQube para realizar análisis de código y detección de vulnerabilidades. SonarQube proporciona un análisis estático del código fuente y genera informes sobre:

- Errores de codificación.
- Problemas de seguridad.
- ***Code smells*** o áreas de mejora.

Estos informes son esenciales para mantener la calidad y seguridad del código a lo largo del tiempo.

**5. Notificaciones a un Canal de Discord**

Finalmente, hemos configurado notificaciones para que se envíen a un canal de Discord. Estas notificaciones informan al equipo de desarrollo sobre el estado de las construcciones (builds) y cualquier problema detectado durante el proceso de CI. Esto ayuda a mantener a todos los miembros del equipo al tanto de la situación del proyecto y a responder rápidamente a cualquier problema.

#### Flujo de Trabajo en GitHub Actions

El flujo de trabajo de GitHub Actions se define en un archivo YAML dentro del directorio `.github/workflows/`. Este archivo especifica:

- Los disparadores (triggers) del flujo de trabajo (por ejemplo, push, pull request).
- Los trabajos (jobs) que se ejecutarán.
- Los pasos (steps) dentro de cada trabajo.

Cada paso puede incluir la ejecución de comandos, la utilización de acciones de terceros o la ejecución de scripts personalizados.

A continuación, se describen las secciones principales del archivo YAML utilizado en nuestro flujo de trabajo:

##### Disparadores (Triggers)

Especifican cuándo se debe ejecutar el flujo de trabajo. Ejemplos comunes incluyen:

- `on: push` - Se ejecuta en cada push al repositorio.
- `on: pull_request` - Se ejecuta en cada pull request creada o actualizada.

##### Trabajos (Jobs)

Define los trabajos que se ejecutarán en el flujo de trabajo. Cada trabajo puede tener múltiples pasos y puede ejecutarse en paralelo con otros trabajos. Ejemplos de trabajos incluyen:

- `build` - Para construir la imagen Docker.
- `analyze` - Para ejecutar el análisis con SonarQube.
- `deploy` - Para subir la imagen a Docker Hub.

3. **Gestión del Estado de las Aplicaciones**: 
    - Dentro del repositorio `bootcamp-devops-gitops`, los directorios `production` y `development` para cada servicio contienen los manifiestos de Kubernetes necesarios para crear o actualizar los deployments y services.
    - Para crear el servicio en Kubernetes, el mismo repositorio tiene otro directorio de aplicaciones para ArgoCD, donde detecta cambios y crea el deployment en caso de que no exista o lo modifica de acuerdo a los cambios.
4. **Entrega Continua con ArgoCD**: ArgoCD monitorea el directorio de aplicaciones en el repositorio y sincroniza el estado del clúster de Kubernetes con el estado definido en los manifiestos de Git. Detecta cualquier cambio y aplica las actualizaciones necesarias para mantener el clúster en el estado deseado.
6. **Monitoreo y Visualización con Prometheus y Grafana**: Prometheus recolecta métricas del clúster y las aplicaciones, y Grafana visualiza estas métricas en dashboards interactivos.



