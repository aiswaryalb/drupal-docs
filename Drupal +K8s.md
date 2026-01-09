# Kubernetes Drupal + MySQL Lab

This guide summarizes the steps to deploy a Drupal project with MySQL on Minikube using Docker images and Kubernetes.

---

## 1. Create Drupal Project
```
composer create-project drupal/recommended-project k8s-test
cd k8s-test
```
## 2. Added the Dockerfile
docker/nginx/nginx.conf
```
server {
    listen 80;
    server_name localhost;
    root /var/www/html/web;

    index index.php index.html index.htm;

    location / {
        try_files $uri /index.php?$query_string;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        try_files $uri @rewrite;
    }

    location @rewrite {
        rewrite ^/(.*)$ /index.php?q=$1;
    }
}
```

k8s-test/docker/scripts/entrypoint.sh
```
#!/bin/sh
set -e

# Wait for DB to be ready
echo "Waiting for database..."
until nc -z "$DB_HOST" 3306; do
  sleep 2
done
echo "Database is ready!"

# Start PHP-FPM in the background
php-fpm &

# Start Nginx in the foreground
nginx -g "daemon off;"
```

Dockerfile
```
FROM php:8.3-fpm

# -------------------------------
# Working directory
# -------------------------------
WORKDIR /var/www/html

# -------------------------------
# System dependencies + PHP extensions
# -------------------------------
RUN apt-get update && apt-get install -y \
    nginx \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libzip-dev \
    libpq-dev \
    libmagickwand-dev \
    unzip git curl netcat-openbsd \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install pdo pdo_mysql pdo_pgsql gd zip opcache \
 && pecl install imagick \
 && docker-php-ext-enable imagick \
 && rm -rf /var/lib/apt/lists/*

# -------------------------------
# Composer
# -------------------------------
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# -------------------------------
# Install PHP dependencies first (layer caching)
# -------------------------------
COPY composer.json composer.lock ./
RUN composer install --no-dev --prefer-dist --optimize-autoloader --no-interaction

# -------------------------------
# Copy Drupal code
# -------------------------------
COPY . .

# -------------------------------
# Nginx config
# -------------------------------
RUN rm -f /etc/nginx/sites-enabled/default
COPY docker/nginx/nginx.conf /etc/nginx/sites-available/default
RUN ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default

# -------------------------------
# Permissions
# -------------------------------
RUN chown -R www-data:www-data /var/www/html \
 && chmod -R 755 /var/www/html

# -------------------------------
# Entrypoint
# -------------------------------
COPY docker/scripts/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

EXPOSE 80 9000
ENTRYPOINT ["/entrypoint.sh"]
```

## 3. Build and Push Docker Image

```
docker build -t drupal-k8s:latest
docker tag drupal-k8s:latest aiswaryabpillai/drupal-k8s:latest
docker login -u <username>
docker push  <username>/drupal-k8s:latest
```

## 4. MySQL Secret

```
kubectl create secret generic mysql-secret --from-literal=password=drupal
```

```
kubectl get secrets
kubectl describe secret mysql-secret
kubectl get secret mysql-secret -o jsonpath="{.data.password}" | base64 --decode
```

## 5. Drupal Deployment
k8s/drupal-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal
spec:
  selector:
    matchLabels:
      app: drupal
  replicas: 1
  template:
    metadata:
      labels:
        app: drupal
    spec:
      containers:
        - name: drupal
          image: aiswaryabpillai/drupal-k8s:latest
          ports:
            - containerPort: 80
          env:
            - name: DB_HOST
              value: mysql
            - name: DB_NAME
              value: drupal
            - name: DB_USER
              value: drupal
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: password
---
apiVersion: v1
kind: Service
metadata:
  name: drupal
spec:
  selector:
    app: drupal
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort

```
## 6. MySQL Deployment
k8s/mysql-deployment.yaml
```
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
stringData:
  password: drupal
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  replicas: 1
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: password
            - name: MYSQL_DATABASE
              value: drupal
            - name: MYSQL_USER
              value: drupal
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: password
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-data
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-data
          persistentVolumeClaim:
            claimName: mysql-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  selector:
    app: mysql
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306

```

## 7. Apply Deployments
```
kubectl get pods
kubectl get svc

kubectl apply -f k8s/mysql-deployment.yaml
kubectl apply -f k8s/drupal-deployment.yaml

kubectl get pods
kubectl get svc
kubectl get svc drupal
```

## 8. Drupal Installation Page

Use these details:

  - DB Name: drupal
  - DB User: drupal
  - DB Password: from mysql-secret (drupal in example)
  - DB Host: mysql
  - DB Port: 3306

Access the site via:
```
minikube service drupal
```

## 9. Kubernetes Basics to Test

  - Check pods
  ```
  kubectl get pods
  ```

  - Scale deployment
  ```
  kubectl scale deployment drupal --replicas=2
  ```
  
  - Delete a pod and watch auto-replacement
  ```
  kubectl delete pod drupal-<pod-name>
  kubectl get pods -w
  ```
  
  - Rolling update (change image)
  ```
  kubectl set image deployment/drupal drupal=yourdockerhubusername/drupal-k8s:latest
  kubectl rollout status deployment/drupal
  ```
  
  - MySQL failover test (Drupal will fail if DB is down)
  ```
  kubectl delete pod mysql-<pod-name>
  ```

  - Restart
  ```
  kubectl rollout restart deployment/mysql
  kubectl rollout restart deployment/drupal
  ```
