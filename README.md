# 🐾 Sistema de Gestão de Clínica Veterinária (A3 - SDM)

Projeto desenvolvido para a Avaliação A3 da disciplina de **Sistemas Distribuídos e Mobile (SDM)** do Centro Universitário IBMR.

---

## 🏗️ Arquitetura do Sistema

O projeto demonstra conceitos de **Sistemas Distribuídos**, operando com serviços desacoplados que se comunicam via HTTP/REST:

1. **API Privada da Clínica (`api_clinica.py` — Porta 8000)**
   - Persistência relacional via SQLite (`clinica.db`) com SQLAlchemy
   - Autenticação e hash de segurança
   - Integração com a API pública **ViaCEP**
   - Gestão de pendências e fila de sincronização

2. **API Governamental Simulada (`api_governo.py` — Porta 8001)**
   - Persistência isolada (`governo.db`)
   - Registro obrigatório de vacinação antirrábica e controle de microchip RFID
   - Garantia de **idempotência** (evita duplicidade em retentativas)
   - Painel de simulação de indisponibilidade (HTTP 503)

3. **Frontend Web (`index.html`)**
   - Interface em HTML5, Bootstrap 5 e JavaScript assíncrono (`fetch`)
   - Estruturada em 5 abas operacionais: Autenticação, Tutores, Animais, Prontuário e Fila de Integração

---

## 🚀 Como Executar o Projeto

### 1. Clonar o Repositório

```bash
git clone https://github.com/IlkerSobrinho/projeto-sdm-vet.git
cd projeto-sdm-vet2. Criar e Ativar o Ambiente VirtualWindows (PowerShell):PowerShellpython -m venv .venv
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.venv\Scripts\activateLinux / macOS:Bashpython3 -m venv .venv
source .venv/bin/activate3. Instalar as DependênciasBashpip install -r requirements.txt4. Iniciar os ServiçosAbra dois terminais:Terminal 1 — API Governamental (Porta 8001):Bashuvicorn api_governo:app --reload --port 8001Terminal 2 — API da Clínica (Porta 8000):Bashuvicorn api_clinica:app --reload --port 8000Nota: Na primeira inicialização da API da Clínica, os bancos clinica.db e governo.db são gerados automaticamente com dados iniciais de teste.🌐 Acessos e Documentação




















RecursoURL / CaminhoInterface WebAbra o arquivo index.htmlSwagger OpenAPI (Clínica)http://localhost:8000/docsSwagger OpenAPI (Governo)http://localhost:8001/docs🧪 Contas Pré-configuradas para Teste de Login
























E-mailSenhaComportamento / Cenárioativo@clinica.com123456HTTP 200 OK — Veterinário ativo e com senha confirmadainativo@clinica.com123456HTTP 403 Forbidden — Bloqueio de acesso para veterinário inativopendente@clinica.com123456HTTP 403 Forbidden — Bloqueio por confirmação pendente (Código: CODIGO-123)