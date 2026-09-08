# 📍 CadeAi

> Sistema web para busca de estabelecimentos por proximidade — lanchonetes, petshops, farmácias, supermercados e mais.

> ⚠️ **Nota:** este é um repositório de apresentação. O código-fonte é privado; aqui você encontra a documentação do projeto, decisões técnicas e demonstração visual.

---

## 🎯 Sobre o projeto

O **CadeAi** nasceu de um problema simples do dia a dia: encontrar rápido o que você precisa perto de você, sem precisar comparar três apps diferentes. O sistema permite buscar estabelecimentos por categoria e proximidade geográfica, exibindo resultados num mapa interativo com avaliações reais de outros usuários.

**Principais funcionalidades:**
- 🔍 Busca por categoria (lanchonete, petshop, farmácia, supermercado, etc.) e raio de distância
- 🗺️ Mapa interativo com marcadores customizados por categoria
- ⭐ Sistema de avaliações (nota + comentário) com cálculo de média em tempo real
- ❤️ Favoritos salvos por usuário autenticado
- 🔐 Login por e-mail/senha ou via Google (OAuth2)
- 🛡️ Rate limiting, proteção contra spam e políticas de autorização por usuário

---

## 🖼️ Demonstração


| Tela de busca | Mapa interativo | Perfil e favoritos |
|:---:|:---:|:---:|

<img width="1090" height="752" alt="Captura de Tela 2026-09-08 às 16 21 15" src="https://github.com/user-attachments/assets/25090e9e-d01f-42e7-9d98-39acc58a21ee" />
<img width="1096" height="781" alt="Captura de Tela 2026-09-08 às 16 21 03" src="https://github.com/user-attachments/assets/c66f5069-1d14-4ece-8bae-19fcc8a25a0b" />
<img width="1368" height="764" alt="Captura de Tela 2026-09-08 às 16 20 42" src="https://github.com/user-attachments/assets/48763190-2e27-4d8a-9269-13cd8c256bf8" />

---

## 🏗️ Arquitetura

```mermaid
flowchart TD
    A[Usuário] -->|Busca por categoria/local| B[Laravel App]
    B --> C[Cálculo de distância - Haversine]
    B --> D[API de Mapas]
    B --> E[(Banco de Dados)]
    B --> F[Cache - Redis]
    A -->|Login| G[Auth: Email/Senha ou Google OAuth]
    G --> B
    A -->|Avaliação/Favorito| H[Reviews & Favorites]
    H --> E
```

---

## ⚙️ Stack técnica

| Camada | Tecnologia |
|---|---|
| Backend | Laravel 11 (PHP 8.2+) |
| Banco de dados | MySQL |
| Frontend | Blade + Alpine.js |
| Autenticação | Laravel Breeze + Socialite (Google OAuth) |
| Mapas | Google Maps Platform / Leaflet |
| Cache | Redis |
| Testes | PHPUnit / Pest |

---

## 🧠 Decisões técnicas e desafios

**Busca por proximidade geográfica**
Implementei o cálculo de distância direto na query SQL usando a fórmula de Haversine, evitando trazer todos os registros pra memória e filtrar em PHP — importante pra performance conforme a base de estabelecimentos cresce.

**Segurança em avaliações**
Como qualquer usuário autenticado pode avaliar um local, precisei limitar a frequência de avaliações por usuário (rate limiting), sanitizar comentários contra XSS e garantir via Policies que cada um só edite as próprias avaliações.

**Autenticação flexível**
Além do login tradicional, integrei OAuth2 com Google via Laravel Socialite, unificando contas pelo e-mail quando o usuário já tinha cadastro prévio.

**Cache de buscas frequentes**
Buscas populares (ex: "farmácia" numa região específica) são cacheadas por poucos minutos no Redis, reduzindo carga no banco em picos de acesso.

---

## 📌 Status do projeto

🚧 Em desenvolvimento ativo — testando com um grupo reduzido de usuários antes de expandir.

---

## 📫 Contato

Ficou com alguma dúvida técnica sobre o projeto ou quer trocar uma ideia? Me chama:

- LinkedIn: www.linkedin.com/in/karlysonf
- E-mail: karlysonsantosdev@gmail.com
