server {
    listen 80;
    server_name server_domain_or_IP;

    # Serve React frontend
    location / {
        root /var/www/vanakproject/frontend/dist;
        index index.html;
        try_files $uri /index.html;
    }

    # Proxy requests to Django API (Gunicorn)
    location /api/ {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn.sock;
    }

    # Serve static files correctly
    location /static/ {
        root /var/www/vanakproject/frontend/dist;
        index index.html;
    }

    # Optional: Favicon handling (to avoid error logs)
    location = /favicon.ico { access_log off; log_not_found off; }

}
