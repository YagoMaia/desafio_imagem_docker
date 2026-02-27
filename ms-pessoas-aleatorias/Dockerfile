# --- Estágio 1: Builder ---
# Usamos a imagem Python 3.13 na versão 'slim' como base para o build.
# 'slim' é menor que a imagem padrão.
# Nomeamos este estágio como 'builder'.
FROM python:3.13-slim AS builder

# Define o diretório de trabalho dentro do container.
WORKDIR /app

# Atualiza o pip para a versão mais recente.
RUN pip install --upgrade pip

# Copia apenas o arquivo de dependências para o container.
# Isso aproveita o cache de camadas do Docker. Se o requirements.txt não mudar,
# o Docker reutilizará a camada cacheada da instalação de dependências.
COPY requirements.txt .

# Instala as dependências como 'wheels' em um diretório separado.
# Wheels são um formato pré-compilado que torna a instalação no próximo estágio mais rápida.
RUN pip wheel --no-cache-dir --wheel-dir /app/wheels -r requirements.txt


# --- Estágio 2: Final ---
# Começamos uma nova imagem, do zero, baseada na mesma versão do Python.
# Isso garante que a imagem final seja limpa e contenha apenas o necessário.
FROM python:3.13-slim

# Define o diretório de trabalho.
WORKDIR /app

# Copia as dependências pré-compiladas (wheels) do estágio 'builder'.
COPY --from=builder /app/wheels /app/wheels

# Copia o código-fonte da aplicação para o container.
COPY . .

# Instala as dependências a partir dos wheels locais.
# Isso é mais rápido e não requer acesso à internet.
RUN pip install --no-cache-dir /app/wheels/*

# Expõe a porta 8000, que é a porta padrão onde o Uvicorn irá rodar.
EXPOSE 8000

# Define o comando para iniciar a aplicação quando o container for executado.
# Usa "0.0.0.0" para que a API seja acessível de fora do container.
# Não usamos '--reload' em produção.
# Os arquivos do projeto (main.py, database.py, etc.) estão em /app
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
