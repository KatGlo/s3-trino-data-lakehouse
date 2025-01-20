# **S3-Trino-Data-Lakehouse Architecture** 🌉

## **Overview** 📖  
This repository provides an implementation of a modern **Data Lakehouse architecture**. It integrates **Trino** (formerly PrestoSQL) for querying data stored in S3-compatible storage, **Apache Hive Metastore** for metadata management, and **Apache Spark** for distributed data processing.  

---

## **Architecture Components** 🏗️  

### **Core Services** ⚙️  
- **PostgreSQL (13)**: Database for storing Hive Metastore metadata.  
- **Hive Metastore (3.1.3)**: Metadata service for table and schema management.  
- **Trino (426)**: High-performance distributed SQL query engine.  
- **Apache Spark (3.4.1)**: Framework for distributed data processing.  
  - **Spark Master**: Cluster manager.  
  - **Spark Worker**: Execution node.  

### **Storage** 📦  
- **S3-Compatible Object Storage**: Primary data lake storage with the following features:  
  - Path-style access.  
  - SSL/TLS enabled.  
  - Compatible with **MinIO**, **AWS S3**, and other S3-compatible storage systems.  

---

## **Port Configuration** 🔌  
| **Service**       | **Port**                  | **Details**                                |
|--------------------|---------------------------|--------------------------------------------|
| PostgreSQL         | `5432`                   | Metadata database                          |
| Hive Metastore     | `9083`                   | Metadata service                           |
| Trino              | `9090`                   | Web UI: [http://localhost:9090](http://localhost:9090) |
| Spark Master       | `7077`                   | Cluster manager                            |
| Spark Web UI       | `8080`                   | Web UI: [http://localhost:8080](http://localhost:8080) |

---

## **Prerequisites** 📋  
- **Docker** and **Docker Compose** installed.  
- Access to **S3-compatible storage** (access key and secret key).  
- Sufficient disk space for PostgreSQL data.  
- Docker network connectivity.  

---

## **Quick Start** 🚀  

1. **Clone the repository**:  
   ```bash
   git clone https://github.com/your-username/s3-trino-data-lakehouse.git
   cd s3-trino-data-lakehouse
   ```

2. **Set up environment variables**:  
   ```bash
   cp .env.example .env
   # Edit .env with your S3 credentials and endpoints
   ```

3. **Start the services**:  
   ```bash
   docker compose build
   docker compose up -d
   ```

4. **Verify the setup**:  
   ```bash
   # Check running containers
   docker ps

   # Connect to Trino
   docker exec -it trinodb trino
   ```

---

## **Usage Examples** 📊  

### **Trino Queries** 🛠️  
```sql
-- Register a Delta Lake table
CALL delta.system.register_table(
    schema_name => 'default',
    table_name => 'mytable',
    table_location => 's3a://my-bucket/path/'
);

-- Query data
SELECT * FROM delta.default.mytable LIMIT 5;
```

### **Spark Processing** ⚡  
```bash
# Connect to Spark shell
docker exec -it spark /opt/bitnami/spark/bin/spark-shell

# Process data in Scala
val df = spark.read.format("delta").load("s3a://my-bucket/path/")
df.show(5)
```

---

## **Directory Structure** 📂  
```
├── conf/                  # Configuration files
├── data/                  # PostgreSQL data directory
├── etc/
│   └── catalog/           # Trino catalog configurations
├── jars/                  # Additional JAR files
├── docker-compose.yaml    # Service definitions
├── Dockerfile             # Hive Metastore container definition
└── README.md
```

---

## **Configuration Files** 🛠️  

### **Hive Metastore**  
- `conf/hive-site.xml`: Hive configuration.  
- `entrypoint.sh`: Initialization script.  

### **Trino**  
- `etc/catalog/delta.properties`: Delta Lake connector configuration.  

### **Spark**  
- `conf/spark-defaults.conf`: Spark configuration.  

---
