# NGINX – Reverse Proxy, Load Balancer e Gateway

Este repositório demonstra, de forma **didática e prática**, como utilizar o **NGINX** para implementar:

- 🔁 **Proxy Reverso**
- ⚖️ **Load Balancer**
- 🚪 **API Gateway simples**

O objetivo é servir como material de estudo e referência para arquiteturas comuns em ambientes **Java, microserviços e aplicações web**.

---

## 📐 Arquitetura

```
Cliente
   ↓
NGINX (80 / 83)
   ↓
Serviço 1 (localhost:81)
Serviço 2 (localhost:82)
```

---

## 📁 Estrutura do Projeto

```
.
├── nginx.conf
├── conf.d/
│   ├── default.conf
│   └── load-balancer.conf
└── README.md
```

---

## ⚙️ Arquivo nginx.conf

Arquivo principal do NGINX responsável por:

- Processos de trabalho
- Logs
- Configuração HTTP
- Inclusão dos arquivos da pasta conf.d

---

## 🔁 Proxy Reverso e Gateway (default.conf)

O NGINX atua como **gateway**, roteando as requisições conforme o contexto da URL.

| URL | Destino |
|----|--------|
| /servico1 | localhost:81 |
| /servico2 | localhost:82 |

---

## ⚖️ Load Balancer (load-balancer.conf)

Distribui requisições entre múltiplos serviços usando **round-robin**.

- Porta de acesso: **83**
- Serviços balanceados: **81 e 82**
- Preserva IP do cliente via `X-Real-IP`

---

## ▶️ Como testar

Suba dois serviços simples:

```bash
python3 -m http.server 81
python3 -m http.server 82
```

Inicie o NGINX:

```bash
nginx -s reload
```

Testes:

```bash
curl http://localhost/servico1
curl http://localhost/servico2
curl http://localhost:83
```

---

## 🧠 Casos de Uso

- Aplicações Java (Spring Boot, Tomcat, WildFly)
- Microserviços
- API Gateway
- Balanceamento de carga
- Segurança e performance

---

## 👤 Autor

Franklin  
Estudos práticos com NGINX e arquitetura de software.

