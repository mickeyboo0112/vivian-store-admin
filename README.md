# Bind Node port 80
sudo apt-get install libcap2-bin
sudo setcap cap_net_bind_service=+ep `readlink -f \`which node\``

# Bind subdomain with Nginx

upstream vivianstore.tk {
  server 127.0.0.1:3001;
}
server {
  listen 0.0.0.0:80;
  server_name vivianstore.tk;
  access_log /var/log/nginx/vivianstore.access.log;
  error_log /var/log/nginx/vivianstore.error.log debug;
  location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_pass http://vivianstore.tk;
    proxy_redirect off;
  }
}

upstream cms.vivianstore.tk {
  server 127.0.0.1:3000;
}
server {
  listen 0.0.0.0:80;
  server_name cms.vivianstore.tk;
  access_log /var/log/nginx/cms.vivianstore.access.log;
  error_log /var/log/nginx/cms.vivianstore.error.log debug;
  location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_pass http://cms.vivianstore.tk;
    proxy_redirect off;
  }
}
