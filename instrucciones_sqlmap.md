
# Instrucciones para usar SQLMap

## Instalación de SQLMap
SQLMap es una herramienta de código abierto para detectar y explotar vulnerabilidades de inyección SQL. A continuación se detallan los pasos para instalar y utilizar SQLMap.

### Requisitos previos:
1. Tener Python instalado.
2. Clonar el repositorio de SQLMap desde GitHub:
    ```bash
    git clone https://github.com/sqlmapproject/sqlmap.git
    ```

### Verificar instalación:
Navega a la carpeta de SQLMap y ejecuta el siguiente comando para verificar la instalación:
   ```bash
   cd sqlmap
   python sqlmap.py --help
   ```

---

## Uso de SQLMap

### 1. Auditoría con Parámetros GET
Para auditar una URL que contiene parámetros en la cadena de consulta (GET), usa el siguiente formato:

```bash
python sqlmap.py -u "http://tusitio.com/pagina.php?id=123" --dbs
```
Donde:
- `-u` especifica la URL con los parámetros GET.
- `--dbs` le dice a SQLMap que intente listar las bases de datos disponibles.

### 2. Auditoría con Parámetros POST
Si la página web envía datos mediante un formulario POST, puedes especificar los parámetros de la siguiente manera:

```bash
python sqlmap.py -u "http://tusitio.com/pagina.php" --data="param1=value1&param2=value2" --dbs
```
Donde:
- `--data` especifica los parámetros POST que SQLMap usará para intentar detectar vulnerabilidades.

### Ejemplos adicionales:
- Obtener información del usuario actual de la base de datos:
    ```bash
    python sqlmap.py -u "http://tusitio.com/pagina.php?id=123" --current-user
    ```
- Listar las tablas dentro de una base de datos específica:
    ```bash
    python sqlmap.py -u "http://tusitio.com/pagina.php?id=123" -D nombre_de_la_bd --tables
    ```
- Extraer datos de una tabla específica:
    ```bash
    python sqlmap.py -u "http://tusitio.com/pagina.php?id=123" -D nombre_de_la_bd -T nombre_de_la_tabla --dump
    ```

---

## Consideraciones de seguridad
- **Entorno de pruebas**: Realiza las pruebas solo en entornos controlados para evitar dañar sistemas de producción.
- **Permisos**: Asegúrate de tener permisos para realizar auditorías en los sistemas que estás evaluando.
- **Validación continua**: Después de encontrar vulnerabilidades, implementa consultas preparadas y valida los inputs del usuario para mitigar ataques SQL Injection.

