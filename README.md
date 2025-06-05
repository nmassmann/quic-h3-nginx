# HTTP/3 com QUIC no Nginx: Descomplicando da Teoria à Implementação

Este repositório contém os materiais e configurações para a oficina prática sobre os protocolos **QUIC** (Quick UDP Internet Connections) e **HTTP/3** e sua configuração no servidor web **Nginx** em um ambiente **Ubuntu 22.04**.

---

## Sobre esta Oficina

Nesta oficina, vamos explorar a próxima geração de protocolos de comunicação web, o **HTTP/3**, que é construído sobre o inovador protocolo de transporte **QUIC**. Entenderemos por que esses protocolos são cruciais para a performance e segurança da internet moderna e, o mais importante, vamos **colocar a mão na massa** para configurar e testar o **Nginx** com suporte a eles.

Nosso objetivo é proporcionar uma experiência prática, desde a instalação das dependências até a configuração final e a verificação do funcionamento do HTTP/3.

---

## Conteúdo Abordado

* **Introdução a QUIC e HTTP/3**: Compreendendo a evolução e os benefícios.
* **Preparação do Ambiente**: Instalação do Ubuntu 22.04 (ou ambiente similar).
* **Instalação do Nginx QUIC**: Utilizando pacotes pré-compilados.
* **Configuração do Nginx**: Habilitando HTTP/3 e SSL (TLS 1.3).
* **Geração e Uso de Certificados Auto-assinados**: Testes locais seguros.
* **Testes e Verificação**: Confirmando o funcionamento do HTTP/3.

---

## Materiais de Apoio e Referências

Aqui estão alguns recursos valiosos que serviram de base ou complementam o conteúdo desta oficina:

* **Apresentação Base da Oficina**:
    * [QUIC - SemanaCap NIC.br](https://semanacap.bcp.nic.br/files/apresentacao/arquivo/1808/QUIC.pdf)
    *(Este arquivo será a base do nosso aprendizado, contendo os slides principais da oficina.)*

* **Protocolos a Nível Mundial**:
    * [W3Techs - Overview of Site Elements](https://w3techs.com/technologies/overview/site_element)
    *(Entenda a adoção e o panorama dos protocolos de rede na web global.)*

* **Implantação nos Navegadores**:
    * [Can I use... HTTP/3](https://caniuse.com/http3)
    *(Acompanhe o status de suporte ao HTTP/3 nos principais navegadores.)*

* **Conteúdo Audiovisual - Entendendo o protocolo QUIC (HTTP/3 e TLS 1.3)**:
    * [Gravação da SemanaCap 8 - Curso sobre QUIC](https://www.youtube.com/live/8hmnzwmaJDo)
    *(Neste video, você pode acompanhar o curso “Entendendo o protocolo #QUIC (HTTP/3 e TLS 1.3), o protocolo que está mudando a Internet!" que ocorreu na 8ª edição da Semana de Capacitação Online do NIC.br.)*

* **Servidor Alternativo com Suporte a HTTP/3:
   * OpenLiteSpeed - HTTP/3 Support
     *(O OpenLiteSpeed é um servidor web leve, gratuito e de alto desempenho que também oferece suporte nativo a HTTP/3 e QUIC. Pode ser uma alternativa interessante ao Nginx em diversos cenários.)* 

---

## 🛠️ Pré-requisitos

* Um ambiente com **Ubuntu 22.04** (máquina virtual ou servidor).
* Acesso de superusuário (`sudo`).
* Conexão com a internet para download de pacotes.
* **Nginx versão 1.25.1 (ou superior) com suporte a QUIC/HTTP/3**.

---

## 👨‍🏫 Instrutor

* **Neimar Marcos Assmann**
    * **Apresentação:** Analista de Tecnologia da Informação na Universidade Federal da Fronteira Sul (UFFS), com mais de 20 anos de experiência em infraestrutura de TI, redes e sistemas. Possui graduação em Tecnólogo em Informática, especialização em Teleinformática em Redes de Computadores e mestrado em Ciência da Computação.

---

## Ordem dos Passos para Configuração do Nginx com HTTP/3 (QUIC)

Este documento descreve a sequência recomendada para seguir os arquivos de configuração e documentação presentes neste repositório, visando a instalação e configuração do Nginx com suporte a HTTP/3.

---

### 1. Preparação do Ambiente

* **`preparacao.md`**:
    * Este arquivo deve ser consultado primeiro. Ele provavelmente contém informações sobre pré-requisitos, atualizações de sistema operacional, ou qualquer outra etapa inicial necessária antes de instalar o Nginx.

---

### 2. Instalação do Nginx

* **`Instalacoes_Nginx.md`**:
    * Após a preparação do ambiente, siga este guia para instalar o Nginx em seu sistema.

---

### 3. Configurações Básicas e Certificados

* **`certificados` (Pasta)**:
    * Antes de configurar o Nginx para HTTPS, você precisará dos seus certificados SSL/TLS. Esta pasta deve conter instruções ou os próprios arquivos de certificado (`.crt` e `.key`) que serão utilizados na configuração do Nginx. Gere ou coloque seus certificados aqui.
* **`hosts`**:
    * Este arquivo pode conter exemplos ou instruções para configurar o `/etc/hosts` localmente, o que pode ser útil para testes ou para mapear domínios antes da configuração de DNS.
* **`configurando_nginx.md`**:
    * Este arquivo provavelmente aborda as configurações gerais e essenciais do Nginx, que podem incluir conceitos básicos, diretórios importantes e permissões.

---

### 4. Configuração Avançada (HTTP/3)

* **`default_quic.conf`**:
    * Este é o arquivo de configuração principal para habilitar HTTP/3 e HTTPS. Siga as instruções fornecidas (em outros arquivos ou diretamente neste `.conf`) para colocá-lo no local correto do Nginx (geralmente `/etc/nginx/conf.d/` ou `/etc/nginx/sites-available/` com um symlink para `sites-enabled`).

---

### 5.  Comandos Úteis

* **`Comandos_Nginx.md`**:
    * Após a configuração, este arquivo será essencial para verificar a sintaxe do Nginx, reiniciar o serviço e testar se tudo está funcionando como esperado. Ele deve conter comandos úteis para gerenciamento do Nginx.

---

### 6. Conteúdo do Site

* **`html.zip`**:
    * Este arquivo contém o conteúdo HTML  site. Descompacte-o e coloque os arquivos no diretório raiz configurado no `default_quic.conf` (provavelmente `/usr/share/nginx/html/`).

---


## Contribuições

Sinta-se à vontade para abrir [issues](https://github.com/SEU_USUARIO/SEU_REPOSITORIO/issues) ou [pull requests](https://github.com/SEU_USUARIO/SEU_REPOSITORIO/pulls) se tiver sugestões de melhoria ou encontrar problemas.

---
