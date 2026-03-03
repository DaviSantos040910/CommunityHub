# 🌸 AnimeSocial

**AnimeSocial** é uma rede social focada no compartilhamento amigável e focado de desenhos, ilustrações e fanarts de animes. Os usuários podem criar uma conta, editar seus perfis (incluindo avatares reais armazenados em nuvem), postar suas criações e interagir curtindo as obras de outros artistas da comunidade.

---

## 🚀 Tecnologias Utilizadas

Este projeto foi construído utilizando as seguintes tecnologias:

*   **Backend:** Python 3.13 e **[Django 5.2](https://www.djangoproject.com/)**
*   **Frontend:** HTML5, CSS3 (Vanilla), JavaScript e ícones do **[Lucide](https://lucide.dev/)**
*   **Banco de Dados:**
    *   `SQLite` (Ambiente de Desenvolvimento Local)
    *   `PostgreSQL` via Render (Ambiente de Produção)
*   **Armazenamento de Mídia:** **[Cloudinary](https://cloudinary.com/)** (Para armazenamento persistente e seguro de avatares e imagens dos posts)
*   **Hospedagem & Deploy:** **[Render](https://render.com/)** com `gunicorn` e `Whitenoise` para os arquivos estáticos
*   **Autenticação:** Sistema robusto nativo do Django + integração de base com `django-allauth`


---

## ✨ Principais Funcionalidades

*   🔐 **Sistema de Autenticação:** Cadastro simplificado e Login.
*   👤 **Perfis de Usuário Personalizáveis:** Adição de bio e upload de Avatar.
*   🖼️ **Feed de Postagens:** Interface agradável no padrão "Galeria" para visualização das artes criadas pelos membros.
*   ❤️ **Sistema de Curtidas (Em Tempo Real):** Feito de forma assíncrona com JavaScript (AJAX) para curtir posts sem recarregar a página.
*   🔍 **Busca Inteligente:** Motor de busca integrado com AJAX para encontrar postagens por título e usuários simultaneamente.
*   ☁️ **Upload de Mídia Moderno:** Sem mais fotos quebradas na nuvem, todas as imagens são enviadas nativamente para os servidores de nuvem do Cloudinary.

---

## 💻 Como rodar o projeto localmente (Para Desenvolvedores)

Siga estas instruções caso queira criar um clone do projeto e rodar tranquilamente na sua máquina:

### 1. Pré-requisitos
*   [Python](https://www.python.org/downloads/) 3.12 ou superior instalado.
*   [Git](https://git-scm.com/) instalado.

### 2. Passo a Passo

Clone o repositório na sua máquina:
```bash
git clone https://github.com/SeuUsuario/anime-social.git
cd anime-social
```

Ative o ambiente virtual configurado na pasta do projeto:
*   **Windows:**
    ```bash
    .\venv\Scripts\activate
    ```
*   **Linux/Mac:**
    ```bash
    source venv/bin/activate
    ```

Caso o ambiente virtual acima não tenha sido copiado ou queira montar um do 0, você pode criá-lo:
```bash
python -m venv venv
.\venv\Scripts\activate   # ou source venv/bin/activate
pip install -r requirements.txt
```

Crie suas próprias chaves do Cloudinary em [cloudinary.com](https://cloudinary.com/). E configure temporariamente as variáveis de ambiente na sua máquina se quiser testar uploads localmente (como `CLOUDINARY_CLOUD_NAME`).

Faça as migrações (Cria as tabelas do banco de dados na sua máquina):
```bash
python manage.py migrate
```

Crie o usuário de laboratório localmente:
```bash
python manage.py createsuperuser
```

Suba o projeto localmente:
```bash
python manage.py runserver
```

**Parabéns!** O servidor está de pé no link: `http://127.0.0.1:8000/`.

---

## 🌐 Instruções Completas de Deploy (Render.com)

Preparar uma aplicação web para o mundo real exige alguns ajustes. Se você quer saber a trilha precisa para colocar **este projeto** no ar com estabilidade: 

O projeto conta com um documento na pasta raiz 📄 `deploy_guide.md` redigido inteiramente com o **passo a passo mastigado de Hospedagem**. Acesse-o no VS Code para ler a cartilha atual de integração entre o Github, o Render WebService, o Banco de Dados Gratuito em Nuvem e as Variáveis de Segredo da aplicação!

---

💡 *Desenvolvido com dedicação por Davi Silva.*
