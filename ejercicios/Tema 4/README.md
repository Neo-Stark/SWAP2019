# Ejercicios Tema 4

## T4.1: Buscar información sobre cuánto costaría en la actualidad un mainframe que tuviera las mismas prestaciones que una granja web con balanceo de carga y 10 servidores finales (p.ej)

En este [artículo](https://bits.blogs.nytimes.com/2013/07/23/mainframe-computers-that-change-with-the-times/) se hace una comparativa sobre un nuevo modelo de mainframe que sacó IBM a la venta, el *zEnterprise BC12* con un coste de 75,000$. Se habla sobre la potencia del mismo y sobre el abaratamiento de los equipos dando un ejemplo en el que hace 10 años los mainframes de IBM empezaban con un precio de entrada de 1 millón de $. Comparándolo con servidores normales x86 que forman las granjas web, los propios ingenieros de IBM aseguraban que este nuevo mainframe tiene la capacidad computacional al equivalente de 520 de estos servidores.

## T4.2: Implementar un pequeño servicio web en los servidores finales que devuelva el % CPU y % RAM que en un instante tiene en uso dicho servidor

```php
<?php
function get_server_memory_usage()
{
  if (stristr(PHP_OS, 'win')) {
    $wmi_found = false;
    $memory_usage = '';
    if ($wmi_query = wmiWBemLocatorQuery(
      "SELECT FreePhysicalMemory,FreeVirtualMemory,TotalSwapSpaceSize,TotalVirtualMemorySize,TotalVisibleMemorySize FROM Win32_OperatingSystem"
    )) {
      foreach ($wmi_query as $r) {
        $result['MemFree'] = $r->FreePhysicalMemory * 1024;
        $result['MemAvailable'] = $r->FreeVirtualMemory * 1024;
        $result['SwapFree'] = $r->TotalSwapSpaceSize * 1024;
        $result['SwapTotal'] = $r->TotalVirtualMemorySize * 1024;
        $result['MemTotal'] = $r->TotalVisibleMemorySize * 1024;
        $wmi_found = true;
      }
    }
  } else {
    $free = shell_exec('free');
    $free = (string)trim($free);
    $free_arr = explode("\n", $free);
    $mem = explode(" ", $free_arr[1]);
    $mem = array_filter($mem);
    $mem = array_merge($mem);
    $memory_usage = $mem[2] / $mem[1] * 100;
  }

  return $memory_usage;
}

function get_server_cpu_usage()
{

  $load = '';
  if (stristr(PHP_OS, 'win')) {
    $cmd = 'wmic cpu get loadpercentage /all';
    @exec($cmd, $output);
    if ($output) {
      foreach ($output as $line) {
        if ($line && preg_match('/^[0-9]+$/', $line)) {
          $load = $line;
          break;
        }
      }
    }
  } else {
    $sys_load = sys_getloadavg();
    $load = $sys_load[0];
  }
  return $load;
}

echo  "CPU" . get_server_cpu_usage() . "% MEM" . get_server_memory_usage() . "%";
```

El anterior código muestra por pantalla la carga de cpu y memoria actual del sistema en los SO Windows y Linux.

## T4.6: Buscar información sobre los bloques de IP para los distintos países o continentes. Implementar en JavaScript o PHP la detección de la zona desde donde se conecta un usuario

Las asignaciones IP se realizan de forma jerárquica (bloques contiguos de direcciones IPs). IANA asigna bloques a los Regional Internet Registry (RIR), e.g. RIPE NCC, en Europa). A su vez los RIR asignan baja demanda a los Local Internet Registry (LIR), e.g. ISP grandes, Movistar. En Europa a día de hoy (mayo de 2019) quedan disponibles 4.89 millones de direcciones IP. Para mas información consultar la página oficial del organismo [RIPE](https://www.ripe.net/).

Con este simple código podemos obtener la ciudad desde la que se conecta un cliente en concreto a nuestro servidor a través de su IP usando la página <http://ipinfo.io> como "base de datos", teniendo en cuenta los bloques de IP asignados a cada lugar que hemos comentado anteriormente.

```php
<?php
$ip = $_SERVER['REMOTE_ADDR'];
$details = json_decode(file_get_contents( "http://ipinfo.io/{$ip}/json"));
echo $details->city;
```