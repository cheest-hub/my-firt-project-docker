# Atividade Docker + CI — NELSON CONCHO

> Preencha todos os campos marcados com `[...]` e substitua os prints de exemplo pelos seus. Salve as imagens em `docs/imagens/` e mantenha os nomes de arquivo indicados.

**Aluno(a):** NELSON ENRIQUE CONCHO ACOSTA  
**Turma:** ITEAM NOITE  
**Data:** 24/07/2026  
**Aplicação usada:** `docker/getting-started-app` — To-Do em Node.js

---

## 1. Como executar este projeto

```bash
git clone https://github.com/cheest-hub/my-firt-project-docker.git
cd 
cp .env.example .env
docker compose up -d --build
```

**Acesse:** http://localhost:3000

**Para derrubar:**  
`docker compose down` (mantém dados) ou `docker compose down -v` (apaga dados).

![Texto alternativo](docs/images/Captura de tela 2026-07-24 204709.png)

---

## 2. Imagem e Dockerfile multi-stage

- **Estágios utilizados:** builder (instala dependências) e estágio final (runtime enxuto)]
- **Imagem base:** `node:20-alpine` (nos dois estágios)
- **Usuário de execução:** node, não-root
- **Tamanho final da imagem:** 68,4MB

**Por que o multi-stage ajuda?**  
Dockerfile com dois estágios build pra diminuir o tamanho da imagem, diminuir a superfície de ataque.

**Print 1 — build + docker images**  
![Print 1](docs/imagens/print1.png)

**Print 2 — aplicação rodando com tarefas cadastradas**  
![Print 2](docs/imagens/print2.png)

---

## 3. Volumes e persistência

- **Volume usado:** `[mysql-data]` → montado em `[/var/lib/mysql]`

**Print 3 — SEM volume: dados perdidos ao recriar o container**  
![Print 3](docs/imagens/print3.png)

**Print 4 — COM volume: dados preservados**  
![Print 4](docs/imagens/print4.png)

**Diferença entre `docker compose down` e `docker compose down -v`:**  
`docker compose down` mantém os dados e `docker compose down -v` limpa os volumes completamente.

---

## 4. Rede

- **Rede criada:** todo-net
- **Serviços conectados:** app e db
- **A porta do banco está exposta ao host?** Não, a comunicação é pelo nome do host.
- **Por que o app consegue chamar o host mysql / db sem saber o IP?** Todo-net.

**Print 5 — docker network inspect**  
![Print 5](docs/imagens/print5.png)

**Print 6 — dados dentro do MySQL (`select * from todo_items;`)**  
![Print 6](docs/imagens/print6.png)

---

## 5. Docker Compose

- **Serviços:** [app, db]
- **Rede:** [nome]
- **Volume:** [nome]
- **Healthcheck em:** [db]
- **depends_on com:** `[condition: service_healthy]`
- **Variáveis sensíveis:** carregadas via `.env` (não versionado). Modelo em `.env.example`.

**Print 7 — docker compose ps**  
![Print 7](docs/imagens/print7.png)

---

## 6. Integração Contínua (GitHub Actions)

- **Arquivo do workflow:** `.github/workflows/ci.yml`
- **Gatilhos:** [push e pull_request]

**O que o pipeline faz:**
1. [valida o compose]
2. [builda a imagem]
3. [sobe a stack]
4. [aguarda a app responder e testa criar uma tarefa via API]
5. [derruba a stack]

---

## 7. Quebra proposital do CI

- **O que eu quebrei:** [descreva a alteração exata que você fez]
- **Erro que apareceu no log:** [cole a mensagem principal]
- **Como o CI reagiu:** [em qual step falhou e por quê]
- **Como eu corrigi:** [o que foi alterado]
- **Link do Pull Request:** [URL]

---

## 8. Dificuldades e aprendizados

[3 a 5 linhas: o que travou, como resolveu, o que ficou mais claro sobre containers depois da atividade]

---

## 9. Checklist de autoavaliação

- [ ] Dockerfile multi-stage funcionando
- [ ] `.dockerignore` presente
- [ ] Container não roda como root
- [ ] Volume nomeado + persistência demonstrada
- [ ] Rede nomeada + banco não exposto ao host
- [ ] `compose.yaml` sobe tudo com um comando
- [ ] `.env` no `.gitignore` e `.env.example` versionado
- [ ] CI verde
- [ ] PR com CI vermelho documentado
- [ ] Todos os 9 prints no README
