---

## Passos Preparatórios para Configuração do Nginx (HTTP/3)

Antes de configurar o Nginx, é fundamental preparar o ambiente, incluindo a configuração do DNS local e a organização dos certificados SSL.

---

### 1. Configuração do Arquivo `hosts`

Para testar seu ambiente de desenvolvimento localmente, é necessário mapear o nome de domínio que o Nginx usará para o endereço IP do seu próprio computador (loopback). Isso evita a necessidade de um DNS público.

1.  **Edite o arquivo `hosts`:**
    Abra o arquivo `hosts` com permissões de superusuário:

    ```bash
    sudo nano /etc/hosts
    # ou sudo vim /etc/hosts
    ```

2.  **Adicione a entrada para seu domínio:**
    No final do arquivo, adicione a seguinte linha:

    ```
    127.0.0.1       oficina.uffs.edu.br
    ```
    * `127.0.0.1`: É o endereço de loopback, que sempre aponta para o seu próprio computador.
    * `oficina.uffs.edu.br`: É o nome de domínio que você usará para acessar seu site Nginx localmente.

3.  **Salve e feche o arquivo.**

Você pode verificar se o mapeamento funcionou com um `ping`:

```bash
ping oficina.uffs.edu.br
```
A resposta deve mostrar que o ping está sendo enviado para `127.0.0.1`.

---

### 2. Organização dos Certificados SSL

Seu Nginx precisa de um certificado SSL (e sua chave privada) para habilitar conexões HTTPS (e posteriormente HTTP/3). Recomendamos criar um diretório dedicado para eles e copiar os arquivos para lá.

1.  **Crie o diretório para os certificados:**
    É uma boa prática centralizar os certificados SSL em um local seguro e padronizado. Crie o diretório `/etc/nginx/ssl/`:

    ```bash
    sudo mkdir -p /etc/nginx/ssl
    ```

2.  **Copie os arquivos de certificado:**
    Você pode obter os arquivos de certificado (`certificate.crt` e `private.key`) do seu repositório Git. Certifique-se de que eles estão no seu ambiente local (por exemplo, na pasta onde você clonou o repositório) e então copie-os para o diretório recém-criado:

    ```bash
    # Supondo que você esteja no diretório onde os arquivos certificate.crt e private.key estão
    sudo cp certificate.crt /etc/nginx/ssl/nginx-selfsigned.crt
    sudo cp private.key /etc/nginx/ssl/nginx-selfsigned.key
    ```

3.  **Defina as permissões corretas:**
    A chave privada (`.key`) deve ter permissões restritivas para segurança. O certificado (`.crt`) pode ser mais aberto.

    ```bash
    sudo chmod 600 /etc/nginx/ssl/private.key
    sudo chmod 644 /etc/nginx/ssl/certificate.crt.crt
    sudo chmod 755 /etc/nginx/ssl/ # Garante que o diretório é acessível
    ```

---
