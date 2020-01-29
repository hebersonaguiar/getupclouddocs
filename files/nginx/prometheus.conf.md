upstream backend_prometheus {
   server 10.128.0.44:32295; 
}

server {
    listen 80;
    server_name prometheus.hebersonaguiar.me; 

    location / {
    return 301 https://prometheus.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name prometheus.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_prometheus;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}