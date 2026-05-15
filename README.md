[Unit]
Description=Gunicorn daemon for ShopAPI
After=network.target postgresql.service

[Service]
User=shopapi
Group=www-data
WorkingDirectory=/opt/bacon-shopapi
Environment="PATH=/opt/bacon-shopapi/.venv/bin"
EnvironmentFile=/opt/bacon-shopapi/.env
ExecStart=/opt/bacon-shopapi/.venv/bin/gunicorn \
          --workers 3 \
          --bind unix:/run/gunicorn-shopapi.sock \
          --access-logfile /var/log/gunicorn-shopapi-access.log \
          --error-logfile /var/log/gunicorn-shopapi-error.log \
          config.wsgi:application
ExecReload=/bin/kill -s HUP $MAINPID
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target