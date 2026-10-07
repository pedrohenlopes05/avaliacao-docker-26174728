# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome:
Matrícula:
Usuário do GitHub:
Usuário do Docker Hub:

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
R: nginx:1.27-alpine. 21MB

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
R: "/usr/share/nginx/html/". `docker exec teste-portal ls -l /usr/share/nginx/html/`.

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
R: Nome: "pedrolopes05/viaserra-portal:1.0-26174728". Link: "https://hub.docker.com/repository/docker/pedrolopes05/viaserra-portal/general"

4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?
R: "docker build -t pedrolopes05/viaserra-portal:1.0-26174728 ./portal" para recriar a imagem;
   "docker push pedrolopes05/viaserra-portal:1.0-26174728" para enviar a nova versão ao repositório.

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 |COPY pagina/ . |O Dockerfile tentava copiar a pasta pagina/, mas essa pasta não existia. A pasta correta era site/. |O docker build falhou com a mensagem "/pagina": not found. |Alterei para COPY site/ .. |
| 2 |CMD ["nginx"] |O Nginx era iniciado de forma que o processo principal do container terminava. |O container era criado, mas não aparecia em docker ps e ficava como Exited (0) em docker ps -a. |Alterei para CMD ["nginx", "-g", "daemon off;"], mantendo o Nginx em primeiro plano. |
| 3 |WORKDIR /usr/share/nginx |O index.html era copiado para /usr/share/nginx, mas o Nginx serve os arquivos do site em /usr/share/nginx/html. |O container podia ficar em execução, porém a página de manutenção correta não era exibida. |Alterei para WORKDIR /usr/share/nginx/html, fazendo o COPY site/ . colocar o arquivo no diretório correto. |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.

8. Qual comando derruba os dois containers de uma vez?

## Verificador

9. Código de conclusão impresso pelo verificador:

```
================================================================
 Verificador · Avaliação Prática de Docker · Turma C
================================================================
 Matrícula 26174728 · portal 8028 · manutenção 7028

A. Arquivos e Git
[ OK ] A1 portal/Dockerfile segue os requisitos
[ OK ] A2 .env fora do Git e .env.example versionado
[FALHA] A3 4+ commits e remoto no GitHub (encontrados: 2)
         -> faça um commit por parte e configure o origin
[ OK ] A4 imagem pedrolopes05/viaserra-portal:1.0-26174728 pública no Docker Hub

B. docker compose
         (ainda há lacunas ____ no docker-compose.yml)
[FALHA] B1 serviços portal e manutencao em execução
         -> rode docker compose up -d e confira com docker compose ps
[FALHA] B2 portal roda a imagem publicada
         -> imagem em uso:
[FALHA] B3 portas: portal em 8028 e manutenção em 7028
         -> portal= manutencao=

C. Conteúdo
[FALHA] C1 portal mostra seu nome e sua matrícula
         -> edite o rodapé do index.html, reconstrua, publique e recrie o container
[FALHA] C2 página de manutenção servindo o aviso "Voltamos em breve"
         -> o container responde, mas não com a página de manutenção (ou não responde)

================================================================
 Resultado: 3/9 verificações
 Ainda há falhas. Corrija e rode de novo.
================================================================
```
