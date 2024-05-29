# DevOps Bootcamp

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

## Flujo de Trabajo

### Estructura del Repositorio
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

### Proceso de Despliegue

1. **Definición de Infraestructura con Terraform**: Terraform se utiliza para definir y provisionar el clúster de Kubernetes y los componentes necesarios en AWS.
2. **Gestión de Código con GitHub**: El código de la aplicación y las configuraciones se almacenan en repositorios de GitHub.
3. **Integración Continua con GitHub Actions**:
    - Se realiza el análisis de código y detección de vulnerabilidades con SonarQube.
    - Se construyen imágenes Docker de las aplicaciones y se almacenan en un registro de contenedores.
    - Se actualizan las configuraciones de despliegue utilizando Kustomization.
4. **Gestión del Estado de las Aplicaciones**: 
    - Dentro del repositorio `bootcamp-devops-gitops`, los directorios `production` y `development` para cada servicio contienen los manifiestos de Kubernetes necesarios para crear o actualizar los deployments y services.
    - Para crear el servicio en Kubernetes, el mismo repositorio tiene otro directorio de aplicaciones para ArgoCD, donde detecta cambios y crea el deployment en caso de que no exista o lo modifica de acuerdo a los cambios.
5. **Entrega Continua con ArgoCD**: ArgoCD monitorea el directorio de aplicaciones en el repositorio y sincroniza el estado del clúster de Kubernetes con el estado definido en los manifiestos de Git. Detecta cualquier cambio y aplica las actualizaciones necesarias para mantener el clúster en el estado deseado.
6. **Monitoreo y Visualización con Prometheus y Grafana**: Prometheus recolecta métricas del clúster y las aplicaciones, y Grafana visualiza estas métricas en dashboards interactivos.
