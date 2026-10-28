# Desafio DevSecOps — Gerenciador de Tarefas

## Sobre o Projeto
Este repositório faz parte do desafio prático do módulo de DevSecOps da ADA Tech.
Você receberá este projeto com vulnerabilidades propositais e uma pipeline incompleta.
Seu objetivo é **implementar a pipeline de segurança** e **corrigir as vulnerabilidades**.

## Estado atual
A pipeline está **incompleta**. Os steps de segurança precisam ser implementados por você.

## Sua missão
1. Implementar os steps de segurança no `pipeline.yml`
2. Fazer a pipeline **quebrar** ao detectar os problemas
3. Corrigir as vulnerabilidades encontradas
4. Fazer a pipeline **passar** com tudo verde ✅
5. Documentar o funcionamento da pipeline neste README

## O que implementar
- [ x] Secrets Scanning com **Gitleaks**
- [ x] SAST com **Semgrep**
- [ x] SCA com **Grype**
- [ x] Assinatura do artefato com cosign
- [ x] Deploy com **GitHub Pages**

## Como a pipeline funciona
A pipeline roda no arquivo `.github/workflows/pipeline.yml.
Job 1: Build, Segurança e Assinatura
Aqui é onde as ferramentas de segurança são executadas. Elas barreiras que  se alguma encontrar um problema, a pipeline para.

Secrets Scanning (Gitleaks): Ele vasculha o código atrás de senhas ou chaves escritas dentro do código e é importante porque evita que credenciais vazem.
2. SAST (Semgrep): Verifica o código-fonte sem executar, procurando por formas  erradas de programar, como o uso de `eval. Ele corrigi falhas no começo do projeto é mais barato corrigir.
3. SCA - Análise de Dependências (Grype):Verifica as bibliotecas de terceiros e avisa se alguma delas tem falhas de segurança conhecidas (CVEs). As vezes o código está correto, mas a biblioteca usada está vulnerável.
Depois disso o código é empacotado e recebe uma assinatura digital (Cosign) para garantir a integridade.

Job 2: Verificação e Deploy
Se todas as ferramentas do Job 1 passarem (tudo verde), a pipeline baixa o site e publica automaticamente no GitHub Pages.

 O que eu precisei corrigir no código
As ferramentas de segurança me ajudaram a encontrar os problemas que já existiam no projeto:

1.  Segredos expostos: Apaguei uma chave de API e uma senha que estavam escritas direto no src/script.js.
2.  Uso de eval: Troquei a linha que usava eval por console.log`que é mais seguro.
3.  Vulnerabilidade XSS: Substitui o innerHTML por `createElement e innerText, impedindo injeção de código malicioso.
4.  Dependências antigas: Atualizei as versões do lodash, express e axios no package.json.


AS DIFICULDADES DE DE UM INICIANTE QUE NUNCA USOU A FERRAMENTA:

Fazer isso pela primeira vez confesso que não foi fácil. Abaixo, listo os principais desafios que enfrentei e como consegui resolvê-los:


1.  Espaços vazios no YAML: Descobri que o arquivo `.yml` é extremamente sensível à indentação. Um único espaço a mais ou a menos fazia a pipeline nem começar a rodar. Tive que aprender a alinhar tudo.
2.  Conflitos no Merge. Quando fui tentar unir as minhas alterações da branch de trabalho com a main, tomei um susto com a mensagem `CONFLICT (content)`. Demorei para entender que precisava apagar as linhas com `<<<<<<<`, `=======` e `>>>>>>>` e usar os comandos de terminal para forçar a resolução correta, pois nem salvar estava conseguindo. (Tive que pedir ajudar).
3.  O erro de digitação no comando do Grype: Passei um bom tempo quebrando a cabeça com um erro no step do Grype. O log dizia `Invalid tag name "==package-lock-only". Quando fui olhar o código, percebi que por um erro de digitação, coloquei dois sinais de igual em vez de dois traços no comando do npm install. 
4. A Action desatualizada: Quando finalmente achei que tinha corrigido tudo, o Grype apontou uma vulnerabilidade que não estava no meu código, mas sim na própria Action do GitHub que eu estava usando (actions/download-artifact@v4). Precisei pesquisar e atualizar a versão dela no arquivo. (Mais uma vez tive que pedir ajuda).
5.  A dificuldade de salvar no Codespaces: Usar o VS Code direto no navegador foi ótimo, mas o atalho de salvar às vezes não respondia, e eu ficava perdido sem saber se o arquivo tinha salvado. Aprendi a usar o terminal (git add ) para garantir que as alterações fossem registradas. Não teve jeito, tive que pedir ajuda.



## URL de Produção
> Adicione aqui o link do GitHub Pages após o deploy.
