# Root Config: Aplicativo contêiner para single-spa 5

- Baixe o projeto
  - `git clone git@github.com:natancardosodev/root-config.git`
- Instale as dependências com node 20 e npm 10
  - `npm i`
- Execute o build da aplicação
  - `npm run build`

- Coloque um dominio de testes em `sudo gedit /etc/hosts`
  - Adicione a linha: `127.0.0.1 mfe.testes.com`

- Rode `sudo gedit /etc/nginx/sites-available/default` e adicione:

```conf
server {
	listen 8080;
	listen [::]:8080;

	server_name mfe.testes.com;

	root /www/angular/mfe/root-config/dist;
	index index.html;

	location /micro-ng {
		alias /www/angular/mfe/mfe-single-spa-angular/dist;
		try_files $uri $uri/ /micro-ng/index.html;
	}

	location / {
		try_files $uri $uri/ =404;
	}
}
```

- Execute `sudo systemctl restart nginx`
- Instale um projeto compatível com single-spa, como em: https://github.com/natancardosodev/mfe-single-spa-angular
- Após ter buildado a aplicação, abra no navegador https://mfe.testes.com

> Devido a lib do navbar utilizada, a aplicação espera um projeto angular com a rota base '/micro-ng', assim como react e vue.

## Arquivos principais:
- `microfrontend-layout.html`: contém o mapeamento de rotas e projetos
- `index.ejs`: base para o index.html feito com EJS, um template engine
- `importmap.json`: mapeamento dos bundles das aplicações filhas
