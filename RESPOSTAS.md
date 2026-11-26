# Respostas da aula guiada

Equipe

Integrantes = Eduardo Santana, Alan Lins, José Ricardo, Vinicius Matos

Repositório = https://github.com/Santof-dev/quiz-devops-aula

Endereço publicado

Responda Q01 a Q22 em equipe. Cada integrante deve registrar suas respostas próprias a Q23 e Q24. Registre previsões antes de executar e use a linha Observação após o resultado.

## Etapa 01 Preparação do projeto e das contas

### Q01

Se queremos automatizar os testes e disponibilizar o site por um endereço, qual responsabilidade cabe ao GitHub Actions e qual cabe ao GitHub Pages?

Resposta = O GitHub Actions automatiza a execução, ele roda os testes e os passos do pipeline sempre que ocorre um evento, como um push. O GitHub Pages hospeda o site e o disponibiliza por um endereço na internet.

Observação após a execução = O repositório foi preparado no GitHub, utilizando a branch main. Nesta etapa, ainda era necessário configurar o GitHub Actions e o GitHub Pages para automatizar os testes e disponibilizar o site.

### Q02

Que problema operacional pode ocorrer se cada integrante publicar uma cópia diferente do código sem registrar a versão utilizada?

Resposta = Ninguém saberia qual versão está no ar nem qual foi testada. Isso causa conflito entre cópias, erros difíceis de reproduzir, perda de alterações e impossibilidade de voltar a uma versão anterior

Observação após a execução = O projeto ficou registrado em um repositório centralizado na branch main, permitindo que os integrantes utilizem a mesma versão do código e acompanhem o histórico das alterações.

Evidência = https://github.com/Santof-dev/quiz-devops-aula, branch main

Anote no arquivo de respostas o link do repositório e o nome da branch.

## Etapa 02 Código inicial e primeiro commit

### Q03

Que informação o commit registra para que outra pessoa consiga localizar esta entrega depois?

Resposta = O commit registra o autor, a data e hora, a mensagem e o identificador único (hash) do conjunto de alterações. Com esse identificador, qualquer pessoa localiza exatamente aquela versão no histórico.

Observação após a execução = O commit inicial foi registrado com a mensagem "Adiciona aplicação inicial do quiz" e o identificador f62e7d3, permitindo localizar essa versão no histórico do repositório.

### Q04

O que pode acontecer com nosso pipeline se site/index.html ficar dentro de projeto_inicial/site/index.html no repositório?

Resposta = O workflow e os testes procuram o site na pasta site/ da raiz. Se estivesse em projeto_inicial/site/, o path: './site' apontaria para uma pasta inexistente, o teste não encontraria ../site/pontuacao.js e a publicação falharia ou ficaria vazia.

Observação após a execução = Foi verificado que a pasta site e o arquivo package.json estavam na raiz do repositório, mantendo a estrutura necessária para a execução dos testes e a futura publicação.

Evidência = mensagem Adiciona aplicação inicial do quiz e identificador f62e7d3

Copie a mensagem e o identificador curto do commit. Para consultar, use o histórico de commits da aba Code ou History em um arquivo.

## Etapa 03 Mudança identificável e histórico

### Q05

Se uma execução futura apresentar um erro, por que devemos anotar o identificador do commit associado a ela?

Resposta = O identificador do commit indica exatamente qual versão do código gerou a execução com erro. Com ele, a equipe compara com a versão anterior, descobre o que mudou, reproduz o problema e sabe a qual versão voltar ou corrigir.

Observação após a execução = A alteração do título do quiz foi registrada no commit 7359b48, permitindo identificar a nova versão e compará-la com a versão anterior do projeto.

### Q06

A mudança do título já iniciou algum teste automático neste momento? O que ainda precisamos configurar?

Resposta = Não. A mudança do título criou apenas um novo commit no histórico. Nenhum teste rodou, porque ainda não existe workflow nem testes automáticos. Falta criar o arquivo de testes e o comando npm test (Etapa 5) e o workflow do GitHub Actions em .github/workflows/ (Etapa 6), que reage ao evento push.

Observação após a execução = A alteração do título gerou um novo commit no histórico do repositório, mas não iniciou testes automáticos, pois o workflow ainda não estava configurado nessa etapa.

Evidência = [7359b48] . A diferença entre as versões: o título da página passou de "Quiz Cloud e DevOps" para "Quiz Cloud e DevOps - Turma 2026"

Registre o identificador do novo commit e descreva em uma frase a diferença entre as duas versões.

## Etapa 04 Regra de negócio que será protegida

### Q07

Por que executar testes sobre a mesma função utilizada pela interface ajuda a verificar a versão que vamos publicar?

