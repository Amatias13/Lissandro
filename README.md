# 🎵 Lissandro

Bot de música para Discord com dashboard web no GitHub Pages.

## Estrutura

```
discord-music-bot/
├── bot.py              # Bot Discord + API REST (corre no teu PC/VPS)
├── requirements.txt    # Dependências Python
├── .env.example        # Template das variáveis de ambiente
└── index.html          # Dashboard (vai para o GitHub Pages)
```

---

## 1. Configurar o Bot (bot.py)

### Pré-requisitos

- Python 3.8+
- FFmpeg instalado no sistema

**Instalar FFmpeg:**

```bash
# Windows
winget install Gyan.FFmpeg

# Mac
brew install ffmpeg

# Ubuntu/Debian
sudo apt install ffmpeg
```

**Instalar dependências Python:**

```bash
pip install -r requirements.txt
```

### Configurar variáveis de ambiente

Copia o ficheiro de exemplo:

```bash
cp .env.example .env
```

Edita o `.env`:

```env
DISCORD_TOKEN=o_teu_token_do_discord_aqui
API_SECRET=escolhe-uma-chave-secreta-unica
```

> ⚠️ **Nunca partilhes o `.env` nem o commites para o GitHub!**  
> Adiciona `.env` ao `.gitignore`.

### Correr o bot

```bash
python bot.py
```

Verás no terminal:

```
✅ Bot ligado: NomeDoBot#1234
🌐 API a correr em http://localhost:5000
🔑 API_SECRET: a-tua-chave
```

---

## 2. Expor a API à internet (obrigatório para o GitHub Pages)

O GitHub Pages é um site externo — precisas que a tua API seja acessível pela internet.

### Opção A — ngrok (mais fácil, para testes)

```bash
# Instalar ngrok: https://ngrok.com/download
ngrok http 5000
```

O ngrok dá-te um URL tipo `https://abc123.ngrok-free.app`.  
Usa esse URL na dashboard.

### Opção B — VPS (Railway, Fly.io, DigitalOcean)

Faz deploy do `bot.py` numa VPS. O URL será o IP/domínio público da VPS na porta 5000.

### Opção C — Router (IP fixo em casa)

Faz port forwarding da porta 5000 no teu router para o teu PC.  
Usa o teu IP público como URL.

---

## 3. Publicar a Dashboard no GitHub Pages

1. Cria um repositório no GitHub (pode ser privado)
2. Faz upload do `index.html` para a raiz do repositório
3. Vai a **Settings → Pages → Branch: main → Save**
4. A dashboard fica em: `https://teu-user.github.io/nome-do-repo`

> O `index.html` não contém tokens nem chaves — é seguro ser público.

---

## 4. Usar a Dashboard

1. Abre o teu URL do GitHub Pages
2. Insere o URL da API (ex: `https://abc123.ngrok-free.app`)
3. Insere a `API_SECRET` que definiste no `.env`
4. Clica **Entrar**

As credenciais ficam guardadas no `localStorage` do browser — não precisas de inserir de novo.

---

## Comandos do Bot (via Discord)

| Comando              | Alias  | Descrição               |
| -------------------- | ------ | ----------------------- |
| `!play [música/URL]` | `!p`   | Toca ou adiciona à fila |
| `!skip`              | `!s`   | Salta para a próxima    |
| `!pause`             | —      | Pausa a música          |
| `!resume`            | —      | Continua após pausa     |
| `!stop`              | —      | Para e desliga do canal |
| `!queue`             | `!q`   | Mostra a fila           |
| `!volume [0-200]`    | `!vol` | Ajusta o volume         |

O prefixo pode ser alterado na dashboard em **Definições**.

---

## Funcionalidades da Dashboard

- ▶ Controlo completo: play/pause/skip/stop
- 🎵 Fila interativa: reordenar e remover músicas
- 📜 Histórico das últimas 50 músicas com replay
- ⚙ Definições: prefixo e volume
- 🔒 Autenticação por chave secreta
- 💾 Login guardado no browser

---

## Segurança

- A `API_SECRET` protege todos os endpoints da API
- O bot aceita pedidos de qualquer origem (CORS aberto) — necessário para o GitHub Pages
- **Não partilhes a tua `API_SECRET` nem o teu `DISCORD_TOKEN`**
- Para mais segurança, podes restringir o CORS ao domínio do teu GitHub Pages em `bot.py`:
  ```python
  CORS(app, origins=["https://teu-user.github.io"])
  ```

---

## Problemas comuns

**"Não foi possível ligar"** na dashboard  
→ Verifica se o bot está a correr e se o URL da API está correto (com `http://` ou `https://`)

**Sem som no Discord**  
→ Confirma que o FFmpeg está instalado: `ffmpeg -version`

**Erro de CORS**  
→ Certifica-te que tens `flask-cors` instalado e que o `CORS(app)` está no `bot.py`

**Bot não entra no canal de voz via dashboard**  
→ Entra tu primeiro num canal de voz — o bot liga-se automaticamente ao canal onde há membros
