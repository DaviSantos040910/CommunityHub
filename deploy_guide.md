# 🚀 Guia de Deploy — AnimeSocial no Render + Cloudinary

Este guia contém o passo a passo completo para colocar o seu projeto Django "AnimeSocial" no ar utilizando o Render (hospedagem e banco de dados PostgreSQL) e o Cloudinary (para armazenar avatares e imagens de posts).

---

## ✅ O que já foi feito no código (Pronto!)
Eu já fiz todas as configurações de código necessárias no seu projeto:
- **`settings.py`**: Configurado para usar variáveis de ambiente, banco de dados PostgreSQL (`dj-database-url`), Whitenoise para arquivos estáticos e Cloudinary para mídia.
- **`models.py` e `views.py`**: Substituído o `ImageField` por `CloudinaryField` e adaptado a remoção de imagens.
- **`build.sh`**: Criado um script que o Render usará para instalar pacotes, coletar estáticos e rodar migrações automaticamente.
- **`requirements.txt`**: Gerado com todas as bibliotecas necessárias para produção.
- **Ambiente Virtual e Migrations**: Criado o `venv` e geradas as migrações necessárias para a mudança de formato das imagens.

---

## 🛠️ O que VOCÊ precisa fazer agora (Siga na ordem)

### Passo 1: Obter as chaves do Cloudinary

O Render reinicia a máquina de tempos em tempos. Se as imagens ficassem salvas apenas lá, elas seriam apagadas. Por isso usaremos o Cloudinary.

1. Acesse **[cloudinary.com](https://cloudinary.com/)** e crie uma conta gratuita.
2. Após criar a conta, faça login e vá para o **Dashboard (Painel Inicial)** ou vá na seção de **API Keys**.
3. Anote em um bloco de notas os três valores abaixo (vamos usar no Passo 5):
   - **Cloud Name** (ex: `dxyz1abc2`)
   - **API Key** (ex: `123456789012345`)
   - **API Secret** (ex: `AbCdEfGhIjKlMnOpQrStUv`)

---

### Passo 2: Subir o código para o GitHub

Tudo que eu fiz precisa ir para o seu repositório no GitHub para que o Render consiga acessar.

Abra o terminal na pasta do seu projeto (`c:\Users\davis\anime-social`) e rode os comandos abaixo exatamente nesta ordem:

```bash
git add .
git commit -m "Preparacao final para deploy no Render com Cloudinary"
# Este comando garante que o Render possa executar o script de build:
git update-index --chmod=+x build.sh
git push origin main
```

---

### Passo 3: Criar Conta no Render (Grátis)

1. Acesse **[render.com](https://render.com/)** e crie uma conta. 
2. A maneira mais fácil é fazer login diretamente com a sua conta do **GitHub**.

---

### Passo 4: Criar o Banco de Dados (PostgreSQL)

Como o SQLite que usamos localmente não funciona bem em rede para produção, usaremos um banco PostgreSQL de verdade, providenciado de graça pelo Render.

1. No Dashboard (Painel) do Render, clique no botão superior **"New +"** e escolha **"PostgreSQL"**.
2. Preencha as informações:
   - **Name**: `animesocial-db`
   - **Region**: Escolha a mais próxima (ex: `Oregon (US West)` ou alguma no leste dos EUA).
   - **PostgreSQL Version**: `16` (ou deixe o padrão).
   - **Instance Type**: Escolha o plano `Free`.
3. Clique em **"Create Database"** no final da página.
4. Na tela que abrir, role a página para baixo até as informações de conexão e **copie o valor da "Internal Database URL"**. 
   - *A URL vai ser algo parecido com: `postgres://usuario:senha@host-interno/animesocial_db`*
5. Salve essa URL junto com as chaves do Cloudinary, logo usaremos ela.

---

### Passo 5: Criar o Web Service (Onde seu site roda)

1. Volte ao Dashboard inicial do Render.
2. Clique novamente em **"New +"** e agora escolha **"Web Service"**.
3. Conecte sua conta do GitHub e selecione o repositório do projeto (o `anime-social`). Se o repositório não aparecer, você pode precisar ajustar as permissões do GitHub para permitir que o Render veja esse repositório específico.
4. Preencha a configuração do serviço:
   - **Name**: `animesocial` (ou o nome que preferir para url, ex: `animesocial-seu-nome`).
   - **Region**: A *mesma* que você escolheu para o banco de dados.
   - **Branch**: `main` (ou master, dependendo do que você está usando).
   - **Runtime**: `Python 3`
   - **Build Command**: `./build.sh`
   - **Start Command**: `gunicorn animesocial.wsgi:application`
   - **Instance Type**: Selecione o plano `Free`.

5. **NÃO CLIQUE AINDA EM CREATE**. Role até a seção "**Environment Variables**" (Variáveis de Ambiente). CLIQUE em "Add Environment Variable" para adicionar as 7 variáveis listadas abaixo. Isto é a parte **mais importante**.

| Key (Nome da Variável) | Value (Valor) |
| :--- | :--- |
| `PYTHON_VERSION` | `3.13.7` (A versão do python instalada na sua máquina local) |
| `DATABASE_URL` | A ***Internal Database URL*** que você copiou no Passo 4. |
| `SECRET_KEY` | Uma senha enorme, como: `chave-segura-animesocial-1548@#$#@134654!!!*&` (Não pode ter espaços). |
| `DEBUG` | `False` |
| `CLOUDINARY_CLOUD_NAME` | O **Cloud Name** do Cloudinary anotado no passo 1. |
| `CLOUDINARY_API_KEY` | A **API Key** do Cloudinary do passo 1. |
| `CLOUDINARY_API_SECRET` | O **API Secret** do Cloudinary do passo 1. |

6. Após preencher as 7 variáveis, clique no botão azul **"Create Web Service"**.

---

### Passo 6: Acompanhar o Deploy e Acessar o Site

Agora o Render vai começar a instalar os pacotes do `requirements.txt`, coletar seus arquivos CSS/JS com o comando do `build.sh` e rodar as migrações (criar as tabelas no PostgreSQL online).

Isso leva cerca de 3 a 5 minutos. Você pode acompanhar a tela cheia de códigos (os "logs").
Quando a mensagem aparecer: 
`[INFO] Listening at: http://0.0.0.0:10000` (Ou algo muito parecido com "Build successful" / "Your service is live").

Aí basta olhar no canto superior esquerdo da tela onde aparece a **URL do seu site** (ex: `https://animesocial-abcd.onrender.com`). Clique nela e seu site estará no ar! 🎉

---

### Passo 7: Criar a Conta de Administrador (Superusuário) no Site Pronto

Como o banco PostgreSQL é zerado, você precisa criar uma nova conta de administrador. 

1. Na mesma página do seu Web Service no Render, olhe o menu lateral e clique em "**Shell**".
2. Um terminal preto online vai abrir. Digite o seguinte comando e aperte Enter:
   ```bash
   python manage.py createsuperuser
   ```
3. O terminal vai pedir um `Username`, um `Email`, uma `Password` e pedir para confirmar a `Password` (A senha fica invisível quando você digita, isso é normal).
4. Feito isso, navegue no seu navegador para `https://sua-url-do-render.onrender.com/admin/`.
5. Faça login. A partir daí você pode gerenciar usuários, posts, páginas estáticas ou o que quiser!

### 🔄 Como atualizar o site depois?

O Render é integrado ao GitHub. Toda vez que você estiver no VS Code, alterar um texto, arquivo ou HTML e mandar para o Github com `git add .`, `git commit -m "minhas alteracoes"` e `git push`, o Render detecta a atualização e muda o site online automaticamente!