Resposta = A interface (app.js) importa calcularPontuacao de pontuacao.js, e os testes importam a mesma função. Assim o teste verifica o código que o usuário realmente usa, e não uma cópia. Se a função quebrar, o teste falha antes da publicação.

Observação após a execução = Ao analisar o arquivo site/pontuacao.js, foi identificada a função calcularPontuacao, responsável pela regra de pontuação do quiz. Essa função é a que deve ser verificada pelos testes automatizados.

### Q08

Defina o resultado esperado para três acertos e para nenhuma resposta marcada. Qual regra sustenta sua previsão?

Resposta = Para três acertos, o resultado esperado é 3. Para nenhuma resposta marcada, é 0. A regra: cada resposta igual ao gabarito vale 1 ponto, e respostas null ou ausentes não valem ponto.

Observação após a execução = Ao analisar a função calcularPontuacao, foi observado que cada resposta correta acrescenta um ponto e que respostas nulas ou ausentes não acrescentam pontos, confirmando a previsão de 3 pontos para três acertos e 0 para nenhuma resposta marcada.

Evidência = previsões 3 e 0. Caminho da função: site/pontuacao.js.

Escreva no arquivo de respostas os dois resultados previstos e o caminho da função.

## Etapa 05 Testes automatizados e comando do projeto

### Q09

Qual é a sequência entre npm test, package.json e os arquivos de teste?

Resposta = Ao rodar npm test, o npm lê o package.json e procura o script test, que é node --test. O Node então localiza os arquivos terminados em .test.js, executa cada teste e informa quais passaram ou falharam. A sequência é: npm test, depois package.json, depois o runner do Node, depois os arquivos de teste.

Observação após a execução = o commit 0493162 adicionou tests/pontuacao.test.js com 5 testes. Ainda não rodamos npm test, porque a execução automática só acontece a partir da Etapa 6.

### Q10

Se o runner executar zero testes e terminar sem erro, isso é evidência suficiente para liberar a versão? O que você conferiria?

Resposta = Não. Zero testes executados sem erro pode significar que o runner não encontrou os arquivos, por causa de pasta ou nome errado. Eu conferiria no log:

se o número de testes executados é maior que zero;
se todos aparecem como aprovados;
se os nomes dos testes aparecem na saída, por exemplo "tres respostas corretas somam tres pontos".

Observação após a execução = o arquivo ficou em tests/, e não em test/ como o roteiro sugeria. Como o Node procura arquivos .test.js, isso não deve causar problema, mas vamos confirmar no log da Etapa 7.

Evidência = Arquivo de testes: tests/pontuacao.test.js
package.json: na raiz do repositório
Comando definido para os testes: node --test 
Commit: 0493162

Registre os caminhos do arquivo de teste e do package.json e o comando definido para os testes.

## Etapa 06 Primeiro workflow de integração contínua

### Q11

Preveja se o commit que cria esse YAML na main inicia o pipeline. Qual evento e qual filtro do arquivo justificam sua resposta?

Resposta = Sim, o commit que cria o YAML na main inicia o pipeline. O evento é push, e o filtro branches: [main] faz o workflow rodar para pushes na main.

Observação após a execução = O push do commit 8ca71a3 iniciou a execução no Actions, como previsto.

### Q12

Qual é a vantagem de registrar o workflow no mesmo histórico do código da aplicação?

Resposta = O workflow fica versionado junto com o código. Cada mudança na automação tem autor, data e commit, dá para comparar versões, voltar atrás e saber qual versão do pipeline rodou sobre qual versão do código.

Observação após a execução

Evidência = .github/workflows/ci-cd.yml, commit 8ca71a3

Registre o caminho do YAML e o identificador do commit que o criou.

## Etapa 07 Execução no Actions e leitura dos logs

### Q13

Por que configuramos a versão do Node.js no runner? No trecho, o que significam v7 e 24?

Resposta = Configuramos a versão do Node.js para que o runner use um ambiente previsível e igual ao esperado pelo projeto, sem depender do que já vem instalado na máquina. No trecho actions/setup-node@v7 com node-version: '24', v7 é a versão da action setup-node (a ferramenta que prepara o Node), e 24 é a versão do Node.js que será instalada.

Observação após a execução = log confirmou a versão pedida no YAML.

### Q14

Que dados do log você registraria para comprovar que esta revisão foi verificada? Indique também a diferença entre uses e run.

