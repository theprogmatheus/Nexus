# Segurança — Lista de Verificação (Security Checklist)

Esta lista de verificação deve ser consultada obrigatoriamente por desenvolvedores e agentes antes da aprovação de qualquer Pull Request ou alteração na camada de autenticação, dados ou endpoints do **Nexus**.

---

## 1. Camada de Transporte e Rede
- [ ] O tráfego trafega exclusivamente sob HTTPS em produção.
- [ ] O cabeçalho `Strict-Transport-Security` (HSTS) está ativado (`includeSubDomains; max-age=31536000`).
- [ ] Portas desnecessárias do container não estão expostas.

## 2. Manipulação de Credenciais e Senhas
- [ ] Nenhuma senha é persistida em texto claro sob qualquer condição.
- [ ] O algoritmo de hashing utiliza salt aleatório único e fator de trabalho computacionalmente seguro (Argon2id ou BCrypt).
- [ ] As mensagens de falha de login são uniformes e não permitem adivinhar se o e-mail existe na base (prevenção contra enumeração).
- [ ] Há mecanismo de proteção contra força bruta (rate limiting e bloqueio temporário de conta).
- [ ] Tokens de redefinição de senha possuem tempo de vida curto (< 30 min), uso único e são armazenados hasheados.

## 3. Gestão de Tokens e OAuth 2.0 / OIDC
- [ ] O fluxo de Authorization Code exige obrigatoriamente PKCE com método SHA-256 (`S256`).
- [ ] As URLs de redirecionamento (`redirect_uri`) são validadas por casamento exato, sem suporte a wildcards abertos.
- [ ] Access Tokens possuem tempo de vida curto (máximo de 15 minutos).
- [ ] Refresh Tokens utilizam rotação compulsória (RTR) e invalidam a família de tokens caso reuso seja detectado.
- [ ] Tokens JWT são assinados assimetricamente (RS256/ES256) e rejeitam explicitamente `alg: none`.
- [ ] O endpoint público `/.well-known/jwks.json` expõe apenas chaves públicas, nunca dados da chave privada.

## 4. Proteções Web e Sessão
- [ ] Cookies de sessão contêm as flags `HttpOnly`, `Secure` e `SameSite=Lax` (ou `Strict`).
- [ ] O Session ID é renovado e invalidado imediatamente após autenticação primária (proteção contra Session Fixation).
- [ ] Proteção contra Cross-Site Request Forgery (CSRF) está ativa para requisições de estado originadas no navegador.
- [ ] Cabeçalhos de segurança estão presentes nas respostas HTTP (`Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`).

## 5. Código e Operações
- [ ] Nenhum segredo (senhas, chaves privadas, client secrets) foi incluído no commit do Git.
- [ ] Nenhuma credencial ou token completo é emitido nos logs da aplicação.
- [ ] O arquivo [`.gitignore`](file:///C:/Users/matheus.ferreira/Documents/Nexus/.gitignore) cobre arquivos de ambiente (`.env`) e chaves criptográficas (`*.pem`, `*.key`).
- [ ] As dependências de terceiros foram verificadas quanto a vulnerabilidades conhecidas (CVEs).
