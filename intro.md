# 1. Introducción a Kubernetes

## ¿Qué es Kubernetes?

**Kubernetes** (abreviado a menudo como **K8s**) es una plataforma de orquestación de contenedores de código abierto que permite automatizar el despliegue, la escalabilidad y la gestión de aplicaciones contenerizadas.

Fue desarrollada por Google y actualmente está mantenida por la Cloud Native Computing Foundation (CNCF). Es compatible con cualquier infraestructura: local, en la nube pública o en entornos híbridos.

## ¿Por qué utilizar Kubernetes?

Kubernetes ofrece múltiples ventajas para desplegar y mantener aplicaciones modernas:

- **Escalado automático** de aplicaciones según la demanda.
- **Rollouts y rollbacks** controlados.
- **Recuperación automática** de contenedores fallidos.
- **Separación lógica por entornos** (dev, pre, prod) mediante namespaces.
- **Independencia del proveedor**: funciona en local o en cualquier nube.

## ¿Cómo funciona un clúster de Kubernetes?

Un **clúster de Kubernetes** se compone de dos grandes bloques:

- **Control Plane (o plano de control)**: gestiona el estado global del clúster. Incluye componentes como el `kube-apiserver`, `scheduler` y `controller-manager`.

- **Nodos (workers)**: son máquinas físicas o virtuales donde se ejecutan los contenedores (Pods). Cada nodo incluye un `kubelet` y un `container runtime`.

Toda la comunicación y gestión de recursos se realiza a través de la API de Kubernetes.
