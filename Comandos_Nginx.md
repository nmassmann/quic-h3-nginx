---

## Gerenciamento Essencial do Nginx

Este guia rápido apresenta os comandos fundamentais para verificar a versão, testar a configuração e controlar o serviço Nginx.

---

### Verificando a Versão

Use este comando para confirmar a versão do Nginx instalada no seu sistema.

```bash
nginx -v
```

**Exemplo de Saída:**
```
nginx version: nginx/1.27.5
```

---

### Testando a Configuração

Sempre execute este comando antes de aplicar qualquer alteração na configuração. Ele verifica a sintaxe dos seus arquivos Nginx e aponta erros.

```bash
sudo nginx -t
```

**Saída em Caso de Sucesso:**
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

---

### Controlando o Serviço Nginx

Os comandos `systemctl` permitem gerenciar o estado do Nginx.

#### **Status do Serviço**

Verifica se o Nginx está ativo (`active (running)`) e exibe informações detalhadas, incluindo as últimas linhas do log.

```bash
systemctl status nginx
```

#### **Iniciar o Serviço**

Inicia o Nginx.

```bash
systemctl start nginx
```

#### **Parar o Serviço**

Para o Nginx, tornando-o inacessível.

```bash
systemctl stop nginx
```

#### **Recarregar Configurações**

Aplica novas configurações sem interromper o serviço. É o método preferencial para atualizações.

```bash
systemctl reload nginx
```

#### **Habilitar/Desabilitar Inicialização Automática**

Define se o Nginx deve iniciar automaticamente junto com o sistema operacional.

* **Habilitar (Inicia no Boot):**
    ```bash
    sudo systemctl enable nginx
    ```

* **Desabilitar (Não Inicia no Boot):**
    ```bash
    sudo systemctl disable nginx
    ```

---
