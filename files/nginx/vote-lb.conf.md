upstream backend_vote {
   server 10.128.0.44:31000; 
   server 10.168.0.2:31000;
   server 10.138.0.8:31000;
   server 10.142.0.9:31000;
}

server {
    listen 80;
    server_name vote.hebersonaguiar.me; 

    location / {
    return 301 https://vote.hebersonaguiar.me/;
   }
}

server {
   listen 443 ssl;
   server_name vote.hebersonaguiar.me; 

   ssl on;
   ssl_certificate      /etc/nginx/conf.d/certs/fullchain.pem;
   ssl_certificate_key  /etc/nginx/conf.d/certs/privkey.pem;

   location / {
      proxy_pass http://backend_vote;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }
}