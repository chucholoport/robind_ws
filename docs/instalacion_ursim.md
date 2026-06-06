## Guía completa URSim + VMware en Ubuntu (caso real con Secure Boot)

---

## 1. Contexto del problema

Al instalar y ejecutar URSim en Ubuntu moderno (24.04 con kernel 6.x y VMware Workstation 26), pueden aparecer errores como:

* `Could not open /dev/vmmon`
* `Virtual machine monitor failed`
* `Virtual ethernet failed`
* `Unable to start services`

Estos errores no provienen de URSim directamente, sino de la capa de virtualización (VMware + kernel Linux).

---

## 2. Entorno utilizado (caso funcional real)

* Ubuntu: 24.04 LTS
* Kernel: 6.17.x
* VMware Workstation Pro: 26.0.0
* Secure Boot: Enabled
* URSim: imagen virtual oficial (Non-Linux VMware appliance)

---

## 3. Enlaces oficiales

### 3.1 URSim (máquina virtual oficial)

[https://www.universal-robots.com/download/software-ur-series/simulator-non-linux/offline-simulator-ur-series-e-series-ur-sim-for-non-linux-5252/](https://www.universal-robots.com/download/software-ur-series/simulator-non-linux/offline-simulator-ur-series-e-series-ur-sim-for-non-linux-5252/)

---

### 3.2 Guía oficial URSim (PDF)

[https://academy.universal-robots.com/media/r3xlna5e/ursim_vmoracle_installation_guide_v3_es.pdf](https://academy.universal-robots.com/media/r3xlna5e/ursim_vmoracle_installation_guide_v3_es.pdf)

---

### 3.3 VMware Workstation Pro

[https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion)

---

## 4. Nota importante: Windows vs Ubuntu

### ✔ Instalación en Windows

En Windows:

* Instalación directa de VMware Workstation Pro
* Importación de VM (`.vmx`)
* Ejecución inmediata de URSim

👉 No existen problemas de kernel ni Secure Boot

👉 No se requieren pasos adicionales

---

### ⚠ Instalación en Ubuntu (caso de esta guía)

En Linux moderno, VMware depende de módulos del kernel que deben compilarse localmente.

Problemas típicos:

* Secure Boot bloquea módulos no firmados
* Kernel 6.x introduce cambios ABI frecuentes
* VMware usa módulos DKMS (`vmmon`, `vmnet`)

---

## 5. Problema raíz

VMware compila e inyecta módulos:

* `vmmon.ko` → monitor de virtualización
* `vmnet.ko` → red virtual

Con Secure Boot activo:

* El kernel exige firmas válidas
* Módulos compilados localmente no están firmados
* Se rechaza su carga

Resultado:

* `/dev/vmmon` no existe
* Servicios VMware fallan al iniciar

---

## 6. Evidencia del problema

Ejemplo de fallo:

```text
Virtual machine monitor failed
Virtual ethernet failed
Unable to start services
```

Y en nivel kernel:

* módulos presentes pero no cargables
* errores de firma o rechazo silencioso

---

## 7. Solución implementada (caso real funcional)

### 7.1 Verificar Secure Boot

```bash
mokutil --sb-state
```

Salida esperada:

```text
SecureBoot enabled
```

---

### 7.2 Generar clave MOK

```bash
mkdir -p ~/vmware-signing
cd ~/vmware-signing

openssl req -new -x509 -newkey rsa:2048 \
-keyout MOK.priv \
-outform DER \
-out MOK.der \
-nodes \
-days 36500 \
-subj "/CN=VMware Module Signing/"
```

---

### 7.3 Registrar clave en UEFI (MOK)

```bash
sudo mokutil --import MOK.der
```

* Definir contraseña temporal
* Reiniciar sistema
* Entrar a MOK Manager
* Enroll MOK → Confirmar

---

### 7.4 Recompilar módulos VMware

```bash
sudo vmware-modconfig --console --install-all
```

---

### 7.5 Localizar módulos generados

```bash
find /lib/modules/$(uname -r) -name "vmmon.ko*" -o -name "vmnet.ko*"
```

Ejemplo:

* `/lib/modules/.../misc/vmmon.ko`
* `/lib/modules/.../misc/vmnet.ko`

---

### 7.6 Firmar módulos manualmente

```bash
SIGN_FILE="/usr/src/linux-headers-$(uname -r)/scripts/sign-file"

sudo $SIGN_FILE sha256 \
~/vmware-signing/MOK.priv \
~/vmware-signing/MOK.der \
/lib/modules/$(uname -r)/misc/vmmon.ko

sudo $SIGN_FILE sha256 \
~/vmware-signing/MOK.priv \
~/vmware-signing/MOK.der \
/lib/modules/$(uname -r)/misc/vmnet.ko
```

---

### 7.7 Actualizar dependencias del kernel

```bash
sudo depmod -a
```

---

### 7.8 Cargar módulos

```bash
sudo modprobe vmmon
sudo modprobe vmnet
```

---

### 7.9 Verificación final

```bash
lsmod | grep -E "vmmon|vmnet"
```

Resultado esperado:

* vmmon cargado
* vmnet cargado

---

## 8. Resultado final

Después del procedimiento:

* VMware Workstation inicia correctamente
* URSim arranca sin errores
* `/dev/vmmon` se crea correctamente
* Secure Boot permanece activo
* Sistema mantiene seguridad completa

---

## 9. Observaciones importantes para alumnos

* El problema NO es URSim
* Es una interacción entre:

  * Kernel Linux moderno
  * Secure Boot
  * Módulos VMware DKMS
* Cada actualización de kernel puede requerir re-firmado

---

## 10. Alternativas de solución

### Opción 1 (recomendada en clase)

* VM oficial URSim

### Opción 2 (entornos profesionales)

* Docker URSim + ROS2 integration

### Opción 3 (más simple pero menos segura)

* Desactivar Secure Boot

---

## 11. Conclusión

* Windows: instalación directa sin fricción
* Linux: requiere manejo de Secure Boot + firma de módulos
* La solución implementada permite mantener seguridad del sistema sin sacrificar funcionalidad

---
