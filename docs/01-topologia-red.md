# 01 - Armado de la topología de red del lab

**Fecha:** 2026-09-13
**Autor:** [Tu nombre]
**Categoría:** Infraestructura

---

## 1. Objetivo

Armar un entorno virtualizado con dos máquinas —una atacante (Kali Linux) y una
objetivo (Ubuntu Server)— conectadas dentro de la misma red interna, de forma
que puedan comunicarse entre sí y a la vez tener salida a internet para
actualizaciones y descarga de herramientas.

## 2. Contexto / Escenario

Para poder practicar ejercicios de ataque/detección de forma realista, ambas
máquinas necesitan estar en el mismo segmento de red, simulando una red
corporativa simplificada donde un atacante externo o interno intenta comprometer
un servidor.

## 3. Entorno utilizado

- **Hipervisor:** Oracle VirtualBox
- **VM 1 (Atacante):** Kali Linux
- **VM 2 (Objetivo):** Ubuntu Server 24.04 LTS
- **Tipo de red:** Red NAT de VirtualBox (`NatNetwork`), rango `10.0.2.0/24`,
  DHCP habilitado

## 4. Procedimiento

1. Instalación de Kali Linux en VirtualBox (máquina atacante).
2. Instalación de Ubuntu Server 24.04 LTS en VirtualBox (máquina objetivo):
   - 2048 MB de RAM, 2 CPUs asignadas
   - Disco de 25 GiB
   - Durante la instalación se habilitó el servidor OpenSSH para permitir
     administración remota
3. Verificación de que ambas VMs estén configuradas con el mismo adaptador de
   red: **Red NAT → NatNetwork**, en lugar de NAT simple (que aísla cada VM).
4. Verificación en el Administrador de Redes de VirtualBox de que la red
   `NatNetwork` exista, con DHCP habilitado.
5. Obtención de la IP asignada a cada VM mediante:

```bash
ip a
```

6. Prueba de conectividad entre ambas máquinas:

```bash
ping <ip_de_la_otra_maquina>
```

## 5. Hallazgos / Resultados

*(Completar con las IPs reales asignadas por DHCP y el resultado del ping,
una vez confirmadas.)*

| Máquina | IP asignada |
|---|---|
| Kali Linux | 10.0.2.x |
| Ubuntu Server | 10.0.2.x |

## 6. Análisis

El uso de una **Red NAT** (`NatNetwork`) en lugar de un NAT simple es clave:
el NAT simple de VirtualBox aísla cada máquina virtual en su propia red privada
sin visibilidad entre ellas, mientras que la Red NAT permite que todas las VMs
conectadas a ella compartan el mismo segmento y se vean entre sí, además de
mantener salida a internet a través del host.

## 7. Remediación / Contención

No aplica (práctica de infraestructura, no de incidente).

## 8. Lecciones aprendidas

- Durante la instalación de Ubuntu Server se observaron dos problemas comunes
  en entornos virtualizados que vale la pena documentar para el futuro:
  - **Demora prolongada durante el boot inicial** (pantalla aparentemente
    congelada tras "Loading essential drivers"), causada por falta de entropía
    para la generación de números aleatorios dentro de la VM. Se resolvió
    generando entropía manualmente (movimiento de mouse/teclado dentro de la VM).
  - **Advertencia de "soft lockup" del kernel** durante la etapa final de
    instalación, probablemente causada por el host quedándose momentáneamente
    sin recursos de CPU disponibles para la VM. El proceso se recuperó solo
    tras unos minutos sin intervención.
- Verificar siempre que ambas VMs de un lab multi-máquina usen el **mismo
  nombre de red NAT**, no solo el mismo tipo de adaptador.

## 9. Referencias

- [Documentación oficial de Ubuntu Server](https://ubuntu.com/server/docs)
- [VirtualBox Networking Modes](https://www.virtualbox.org/manual/ch06.html)