Resposta = Eu registraria o identificador do commit (e18c179), o número e o link da execução (#2), o evento que a iniciou (push), o resultado do passo "Executar testes" e o resumo dos testes. A diferença: uses chama uma action pronta e reutilizável, e run executa um comando de terminal, como npm test.

Observação após a execução = A execução #1 (commit 8ca71a3) falhou com Missing script: "test". Depois do commit e18c179, a #2 passou. O log mostrou tests 5, pass 5, fail 0, cancelled 0, skipped 0. Na #3, rodada manualmente, o resultado foi sucesso em 17s.

Evidência = Execução automática (#2): https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45849346109. Evento: push, commit e18c179.
Execução manual (#3): clique na #3 na lista do Actions e copie a URL da barra do navegador. Evento: workflow_dispatch, que aparece como "Manually run by Santof-dev".
Resumo real dos testes: 5 testes, 5 aprovados, 0 falhas, 0 cancelados, 0 pulados.

Cole os links da execução automática e da manual e o resumo real dos testes. Identifique o evento observado em cada uma.

## Etapa 08 Falha controlada e correção

### Q15

Antes de executar, quais dos cinco casos você prevê que falharão ao dobrar o ponto por acerto? Justifique.

Resposta = Prevejo que o segundo e o terceiro casos de teste irão falhar, pois, ao dobrar o ponto por acerto, uma resposta correta passará a valer dois pontos em vez de um, e três respostas corretas passarão a valer seis pontos em vez de três. Os demais testes deverão continuar passando, pois não possuem respostas corretas que gerem pontuação.

Observação após a execução = Após a execução, a previsão foi confirmada. O segundo teste esperava 1 ponto, mas recebeu 2, enquanto o terceiro teste esperava 3 pontos, mas recebeu 6. A execução de identificador 45856760745 apresentou falha devido à alteração proposital na pontuação.

### Q16

Por que reexecutar a execução defeituosa não substitui fazer o commit da correção? Use os identificadores das execuções na explicação.

Resposta = Reexecutar a execução defeituosa não substitui o commit da correção, pois o GitHub Actions utiliza a mesma versão do código. A execução #4, de identificador 45856760745, continuou apresentando erro mesmo após ser reexecutada. Somente após corrigir a pontuação e realizar um novo commit, a execução #5 foi concluída com sucesso.

Observação após a execução = Após reexecutar a execução #4, foi observado que a falha permaneceu. Depois de corrigir o código para somar apenas um ponto por acerto e realizar um novo commit, a execução #5 apresentou sucesso, confirmando que a correção resolveu o problema.

Evidência = Execução com erro:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45856760745

Execução corrigida:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45857290795

Teste reprovado: uma resposta correta soma um ponto.

Esperado (expected): 1

Recebido (actual): 2

Teste reprovado: três respostas corretas somam três pontos.

Esperado (expected): 3

Recebido (actual): 6

Erro apresentado: ERR_ASSERTION.

Registre links das execuções com erro e corrigida. Copie esperado e recebido de pelo menos um teste reprovado.

## Etapa 09 Publicação condicionada e configuração do Pages

### Q17

O que needs: testes exige antes de permitir o job publicar neste workflow? Por que o nome usado é testes?

Resposta = O needs: testes exige que o job de testes seja concluído com sucesso antes de iniciar o job de publicação. O nome testes é utilizado porque corresponde ao identificador do job responsável por executar os testes dentro do workflow.

Observação após a execução = Na execução 45859983698, foi possível observar que o job Testar pontuação foi executado primeiro e, após sua aprovação, o job Publicar quiz foi iniciado e concluído com sucesso.

### Q18

Como a publicação automática deste exercício caracteriza implantação contínua? Como seria uma liberação com decisão humana após os testes?

Resposta = A publicação automática caracteriza implantação contínua porque, após um commit na branch main e a aprovação dos testes, o site é publicado automaticamente, sem necessidade de intervenção humana. Em uma liberação com decisão humana, os testes seriam executados automaticamente, mas a publicação dependeria de uma aprovação manual, caracterizando a entrega contínua.

Observação após a execução = Após realizar o commit que alterou o workflow, os testes foram aprovados e o quiz foi publicado automaticamente no GitHub Pages. Também acessei o endereço do site pelo navegador e confirmei que a página estava disponível, sem necessidade de publicação manual.

Evidência = Commit: f5e5012

Mensagem do commit: Adiciona publicacao condicionada no GitHub Pages

Identificador da execução: 45859983698

Link da execução:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45859983698

Linha que conecta os jobs:

needs: testes

Registre o commit que alterou o workflow e copie a linha que conecta os jobs.

## Etapa 10 Artefato publicado e mudança visível

### Q19

O que o path ./site seleciona para o artefato de publicação? Por que precisamos conferir esse caminho?

Resposta = O path ./site seleciona os arquivos da pasta site para gerar o artefato de publicação. Precisamos conferir esse caminho para garantir que os arquivos corretos sejam publicados e que nenhum arquivo necessário para o funcionamento do site fique de fora.

Observação após a execução = Após a execução do workflow, foi gerado um artefato de publicação com os arquivos da pasta site. A publicação foi concluída com sucesso e o novo título apareceu na página.

### Q20

Que evidências ligam a alteração do título à versão que está no endereço publicado?

Resposta = As evidências são o commit cf25d8c, que registra a alteração do título, a execução 45861045752, que realizou os testes e a publicação com sucesso, e o novo título exibido no endereço publicado. Essas informações demonstram que a alteração realizada no código foi disponibilizada automaticamente no GitHub Pages.

Observação após a execução = Após alterar o título no arquivo index.html e realizar o commit, os testes foram aprovados e o site foi publicado automaticamente. Ao acessar o endereço publicado, confirmei que o título foi atualizado para Quiz Cloud e DevOps - Versão 2.0.

Evidência = Endereço do site:
https://santof-dev.github.io/quiz-devops-aula/

Commit: cf25d8c

Mensagem do commit: Atualiza titulo do quiz para versao 2.0

Link da execução:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45861045752

Novo título publicado: Quiz Cloud e DevOps - Versão 2.0.

Registre o endereço do site, o commit do novo título e o link da execução que publicou essa mudança.

## Etapa 11 Bloqueio de uma versão com defeito

### Q21

A execução com falha retirou o site do ar? Explique o que você observou no endereço publicado e no job publicar.

Resposta = Não. A execução com falha apenas bloqueou a publicação da nova versão. A versão anterior permaneceu disponível no GitHub Pages, pois o job publicar depende da aprovação dos testes para ser executado.

Observação após a execução = Na execução 45863356457, os testes falharam e o job publicar ficou com o status skipped. Ao acessar o endereço publicado, observei que o site continuava disponível, exibindo o título Quiz Cloud e DevOps - Versão 2.0.

### Q22

Como a mesma evidência demonstra integração contínua e controle da implantação? Identifique cada parte do processo.

Resposta = A integração contínua é demonstrada pela execução automática dos testes após cada commit, permitindo identificar erros no código. O controle da implantação é demonstrado pelo bloqueio da publicação quando os testes falham, evitando que uma versão defeituosa seja disponibilizada aos usuários.

Observação após a execução = Na execução 45863356457, os testes foram reprovados e a publicação foi bloqueada. Após corrigir a pontuação e realizar um novo commit, a execução 45863600830 apresentou sucesso nos dois jobs, permitindo novamente a publicação automática do site.

Evidência = Execução com testes reprovados e publicação skipped:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45863356457

Commit da falha: eee7045

Execução da recuperação com ambos os jobs aprovados:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45863600830

Commit da recuperação: 496abdc

Endereço do site:
https://santof-dev.github.io/quiz-devops-aula/

Registre o link da execução com testes reprovados e publicar skipped e o link da recuperação com ambos aprovados.

## Etapa 12 Novo teste e explicação individual do pipeline

### Q23

Integrante = Eduardo Santana dos Santos

Se alguém remover needs: testes do job publicar, a falha de testes ainda garante o bloqueio? Qual configuração você restauraria e como comprovaria a correção?

Resposta = Não. Se a linha needs: testes for removida, o job publicar poderá ser executado independentemente do resultado dos testes. Para corrigir esse problema, restauraria a configuração needs: testes no job publicar. Para comprovar a correção, introduziria uma falha controlada e verificaria se os testes foram reprovados e a publicação ficou com o status skipped.

Observação após a execução = Na execução 45863356457 da etapa anterior, os testes falharam e o job publicar ficou com o status skipped. Na execução final 45864360668, os seis testes foram aprovados e a publicação ocorreu normalmente, demonstrando o funcionamento da dependência entre os jobs.

### Q24

Integrante = Eduardo Santana dos Santos

Escolha um comportamento de uma aplicação que seu grupo poderia construir. Proponha um teste com entrada e resultado esperado e explique em que momento do pipeline ele deve executar.

Resposta = Uma aplicação que o grupo poderia construir seria um quiz educacional com cálculo automático de pontuação. O teste proposto verifica se duas respostas corretas e uma incorreta resultam em dois pontos.

Entrada: respostas [0, 1, 3] e gabarito [0, 1, 2].

Resultado esperado: 2 pontos.

Esse teste deve ser executado no job testes, por meio do comando npm test, antes da publicação da aplicação, evitando que erros na pontuação sejam disponibilizados aos usuários

Observação após a execução = Foi adicionado um sexto teste automatizado para verificar a pontuação de duas respostas corretas. Na execução 45864360668, os seis testes foram aprovados, sem nenhuma falha, e o job publicar foi executado com sucesso.

Evidência = https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45864360668

Logs dos seis testes:
https://github.com/Santof-dev/quiz-devops-aula/actions/runs/45864360668/job/140063238288

Resultado:

tests: 6

pass: 6

fail: 0

skipped: 0

Registre o link da execução final com seis testes e complete as duas respostas individuais com identificação de cada integrante.
