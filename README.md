# k8-project
Deploy multi container app

### Architecture

```                    Browser
                       │
                       │ localhost:8080
                       ▼
      kubectl port-forward service/wordpress 8080:80
                       │
                       ▼
                WordPress Service
                       │
                  selects app=wordpress
                       │
                       ▼
               WordPress Pod
         ┌───────────────────────────┐
         │ WordPress Container       │
         │                           │
         │ Environment Variables     │
         │   WORDPRESS_DB_HOST       │
         │   WORDPRESS_DB_USER       │
         │   WORDPRESS_DB_PASSWORD◄── Secret
         │   WORDPRESS_DB_NAME       │
         └─────────────┬─────────────┘
                       │
                mysql:3306
                       │
                       ▼
                  MySQL Service
                       │
                  selects app=mysql
                       │
                       ▼
                 MySQL Pod
         ┌───────────────────────────┐
         │ MySQL Container           │
         │                           │
         │ MYSQL_ROOT_PASSWORD ◄──── Secret
         │ MYSQL_DATABASE            │
         │ MYSQL_USER               │
         │ MYSQL_PASSWORD ◄───────── Secret
         │                           │
         │ /var/lib/mysql            │
         │        ▲                  │
         └────────┼──────────────────┘
                  │
            PersistentVolumeClaim
             mysql-pvc (RWO)
```