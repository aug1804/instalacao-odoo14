# instalacao_odoo14

[Como instalar Odoo CE versão 14.0 no Ubuntu 20.04 com Python 3.8 e PostgreSQL 13]::

Instalar o Odoo pode não ser uma tarefa fácil para quem está começando mas com esse guia pretendo facilitar a vida de todos que estão em busca de instalar o Odoo e usufruir de todos as suas ferramentas disponíveis - e são muitas...

Vamos começar?

Ah, antes gostaria de pedir para colocar nos comentários todo e qualquer problema ou dificuldade que você venha a ter usando esse guia, ok? E caso eu não consiga alterar esse texto aqui, tem uma cópia atualizada no Github:

1. Se ainda não tem o Ubuntu versão 20, baixe a versão ISO, coloque num pendrive e instale o Ubuntu 20 no seu PC ou Notebook.
     (estou considerando que o nome do seu computador é 'aug' e que o seu principal usuário Ubuntu é também 'aug')
     → o $HOME do usuário aug é /home/aug
   Obs.: o Ubuntu 20 já vem com o Python 3.8 mas, se der algum problema, instale com: sudo apt install -y python3.8

2. Instalar o PostgreSQL 13
     sudo su
     apt update
     apt upgrade
     sudo apt install postgresql
   Depois de instalado com sucesso, use os comandos abaixo para:
     habilitar e iniciar o PostgreSQL para iniciar automaticamente na inicialização do sistema: 
       sudo systemctl enable postgresql
     e verificar o status:
       sudo systemctl status postgresql
   Crie um usuário do PostgreSQL de nome 'odoo':
     sudo su - postgres
     createuser -s odoo
   Liste os 'roles':
     psql
     \dg
     \q
     exit

3. Atualizar dependências
3.1. Crie o arquivo `dependencias.txt` com uma lista de todas as dependências do Odoo:
     → conteúdo do arquivo dependencias.txt:
       build-essential 
       fontconfig 
       gcc 
       git 
       gitg 
       libatlas-base-dev 
       libblas-dev 
       libffi-dev 
       libfreetype6 
       libjpeg-dev 
       libjpeg8-dev 
       liblcms2-dev 
       libldap2-dev 
       libmysqlclient-dev 
       libpq-dev 
       libsasl2-dev 
       libssl-dev 
       libssl1.1
       libx11-6 
       libxext6 
       libxml2-dev 
       libxrender1 
       libxslt1-dev 
       libzip-dev 
       nano 
       python3 
       python3.8-dev 
       python3.8-pip
       python3.8-venv 
       python3.8-wheel
       tig 
       virtualenv 
       wget 
       xfonts-75dpi 
       xz-utils 
       zlib1g-dev 
3.2. Depois de criar o arquivo, execute:
     sudo apt install -y -q < dependencias.txt

4. Instale o 'wkhtmltopdf'
   Vá para o terminal e execute:
     sudo su
     apt install wkhtmltopdf
   Caso haja algum problema:
     Execute o comando abaixo para baixar o pacote wkhtmltopdf do Github:
       wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb
     Após fazer o download do arquivo, instale-o usando os seguintes comandos:
       chmod +x wkhtmltox_0.12.6-1.focal_amd64.deb
       sudo apt install ./wkhtmltox_0.12.6-1.focal_amd64.deb
       ln -s /usr/local/bin/wkhtmltopdf /usr/bin/wkhtmltopdf
   Verifique se a instalação foi bem-sucedida conferindo a versão:
       wkhtmltopdf --version
   Pode ser necessário fazer essas cópias:
     sudo cp /usr/local/bin/wkhtmltopdf   /usr/bin/wkhtmltopdf
     sudo cp /usr/local/bin/wkhtmltoimage /usr/bin/wkhtmltoimage
     e / ou
     sudo cp /usr/bin/wkhtmltopdf   /usr/local/bin/wkhtmltopdf
     sudo cp /usr/bin/wkhtmltoimage /usr/local/bin/wkhtmltoimage

5. Instalação do Odoo 14 de acordo com o site oficial
   cd /home/aug
   sudo su
   wget -q -O - https://nightly.odoo.com/odoo.key | sudo gpg --dearmor -o /usr/share/keyrings/odoo-archive-keyring.gpg
   echo 'deb [signed-by=/usr/share/keyrings/odoo-archive-keyring.gpg] https://nightly.odoo.com/14.0/nightly/deb/ ./' | sudo tee /etc/apt/sources.list.d/odoo.list
   sudo apt-get update && apt-get upgrade && sudo apt-get install odoo


6. `Nesse ponto, já pode ir para o seu navegador e executar o Odoo`
   No seu navegador, execute: 'localhost:8069'
     Da 1a vez, o Odoo vai pedir para você criar seu banco de dados:
       Create Database
         Master Password: admin
         Database Name  : Odoo
         Email          : augustocalado@yahoo.com
         Password       : admin
         Phone number   : (81)9 3500-5736
         Language       : Portuguese (BR) / Português (BR)
         Country        : Brazil
         Demo data      : MARCAR ESSA OPÇÃO!!!

7. `git clone` módulos OCA
   Nesse ponto, vamos clonar os principais módulos da OCA e outros.
   Vamos criar um diretório /github e debaixo deles clonaremos os módulos.
     sudo su
     cd /
     mkdir github && cd github
     mkdir oca && cd oca
     git clone https://github.com/OCA/l10n-brazil
     git clone ... (vá clonando os módulos de acordo com sua necessidade)
     mkdir engenere && cd engenere
     git clone https://github.com/Engenere/engenere-addons
     e assim por diante...

8. Editar o arquivo /etc/odoo/odoo.conf
   → várias configurações, inclusive as PASTAS ONDE FICAM OS 'ADDONS' / OS MÓDULOS OCA E OUTROS
   Parâmetros que precisam ser alterados já:
     addons_path = /usr/lib/python3/dist-packages/odoo/addons, → módulos Odoo
                   /github/oca/l10n-brazil,                    → módulo da OCA
                   /github/engenere/engenere-addons            → módulo da Engenere (ATENÇÃO: A ÚLTIMA LINHA NÃO TEM VÍRGULA)
     admin_passwd = admin

9. Execute o Odoo, vá em Aplicativos e:
     - clique em 'Atualizar Lista de Aplicativos'
     - instale primeiro todos os módulos Odoo
     - depois instale os módulos OCA, começando pelo l10n_br_base
     - depois o l10n_br_fiscal
     - depois o l10n_br_coa_generic e ou l10_br_coa_simple
     - depois l10n_br_account
     - só depois os demais módulos, de acordo com suas necessidades
   Talvez agora você tenha que derrubar o Odoo e levantar novamente:
     sudo systemctl restart odoo
   E dar um F5 no seu navegador.

E seja bem-vindo ao mundo Odoo!!!
