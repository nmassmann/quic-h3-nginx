---
## Configuração Nginx com Suporte a HTTP/3 (QUIC) para `oficina.uffs.edu.br`

Este documento explica as alterações implementadas no arquivo de configuração do Nginx, `default.conf`, para habilitar o suporte a **HTTP/3 (QUIC)**, além de manter a compatibilidade com HTTP/1.1 e HTTP/2. Esta configuração redireciona todo o tráfego HTTP para HTTPS e serve o conteúdo do site `oficina.uffs.edu.br`.

Você pode copiar o conteúdo do `default_quic.conf` fornecido e substituí-lo pelo seu `default.conf` no diretório `/etc/nginx/conf.d/` ou `/etc/nginx/sites-available/` (dependendo da sua distribuição e como você organiza os arquivos de configuração do Nginx). Lembre-se de criar um symlink para `sites-enabled` se usar a segunda opção.

---
### Destaques das Alterações

As principais modificações visam aprimorar a segurança e o desempenho da entrega de conteúdo web:

#### 1. Redirecionamento HTTP para HTTPS

* **Configuração:** Um bloco `server` separado foi adicionado para escutar na porta `80` (HTTP).
* **Funcionalidade:** Qualquer requisição HTTP para `oficina.uffs.edu.br` será automaticamente redirecionada (código `301 Moved Permanently`) para a versão HTTPS. Isso garante que todos os usuários acessem o site de forma segura.

    ```nginx
    server {
        listen 80;
        server_name oficina.uffs.edu.br;
        location / {
            return 301 https://$host$request_uri;
        }
    }
    ```

#### 2. Habilitação de HTTP/3 (QUIC)

* **Escuta QUIC:** A diretiva `listen 443 quic reuseport;` foi adicionada no bloco `server` principal. Isso permite que o Nginx aceite conexões QUIC (HTTP/3) na porta 443. O `reuseport` é importante para otimizar o uso da porta em sistemas multicore.
* **Cabeçalho `Alt-Svc`:** O cabeçalho `Alt-Svc 'h3=":$server_port"; ma=86400';` é enviado nas respostas HTTPS. Ele informa aos navegadores que o servidor suporta HTTP/3 na porta especificada, incentivando-os a tentar a conexão via QUIC para futuras requisições.

    ```nginx
    listen 443 quic reuseport;
    listen 443 ssl; # Mantém compatibilidade com HTTP/1.1 e HTTP/2
    # ...
    add_header Alt-Svc 'h3=":$server_port"; ma=86400';
    ```

#### 3. Configuração SSL/TLS Aprimorada

* **Certificados:** Os caminhos para o certificado e a chave SSL (`ssl_certificate` e `ssl_certificate_key`) estão configurados. **É fundamental que esses caminhos (`/etc/nginx/ssl/certificate.crt` e `/etc/nginx/ssl/private.key`) apontem para os seus arquivos de certificado autoassinados ou emitidos por uma CA.**
* **Protocolos TLS:** Apenas os protocolos TLSv1.2 e TLSv1.3 são permitidos (`ssl_protocols TLSv1.2 TLSv1.3;`), garantindo que apenas as versões mais seguras do TLS sejam usadas.
* **Ciphers Seguros:** Uma lista de ciphers seguros e robustos (`ssl_ciphers`) foi definida, priorizando a segurança e o desempenho (`ssl_prefer_server_ciphers on;`).

#### 4. Otimizações QUIC (Opcional)

* **`quic_retry on;`**: Habilita o QUIC Connection Migration, permitindo que as conexões QUIC persistam mesmo se o endereço IP do cliente mudar.
* **`quic_gso on;`**: Habilita Generic Segmentation Offload para QUIC, o que pode melhorar o desempenho do throughput.

#### 5. Diretório Raiz e Logs

* **`root /usr/share/nginx/html;`**: Define o diretório onde o Nginx buscará os arquivos para servir. Certifique-se de que seus arquivos web (`index.html`, etc.) estejam localizados aqui.
* **`access_log /var/log/nginx/host.access.log main;`**: Configura o log de acesso para registrar as requisições ao servidor.

---
### Como Aplicar e Testar

1.  **Salve o Arquivo:** Copie o conteúdo fornecido para o seu arquivo de configuração do Nginx (ex: `/etc/nginx/conf.d/default.conf`).
2.  **Verifique a Sintaxe:** Antes de reiniciar o Nginx, verifique a sintaxe da configuração:
    ```bash
    sudo nginx -t
    ```
    Se houver erros, eles serão exibidos. Corrija-os antes de prosseguir.
3.  **Reinicie o Nginx:** Se a sintaxe estiver OK, reinicie o serviço Nginx para aplicar as novas configurações:
    ```bash
    sudo systemctl restart nginx
    ```
4.  **Teste o Acesso:**
    * Acesse `http://oficina.uffs.edu.br` no seu navegador e verifique se ele é redirecionado para HTTPS.
    * Acesse `https://oficina.uffs.edu.br`.
    * Para verificar se HTTP/3 está funcionando, você pode usar ferramentas como a extensão "HTTP/3 and QUIC" para navegadores ou ferramentas de linha de comando como `curl` (versões recentes com suporte a QUIC):
        ```bash
        curl -I --http3 https://oficina.uffs.edu.br
        ```
        Procure pelo cabeçalho `alt-svc` na resposta ou por mensagens indicando o uso de QUIC.

---
### Observações Finais

* **Certificados Autoassinados:** Lembre-se que certificados autoassinados gerarão avisos de segurança nos navegadores. Para um ambiente de produção, é altamente recomendável usar certificados emitidos por uma Autoridade Certificadora (CA) confiável, como Let's Encrypt.
* **Firewall:** Certifique-se de que as portas `80` (TCP), `443` (TCP) e `443` (UDP, para QUIC) estejam abertas no seu firewall.

Este arquivo de configuração oferece uma base sólida para habilitar HTTP/3 no seu servidor Nginx, melhorando a velocidade e a segurança para os usuários de `oficina.uffs.edu.br`.
