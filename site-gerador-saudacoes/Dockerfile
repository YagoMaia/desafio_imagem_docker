# --- Estágio 1: Definir a imagem base ---
# Usamos a imagem oficial do Nginx com a tag 'alpine'.
# 'alpine' resulta em uma imagem muito menor, o que é ótimo para produção.
FROM nginx:alpine

# --- Estágio 2: Copiar os arquivos do projeto ---
# Copia o arquivo 'index.html' da sua máquina local (o contexto do build)
# para o diretório padrão onde o Nginx serve os arquivos HTML.
COPY index.html /usr/share/nginx/html/index.html

# --- Estágio 3: Expor a porta ---
# Informa ao Docker que o contêiner escutará na porta 80 em tempo de execução.
# Esta é a porta padrão do Nginx.
EXPOSE 80
