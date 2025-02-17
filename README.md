# Documentação Técnica: Run Unit Tests Workflow

Este workflow tem como objetivo executar testes unitários dentro de um ambiente Docker utilizando PHPUnit. O processo inclui:

•	Clonar o repositório para garantir acesso ao código-fonte.

•	Configurar autenticação no Composer para dependências do GitHub.

•	Configurar problem matchers para melhorar a leitura dos logs de erro.

•	Configurar credenciais da AWS para acessar recursos necessários.

•	Autenticar no ECR caso seja necessário fazer pull de imagens Docker.

•	Subir containers necessários para execução dos testes.

•	Executar os testes unitários via PHPUnit e gerar relatórios.

•	Compactar os relatórios de cobertura e logs.

•	Armazenar os relatórios como artefatos no GitHub Actions.


# Parâmetros de Entrada

**Autenticação**

•	CICD_GITHUB_TOKEN: Token de autenticação do GitHub para acessar pacotes no Composer.

**Credenciais AWS**

•	CICD_AWS_KEY: Chave de acesso AWS para autenticação.

•	CICD_AWS_SECRET: Chave secreta AWS para autenticação.

•	REGION: Região AWS onde os recursos estão localizados.

**Configuração do Ambiente**

•	APP_CONTAINER_NAME: Nome do container onde os testes serão executados.


# Etapas do Workflow

1. Checkout do Código

Clona o repositório, garantindo que todo o histórico esteja disponível.

2. Configuração do Composer

Cria um arquivo auth.json para permitir o acesso a pacotes do GitHub via Composer.

3. Configuração dos Problem Matchers

Adiciona regras para melhorar a interpretação dos logs de erro do PHPUnit e Composer dentro do GitHub Actions.

4. Configuração de Problem Matchers Adicionais

Utiliza uma ação de terceiros (mheap/phpunit-matcher-action) para fornecer suporte adicional a logs do PHPUnit.

5. Configuração da AWS

Autentica na AWS para permitir acesso a recursos como o repositório ECR.

6. Autenticação no Amazon ECR

Realiza login no Elastic Container Registry (ECR) para permitir o pull de imagens Docker.

7. Início dos Containers

Executa o script up.sh, que inicia os containers necessários para o ambiente de testes.

8. Execução dos Testes Unitários

Executa os testes dentro do container especificado utilizando PHPUnit, gerando:

•	Relatório de cobertura (coverage.xml).

•	Log de execução (report.junit.xml).

9. Compactação dos Relatórios

Cria um arquivo phpunit-reports.tar.gz contendo os logs e resultados dos testes.

10. Armazenamento dos Relatórios como Artefato

Faz upload do arquivo compactado para o GitHub Actions, permitindo a análise posterior.

# Considerações

•	Os testes são executados dentro de um ambiente Docker, garantindo isolamento e reprodutibilidade.

•	O uso de problem matchers melhora a legibilidade dos logs de erro no GitHub Actions.

•	Os relatórios são armazenados como artefato para facilitar o acesso após a execução do workflow.

•	A autenticação no ECR pode ser removida se os containers já estiverem disponíveis localmente.