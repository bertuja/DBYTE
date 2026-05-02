# 🧾 README - Metabase + NGINX + Certbot

Este entorno Docker permite ejecutar Metabase detrás de un proxy NGINX con SSL automático mediante Certbot (Let's Encrypt).

---

## 📁 Estructura de archivos esperada

```
metabase-ssl/
├── docker-compose.yml
├── default.conf
├── certbot/
│   └── www/         # Usado para validación HTTP-01
```

---

## 🚀 Paso a paso

### 1. Configurar los archivos necesarios

* `docker-compose.yml`: contiene los servicios de Metabase, NGINX y Certbot.
* `default.conf`: contiene la configuración de NGINX con redirección HTTPS y proxy.

Asegurarse de que en `default.conf` esté presente este bloque al inicio:

```nginx
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}
```

### 2. Levantar Metabase y NGINX (sin certbot todavía)

```bash
docker-compose up -d metabase nginx
```

### 3. Generar certificados SSL con Certbot

```bash
docker-compose run --rm certbot
```

Verificá que los certificados estén en:

```
/etc/letsencrypt/live/metabase.webpack.com.ar/
```

### 4. Reiniciar NGINX para aplicar SSL

```bash
docker-compose restart nginx
```

### 5. Verificar funcionamiento

```bash
curl -kI https://localhost
```

O acceder desde el navegador:

```
https://metabase.webpack.com.ar
```

---

## 🔄 Renovación automática del certificado

Editar el crontab del host:

```bash
sudo crontab -e
```

Y agregar:

```bash
0 3 * * * docker-compose -f /home/ec2-user/metabase-ssl/docker-compose.yml run --rm certbot renew && docker-compose -f /home/ec2-user/metabase-ssl/docker-compose.yml exec nginx nginx -s reload
```

Esto intentará renovar el certificado todos los días a las 3:00am y reiniciará NGINX si hay cambios.

---

## ✅ Resultado final

* Metabase disponible en HTTPS
* Certificados SSL automáticos y renovables
* NGINX funcionando como proxy con compresión y timeout optimizados

---

📌 Si querés mover esto a otra instancia, podés copiar la carpeta completa y ejecutar desde allí.
