# NGINX – Reverse Proxy, Load Balancer e Gateway

Este repositório demonstra, de forma **didática e prática**, como utilizar o **NGINX em containers Docker**, usando **docker-compose**, para implementar:

- 🔁 **Proxy Reverso**
- ⚖️ **Load Balancer**
- 🚪 **API Gateway simples**

O foco é servir como material de estudo para arquiteturas comuns em **Java, microserviços e aplicações web**.

---

## 📐 Arquitetura

```
Cliente
   ↓
Docker Host
   ↓
NGINX Container
(80 / 81 / 82 / 83)
   ↓
Serviço 1 (porta 81)
Serviço 2 (porta 82)
```

As portas internas do container são expostas no host da seguinte forma:

| Host | Container |
|-----|-----------|
| 8080 | 80 |
| 8081 | 81 |
| 8082 | 82 |
| 8083 | 83 |

---

## 📁 Estrutura do Projeto

```
.
├── docker-compose.yml
├── nginx_conf/
│   ├── nginx.conf
│   └── conf.d/
│       ├── default.conf
│       └── load-balancer.conf
├── nginx_html/
├── nginx_log/
├── nginx_tmp/
└── README.md
```

---

## 🐳 docker-compose.yml

```yaml
services:
  nginx:
    image: nginx:1.29.4-alpine-slim
    container_name: nginx
    ports:
      - "8080:80"
      - "8081:81"
      - "8082:82"
      - "8083:83"
    volumes:
      - ./nginx_tmp:/tmp
      - ./nginx_conf:/etc/nginx
      - ./nginx_html/:/usr/share/nginx/html
      - ./nginx_log/:/usr/share/nginx/logs
```

---

## ⚙️ nginx.conf

Arquivo principal do NGINX responsável por:

- Processos de trabalho
- Logs
- Configuração HTTP
- Inclusão dos arquivos da pasta conf.d

---

## 🔁 Proxy Reverso e Gateway (default.conf)

| URL (Host) | Destino |
|-----------|---------|
| /servico1 | localhost:81 |
| /servico2 | localhost:82 |

Acesso externo:

```
http://localhost:8080/servico1
http://localhost:8080/servico2
```

---

## ⚖️ Load Balancer (load-balancer.conf)

- Porta externa: **8083**
- Serviços balanceados: **81 e 82**
- Estratégia: **round-robin**

```
http://localhost:8083
```

---

## ▶️ Como subir o ambiente

```
docker-compose up -d
```

### Testes

```
curl http://localhost:8080/servico1
curl http://localhost:8080/servico2
curl http://localhost:8083
```

---

## 🔄 Recarregar configurações

```
docker exec nginx nginx -s reload
```

---

## 🧠 Casos de Uso

- Aplicações Java (Spring Boot, Tomcat, WildFly)
- Microserviços
- API Gateway
- Load Balancer
- Ambientes de desenvolvimento
