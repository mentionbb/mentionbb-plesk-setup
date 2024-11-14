# Installation instructions for Plesk panel

<p align="center">
    <picture>
        <source media="(prefers-color-scheme: dark)"
            srcset="https://raw.githubusercontent.com/mentionbb/mentionbb/master/public/ui/images/logo-nightmode.svg">
        <source media="(prefers-color-scheme: light)"
            srcset="https://raw.githubusercontent.com/mentionbb/mentionbb/master/public/ui/images/logo.svg">
        <img alt="Mention logo" src="https://raw.githubusercontent.com/mentionbb/mentionbb/master/public/ui/images/logo.svg"
            width="500px">
    </picture>
</p>

---

<p align="center">How to install MentionBB with Git on Plesk panel and Nginx settings.</p>

## Hosting settings

- Document Root should be `httpdocs/public`. Mention uses `public` path as the main directory, if you specify root directory there are sensitive files in this section. It should be `public`.

![resim](https://github.com/user-attachments/assets/19a25b1d-b760-486f-80a1-e9358b239022)

## PHP settings

* You can choose the latest PHP version. MentionBB takes a very sensitive approach in this regard, we build according to the latest technologies.
* `FPM application served by Nginx` should be selected for Nginx.

![resim](https://github.com/user-attachments/assets/6762aacb-0bb1-4a26-96eb-0993fd75b08b)

## Apache & Nginx settings

- Disabled `Proxy Mode`

![resim](https://github.com/user-attachments/assets/0d8229ec-eef5-435e-9d33-57bec521465d)

### Additional nginx directives
```text
location / {
	try_files $uri /index.php$is_args$args;
}

location ~ ^/index\.php(/|$) {
	if ($request_method = 'OPTIONS') {
		add_header 'Access-Control-Allow-Origin' '*' always;
		add_header 'Access-Control-Allow-Methods' 'GET, POST, DELETE, OPTIONS' always;
		add_header 'Access-Control-Allow-Headers' 'Authorization,DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range' always;
		add_header 'Access-Control-Max-Age' 1728000 always;
		add_header 'Content-Type' 'text/plain; charset=utf-8' always;
		add_header 'Content-Length' 0 always;
		return 204;
	}
	add_header 'Access-Control-Allow-Origin' '*' always;
	add_header 'Access-Control-Allow-Methods' 'GET, POST, DELETE, OPTIONS' always;
	add_header 'Access-Control-Allow-Headers' 'Authorization,DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range' always;
	add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range' always;

	fastcgi_pass unix:/run/php/php8.3-fpm.sock;
	fastcgi_split_path_info ^(.+\.php)(/.*)$;
	include fastcgi_params;
	fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
	fastcgi_param DOCUMENT_ROOT $realpath_root;
	internal;
}

location ~ \.php$ {
	return 404;
}
location = /favicon.ico {
	log_not_found off;
	access_log off;
}

include /etc/nginx/mime.types;
include /var/www/vhosts/mentionbb.com/httpdocs/.nginx.conf;
```

You need to edit the code in the last line.

`include /var/www/vhosts/mentionbb.com/httpdocs/.nginx.conf;`

You should edit the `mentionbb.com` part according to your own virtual host path.

## Git

![resim](https://github.com/user-attachments/assets/59926859-d744-4f28-88b4-f632086d7430)

* Repository name is `your choice`
* Repo Url is `https://github.com/mentionbb/mentionbb.git`

After this step, you should import the `db.sql` file in the root directory and write the information in `src/DbConfig.php`.
