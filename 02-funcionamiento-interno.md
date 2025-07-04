# 2. ¿Cómo funciona Kubernetes por dentro?

Para comprender realmente Kubernetes, es fundamental conocer cómo se organiza internamente, qué componentes lo conforman y cómo interactúan entre ellos. Esta sección explica la arquitectura base del clúster y los servicios esenciales que lo hacen funcionar.

---

## 2.1. etcd: la base de datos de Kubernetes

`etcd` es una **base de datos clave-valor distribuida** que actúa como el **almacén central** de todo el estado del clúster.

- Almacena el estado deseado: Pods, ConfigMaps, Secrets, Deployments, etc.
- Todos los cambios en Kubernetes pasan por la API y se guardan en `etcd`.
- Es altamente consistente y tolerante a fallos.

> Ejemplo: cuando aplicamos un manifiesto YAML con `kubectl apply`, ese archivo se convierte en una petición a la API de Kubernetes, que registra la información en `etcd`.

---

## 2.2. Componentes del plano de control (Control Plane)

El plano de control orquesta el comportamiento global del clúster. Sus componentes principales son:

| Componente                  | Descripción                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| `kube-apiserver`           | El **punto de entrada del clúster**. Expone la API REST de Kubernetes.     |
| `etcd`                     | La base de datos donde se guarda el estado deseado del clúster.            |
| `kube-scheduler`           | Decide en qué nodo se ejecutará cada Pod nuevo.                            |
| `kube-controller-manager`  | Ejecuta los controladores que supervisan y ajustan el estado del clúster.  |
| `cloud-controller-manager` | Gestiona recursos específicos del proveedor cloud (volúmenes, LB, etc.).   |

---

## 2.3. Componentes de los nodos (Worker Nodes)

Los nodos trabajadores son los encargados de **ejecutar los contenedores**. Sus componentes son:

| Componente        | Función                                                                 |
|-------------------|-------------------------------------------------------------------------|
| `kubelet`         | Agente que comunica el nodo con el plano de control y ejecuta Pods.     |
| `kube-proxy`      | Configura reglas de red y balanceo de carga para los servicios.         |
| `container runtime` | Es quien ejecuta realmente los contenedores (Docker, Containerd, etc.). |

---

## 2.4. Pods del sistema (`kube-system`)

El namespace `kube-system` contiene los Pods que aseguran el funcionamiento básico del clúster:

| Pod / Add-on               | Función                                                              |
|---------------------------|----------------------------------------------------------------------|
| `coredns`                 | Servicio DNS interno para resolver nombres dentro del clúster.       |
| `kube-proxy`             | Reglas de red y forwarding del tráfico interno.                      |
| `metrics-server` (opcional) | Permite el escalado automático (HPA) mediante métricas.           |
| `kube-apiserver`, `scheduler`, etc. | A menudo ejecutados como Pods si usas herramientas como kubeadm. |

---

## 2.5. Seguridad y autenticación

El `kube-apiserver` también gestiona:

- Autenticación de usuarios (certificados, tokens, OIDC, etc.).
- Control de acceso mediante **RBAC** (roles y permisos).
- Validación de peticiones y autorización de acciones.

---

## 2.6. Comunicación entre componentes

El funcionamiento interno se basa en el principio de **estado deseado vs estado real**.

1. El usuario define un estado deseado (por ejemplo, 3 réplicas de una app).
2. El `kube-controller-manager` compara ese estado con el real (almacenado en etcd).
3. Si detecta una diferencia, actúa: por ejemplo, crea o elimina Pods automáticamente.

---

Kubernetes funciona como un **sistema reactivo** que busca siempre alinear el estado real con el deseado, sin intervención directa del usuario una vez configurado.
