# Instalação Rápida do Nginx (HTTP/3 Ready)

---

### 1. Atualizar o Sistema e Instalar Ferramentas Essenciais

Este comando atualiza a lista de pacotes disponíveis e instala ferramentas necessárias para adicionar novos repositórios e gerenciar chaves GPG.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
```

### 2. Adição da Chave GPG do Nginx

Baixa e adiciona a chave de assinatura GPG oficial do Nginx.

```bash
curl [https://nginx.org/keys/nginx_signing.key]| gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

### 3. Configuração do Repositório Nginx Mainline

Adiciona o repositório mainline oficial do Nginx à sua lista de fontes apt.
```bash
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] [http://nginx.org/packages/mainline/ubuntu]`lsb_release -cs` nginx" | sudo tee /etc/apt/sources.list.d/nginx.list
```

### 4. Definição de Prioridade de Pacotes

Garante que o Nginx seja instalado preferencialmente deste novo repositório.
```bash
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" | sudo tee /etc/apt/preferences.d/99nginx
```
### 5. Atualização Final do Cache de Pacotes

Atualiza o apt para reconhecer os novos pacotes e prepara para a instalação.
```bash
sudo apt update && sudo apt upgrade -y
```
Após estes passos, o Nginx estará pronto para ser instalado com:
```bash
sudo apt install nginx -y
```



