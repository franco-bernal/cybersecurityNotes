
# Escaneo de Vulnerabilidades desde una Máquina Local

Este archivo README explica cómo puedes ejecutar herramientas de escaneo de vulnerabilidades como **OWASP ZAP**, **SQLMap**, y **Wapiti** desde tu máquina local para escanear aplicaciones web remotas.

### 1. OWASP ZAP desde tu Local

Puedes correr **OWASP ZAP** en tu máquina local y configurarlo para escanear un sitio web remoto. Si la web es accesible desde Internet, no hay problema en ejecutar el escaneo localmente.

#### Pasos para usar ZAP desde tu Local:

1. **Descargar e instalar OWASP ZAP**:
   - Descarga OWASP ZAP desde su sitio oficial: [OWASP ZAP](https://www.zaproxy.org/download/)
   - Instálalo y ejecútalo en tu máquina local.

2. **Configurar ZAP para escanear una URL remota**:
   - Cuando ZAP esté corriendo, en la interfaz gráfica puedes ingresar la **URL remota** de tu aplicación (por ejemplo, `http://test.pz.cl`).
   - En la interfaz, selecciona la opción **"Spider"** para rastrear todas las URLs del sitio y luego ejecuta un escaneo de vulnerabilidades usando la opción de **Active Scan**.

3. **Ejecutar el script de Python localmente**:
   Si prefieres usar Python para interactuar con ZAP, puedes ejecutar el siguiente script desde tu máquina local:

   ```python
   from zapv2 import ZAPv2
   import time

   zap = ZAPv2(apikey='your-api-key', proxies={'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'})

   # URL objetivo que es remota
   target = 'http://test.pz.cl'

   # Inicia el escaneo del sitio
   print(f"Escaneando objetivo: {target}")
   zap.urlopen(target)  # Abre la URL en ZAP
   zap.spider.scan(target)  # Usa el spider para rastrear todas las URLs

   # Espera a que el spider termine
   while int(zap.spider.status()) < 100:
       print(f"Spider en progreso: {zap.spider.status()}%")
       time.sleep(2)

   print("Spider completo. Escaneando en busca de vulnerabilidades...")

   # Inicia el escaneo de vulnerabilidades
   zap.ascan.scan(target)

   # Espera a que el escaneo de vulnerabilidades termine
   while int(zap.ascan.status()) < 100:
       print(f"Escaneo en progreso: {zap.ascan.status()}%")
       time.sleep(5)

   # Mostrar las vulnerabilidades encontradas
   vulnerabilities = zap.core.alerts(baseurl=target)
   for vuln in vulnerabilities:
       print(f"Vulnerabilidad encontrada: {vuln['name']} en {vuln['url']}")

   print("Escaneo completo.")
   ```

### 2. SQLMap desde tu Local

Puedes ejecutar **SQLMap** en tu máquina local para escanear directamente las URLs de la aplicación remota. 

##### Ejemplo:
Supongamos que tienes una URL como `http://test.pz.cl/index.php?fc=module&module=pz_bluex_features&controller=handlepickuppoints&id_cart=12345`.

Puedes ejecutar SQLMap desde tu local con el siguiente comando:

```bash
python sqlmap.py -u "http://test.pz.cl/index.php?fc=module&module=pz_bluex_features&controller=handlepickuppoints&id_cart=12345" --dbs
```

Esto funcionará desde tu local siempre que el sitio sea accesible desde Internet y no esté bloqueado por reglas de firewall.

### 3. Wapiti desde tu Local

**Wapiti** también puede ejecutarse desde tu máquina local para escanear una aplicación web remota.

Ejecuta Wapiti en la URL remota:

```bash
wapiti http://test.pz.cl
```

Wapiti recorrerá todas las URLs y buscará vulnerabilidades en la aplicación remota desde tu máquina local.

---

### Resumen

- **Sí**, puedes usar herramientas como **OWASP ZAP**, **SQLMap** y **Wapiti** desde tu máquina local para escanear aplicaciones web remotas.
- Estas herramientas funcionan localmente siempre que puedas acceder al sitio a través de Internet.
- Asegúrate de que no haya bloqueos de red o firewall que impidan el acceso desde tu ubicación a la aplicación remota.
