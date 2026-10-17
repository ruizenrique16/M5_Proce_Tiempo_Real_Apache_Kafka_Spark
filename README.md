# Proyecto de Streaming de Datos con Kafka, Spark y Jupyter

Este proyecto configura un entorno completo para el procesamiento de datos en tiempo real utilizando **Apache Kafka**, **Apache Spark** (Master y Worker), **Zookeeper** y **Jupyter Lab** orquestados mediante **Docker Compose**.

---

## 1. Levantar los Servicios con Docker Compose

Asegúrate de tener instalado [Docker](https://docs.docker.com/get-docker/) y Docker Compose. Para iniciar la infraestructura completa en segundo plano, ejecuta el siguiente comando en la raíz del proyecto:

```bash
docker compose up -d
```

### Verificar el estado de los contenedores

Para comprobar que todos los servicios (`zookeeper`, `kafka`, `spark-master`, `spark-worker`, `jupyter`) están ejecutándose correctamente:

```bash
docker compose ps
```

---

## 2. Crear el Tópico en Apache Kafka

Sigue estos pasos para ingresar al contenedor de Kafka y crear el tópico llamado `proyect-final-topic`:

### Paso 1: Ingresar a la terminal (bash) del contenedor de Kafka

```bash
docker exec -it kafka bash
```

### Paso 2: Crear el tópico `proyect-final-topic`

Dentro de la terminal del contenedor, ejecuta:

```bash
kafka-topics --bootstrap-server kafka:9092 --create --topic proyect-final-topic --partitions 1 --replication-factor 1
```

### Paso 3: Verificar que el tópico fue creado

```bash
kafka-topics --bootstrap-server kafka:9092 --list
```

### Paso 4: Salir del contenedor

```bash
exit
```

---

## 3. Acceso a Jupyter Lab y Spark UI

Una vez iniciados los servicios, puedes ingresar a las siguientes interfaces desde tu navegador web:

* **Jupyter Lab:** [http://localhost:8888](http://localhost:8888)
* **Spark Master Web UI:** [http://localhost:8080](http://localhost:8080)
* **Spark Application UI:** [http://localhost:4040](http://localhost:4040)

---

## 4. Orden de Ejecución de los Notebooks

Abre Jupyter Lab visitando `http://localhost:8888`. Los archivos de notebooks se encuentran sincronizados dentro del directorio de trabajo.

###  Ejemplo 1: Laboratorio 01

1. **Primero:** Abre y ejecuta las celdas de `consumer_lab_01.ipynb`.  
   *Debe ejecutarse primero para quedar a la escucha de las publicaciones.*
2. **Segundo:** Abre y ejecuta las celdas de `producer_lab_01.ipynb`.  
   *Este notebook comenzará a enviar eventos hacia el tópico `proyect-final-topic`.*

---

### 🌊 Ejemplo 2: Laboratorio 02 (Streaming)

1. **Primero:** Abre y ejecuta las celdas de `consumer_lab_02_streaming.ipynb`.  
   *Inicia el proceso de Spark Structured Streaming a la espera de mensajes entrantes.*
2. **Segundo:** Abre y ejecuta las celdas de `producer_lab_02_streaming.ipynb`.  
   *Generará el flujo continuo de datos enviando mensajes a Kafka en tiempo real.*

---

## 5. Detener o Iniciar la Infraestructura

* **Detener servicios manteniendo el estado:**
  ```bash
  docker compose stop
  ```
* **Reanudar servicios detenidos:**
  ```bash
  docker compose start
  ```
* **Destruir contenedores y redes:**
  ```bash
  docker compose down
  ```
