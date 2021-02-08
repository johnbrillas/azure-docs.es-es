---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/29/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 25404837d5bc66ff415be8d8670eb6650475c30f
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99094669"
---
## <a name="warm-up-the-cache"></a>Preparación de la memoria caché

El disco con almacenamiento en caché de host ReadOnly puede proporcionar un valor de IOPS mayor que el límite del disco. Para obtener este máximo rendimiento de lectura de la caché de host, primero debe preparar la memoria caché de este disco. Así se garantiza que las operaciones de E/S de lectura en las qué la herramienta de pruebas comparativas manejará el volumen de CacheReads alcanzan realmente la memoria caché y no el disco directamente. Los aciertos de caché generan más IOPS desde el único disco con la memoria caché habilitada.

> [!IMPORTANT]
> Debe preparar la memoria caché antes de ejecutar pruebas comparativas cada vez que se reinicie la máquina virtual.

## <a name="diskspd"></a>DISKSPD

[Descargue la herramienta DISKSP](https://github.com/Microsoft/diskspd) en la máquina virtual. DISKSPD es una herramienta que se puede personalizar para crear sus propias cargas de trabajo sintéticas. Usaremos la misma configuración descrita anteriormente para ejecutar pruebas comparativas. Puede cambiar las especificaciones para probar diferentes cargas de trabajo.

En este ejemplo, usamos el siguiente conjunto de parámetros de base de referencia:

- -c200G: crea (o vuelve a crear) el archivo de ejemplo que se usa en la prueba. Se puede establecer en bytes, KiB, MiB, GiB o bloques. En este caso, se usa un archivo grande del archivo de destino 200-GiB para minimizar el almacenamiento en caché de la memoria.
- -w100: especifica el porcentaje de operaciones que son solicitudes de escritura (-w0 equivale a 100 % de lectura).
- -b4K: indica el tamaño de bloque en bytes, KiB, MiB o GiB. En este caso, se usa el tamaño de bloque de 4 K para simular una prueba de E/S aleatoria.
- -F4: establece un total de cuatro subprocesos.
- -r: indica la prueba de E/S aleatoria (invalida el parámetro -s).
- -o128: indica el número de solicitudes de E/S pendientes por destino por subproceso. Esto también se conoce como profundidad de cola. En este caso, se usa 128 para recargar la CPU.
- -W7200: especifica la duración del tiempo de preparación antes de que se inicien las medidas.
- -d30: especifica la duración de la prueba, sin incluir la preparación.
- -Sh: deshabilita el almacenamiento en caché de escritura de software y hardware (equivalente a -Suw).

Para una lista completa de parámetros, consulte el [repositorio de GitHub](https://github.com/Microsoft/diskspd/wiki/Command-line-and-parameters).

### <a name="maximum-write-iops"></a>Valor máximo de IOPS de escritura
Usamos una profundidad de cola alta de 128, un tamaño de bloque pequeño de 8 KB y cuatro subprocesos de trabajo para dirigir las operaciones de escritura. Los trabajos de escritura dirigen el tráfico del volumen "NoCacheWrites", que tiene tres discos con la memoria caché establecida en "None".

Ejecute el siguiente comando durante 30 segundos de preparación y 30 segundos de medición:

`diskspd -c200G -w100 -b8K -F4 -r -o128 -W30 -d30 -Sh testfile.dat`

Los resultados muestran que la máquina virtual Standard_D8ds_v4 proporciona el límite máximo de IOPS de escritura de 12 800.

:::image type="content" source="../articles/virtual-machines/linux/media/premium-storage-performance/disks-benchmarks-diskspd-max-write-io-per-second.png" alt-text="Para un total de 3 208 642 560 bytes, un máximo de E/S totales de 391 680, con un total de 101,97 MiB/s y un total de 13 052,65 E/S por segundo.":::

### <a name="maximum-read-iops"></a>Valor máximo de IOPS de lectura

Usamos una profundidad de cola alta de 128, un tamaño de bloque pequeño de 4 KB y cuatro subprocesos de trabajo para dirigir las operaciones de lectura. Los trabajos de lectura dirigen el tráfico del volumen "CacheReads", que tiene un disco con la memoria caché establecida en "ReadOnly".

Ejecute el siguiente comando durante dos horas de preparación y 30 segundos de medición:

`diskspd -c200G -b4K -F4 -r -o128 -W7200 -d30 -Sh testfile.dat`

Los resultados muestran que la máquina virtual Standard_D8ds_v4 proporciona el límite máximo de IOPS de lectura de 77 000.

:::image type="content" source="../articles/virtual-machines/linux/media/premium-storage-performance/disks-benchmarks-diskspd-max-read-io-per-second.png" alt-text="En el caso de 9 652 785 152 bytes totales, hubo 2 356 637 E/S totales, a 306,72 MiB/s totales, y un total de 78 521,23 E/S por segundo.":::

### <a name="maximum-throughput"></a>Rendimiento máximo

Para obtener el máximo rendimiento de lectura y escritura, puede cambiar a un tamaño de bloque mayor de 64 KB.
## <a name="fio"></a>FIO

FIO es una popular herramienta para el almacenamiento de información de referencia en las máquinas virtuales de Linux. Tiene flexibilidad para seleccionar distintos tamaños de E/S y lecturas y escrituras secuenciales o aleatorias. Genera subprocesos de trabajo o procesos para realizar las operaciones de E/S especificadas. Puede especificar el tipo de operaciones de E/S que debe realizar cada subproceso de trabajo con archivos de trabajo. Hemos creado un archivo de trabajo por escenario que se ilustra en los ejemplos siguientes. Puede cambiar las especificaciones de estos archivos de trabajo para tener referencia de diferentes cargas de trabajo en Premium Storage. En los ejemplos, usamos la máquina virtual Standard_D8ds_v4 que ejecuta **Ubuntu**. Use la misma configuración descrita al principio de la sección de pruebas comparativas y prepare la memoria caché antes de ejecutar dichas pruebas.

Antes de comenzar, [descargue FIO](https://github.com/axboe/fio) e instálelo en la máquina virtual.

Ejecute el siguiente comando para Ubuntu:

```
apt-get install fio
```

Usamos cuatro subprocesos de trabajo para realizar las operaciones de escritura y cuatro subprocesos de trabajo para realizar las operaciones de lectura en los discos. Los trabajos de escritura dirigen el tráfico del volumen "nocache", que tiene tres discos con la memoria caché establecida en "None". Los trabajos de lectura dirigen el tráfico del volumen "readcache", que tiene un disco con la memoria caché establecida en "ReadOnly".

### <a name="maximum-write-iops"></a>Valor máximo de IOPS de escritura

Cree el archivo de trabajo con las especificaciones siguientes para obtener la IOPS de escritura máxima. Asígnele el nombre "fiowrite.ini".

```ini
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=4k
numjobs=4

[writer1]
rw=randwrite
directory=/mnt/nocache
```

Tenga en cuenta los siguientes aspectos clave que están en consonancia con las instrucciones de diseño que se tratan en secciones anteriores. Estas especificaciones son esenciales para alcanzar la IOPS máxima  

* Una profundidad de la cola alta de 256.  
* Un tamaño de bloque pequeño de 4 KB.  
* Varios subprocesos que realizan escrituras aleatorias.

Ejecute el siguiente comando para ejecutar la prueba FIO durante 30 segundos:  

```
sudo fio --runtime 30 fiowrite.ini
```

Mientras se ejecuta la prueba, puede ver el número de IOPS de escritura que envían la máquina virtual y los discos Premium. Como se muestra en el ejemplo siguiente, la máquina virtual Standard_D8ds_v4 proporciona su límite máximo de IOPS de escritura: 12 800 IOPS.  
    :::image type="content" source="../articles/virtual-machines/linux/media/premium-storage-performance/fio-uncached-writes-1.jpg" alt-text="Número de IOPS de escritura que las máquinas virtuales y los discos SSD prémium entregan, muestra que las escrituras son 13 100 IOPS.":::

### <a name="maximum-read-iops"></a>Valor máximo de IOPS de lectura

Cree el archivo de trabajo con las especificaciones siguientes para obtener la IOPS de lectura máxima. Asígnele el nombre "fioread.ini".

```ini
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=4k
numjobs=4

[reader1]
rw=randread
directory=/mnt/readcache
```

Tenga en cuenta los siguientes aspectos clave que están en consonancia con las instrucciones de diseño que se tratan en secciones anteriores. Estas especificaciones son esenciales para alcanzar la IOPS máxima

* Una profundidad de la cola alta de 256.  
* Un tamaño de bloque pequeño de 4 KB.  
* Varios subprocesos que realizan escrituras aleatorias.

Ejecute el siguiente comando para ejecutar la prueba FIO durante 30 segundos:

```
sudo fio --runtime 30 fioread.ini
```

Mientras se ejecuta la prueba, puede ver el número de IOPS de lectura que envían los discos Premium y de VM. Como se muestra en el ejemplo siguiente, la máquina virtual Standard_D8ds_v4 proporciona más de 77 000 IOPS de lectura. Se trata de una combinación del rendimiento de la caché y el disco.  
    :::image type="content" source="../articles/virtual-machines/linux/media/premium-storage-performance/fio-cached-reads-1.jpg" alt-text="Captura de pantalla del número de IOPS de escritura que la máquina virtual y los discos SSD prémium entregan, muestra que las lecturas son 78 600.":::

### <a name="maximum-read-and-write-iops"></a>Valor máximo de IOPS de lectura y escritura

 Cree el archivo de trabajo con las especificaciones siguientes para obtener la IOPS de lectura y escritura combinadas máxima. Asígnele el nombre "fioreadwrite.ini".

```ini
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k
numjobs=4

[reader1]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=3200
```

Tenga en cuenta los siguientes aspectos clave que están en consonancia con las instrucciones de diseño que se tratan en secciones anteriores. Estas especificaciones son esenciales para alcanzar la IOPS máxima

* Una profundidad de la cola alta de 128.  
* Un tamaño de bloque pequeño de 4 KB.  
* Varios subprocesos que realizan lecturas y escrituras aleatorias.

Ejecute el siguiente comando para ejecutar la prueba FIO durante 30 segundos:

```
sudo fio --runtime 30 fioreadwrite.ini
```

Mientras se ejecuta la prueba, puede ver el número de IOPS de lectura y escritura combinadas que envían la máquina virtual y los discos Premium. Como se muestra en el ejemplo siguiente, la máquina virtual Standard_D8ds_v4 proporciona más de 90 000 IOPS de lectura y escritura combinadas. Se trata de una combinación del rendimiento de la caché y el disco.  
    :::image type="content" source="../articles/virtual-machines/linux/media/premium-storage-performance/fio-both-1.jpg" alt-text="Las IOPS de lectura y escritura combinadas muestran que las lecturas son 78 300 y las escrituras son 12 600 IOPS.":::

### <a name="maximum-combined-throughput"></a>Rendimiento máximo combinado

 Para obtener el rendimiento de lectura y escritura combinado máximo, use un tamaño de bloque y la profundidad de la cola más grandes con varios subprocesos que realizan lecturas y escrituras. Puede usar un tamaño de bloque de 64 KB y una profundidad de la cola de 128.