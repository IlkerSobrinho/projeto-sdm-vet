Markdown# 🐾 Sistema Web de Gestão de Clínica Veterinária com Integração de APIs (A3 - SDM)

Projeto desenvolvido para a Avaliação A3 da disciplina de **Sistemas Distribuídos e Mobile (SDM)** do Centro Universitário IBMR (Prof. Fábio Gomes Pereira).

## 🏗️ Arquitetura do Sistema
O projeto demonstra na prática conceitos de **Sistemas Distribuídos**, dividido em 3 camadas independentes que se comunicam via HTTP/REST:
1. **API Privada da Clínica (`api_clinica.py` - Porta `8000`):** Gerencia as 10 entidades obrigatórias no banco relacional `clinica.db` (SQLite/SQLAlchemy), autenticação com hash SHA-256, consumo da API pública **ViaCEP** e fila de sincronização governamental.
2. **API Governamental Simulada (`api_governo.py` - Porta `8001`):** Serviço externo isolado com banco próprio (`governo.db`), responsável por registrar animais microchipados (RFID), receber notificações obrigatórias de **vacina antirrábica**, emitir protocolos únicos, garantir **idempotência** (evitando duplicidade em reenvios) e simular quedas (HTTP 503).
3. **Frontend Web (`index.html`):** Interface responsiva em HTML5, Bootstrap 5 e JavaScript (`fetch`), estruturada em 5 abas operacionais.

---

## 🚀 Como Rodar em Qualquer Computador (Windows / Linux)

### 1. Clonar o repositório
```bash
git clone SEU_LINK_DO_GITHUB_AQUI
cd projeto-sdm-vet
2. Criar e ativar o ambiente virtual PythonNo Windows (PowerShell):PowerShellpython -m venv .venv
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.venv\Scripts\activate
No Linux / macOS:Bashpython3 -m venv .venv
source .venv/bin/activate
3. Instalar as dependênciasBashpip install -r requirements.txt
4. Iniciar os Dois Servidores (Abra 2 Terminais)Terminal 1 — Subir a API Governamental Simulada (Porta 8001):Bashuvicorn api_governo:app --reload --port 8001
Terminal 2 — Subir a API Privada da Clínica (Porta 8000):Bashuvicorn api_clinica:app --reload --port 8000
5. Acessar o Sistema e a Documentação SwaggerInterface Web: Abra o ficheiro index.html diretamente no navegador.Swagger/OpenAPI (API Clínica): http://localhost:8000/docsSwagger/OpenAPI (API Governo): http://localhost:8001/docs🧪 Credenciais Prontas para Teste de LoginE-mailSenhaComportamento Esperadoativo@clinica.com123456HTTP 200 OK (Veterinário Ativo e Confirmado)inativo@clinica.com123456HTTP 403 Forbidden (Bloqueio de Veterinário Inativo)pendente@clinica.com123456HTTP 403 Forbidden (Bloqueio por Senha Não Confirmada — Código: CODIGO-123)