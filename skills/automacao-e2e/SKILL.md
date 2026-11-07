---
name: automacao-e2e
description: Escrever e manter suite de teste de interface resiliente, com seletor estavel, sessao reutilizada e evidencia por caso.
---

# Automacao de teste ponta a ponta

## Quando usar

Validar fluxo de usuario na interface, montar regressao, provar que uma correcao funciona.

## Seletor: ordem de preferencia

1. Atributo dedicado a teste (`data-testid`)
2. Papel acessivel + nome acessivel (botao chamado "Salvar")
3. Rotulo do campo
4. Texto visivel
5. **Nunca** classe de estilo nem caminho de posicao — quebram na primeira mudanca visual

Campo sem rotulo associado nao tem nome acessivel. Isso e defeito do produto: corrija no produto,
nao contorne no teste.

## Organizacao

- Uma classe por tela com os seletores e as acoes daquela tela. O caso de teste descreve intencao,
  nao clique.
- Preparo compartilhado para sessao autenticada e massa de dados.
- Caso independente: a ordem de execucao nao pode importar.
- Limpeza do que o caso criou, no proprio caso.

## Sessao

Faca **um** login no preparo global e reutilize o estado salvo. Atencao:

- Sistema com sessao unica por usuario derruba logins concorrentes: nao rode duas execucoes em
  paralelo com a mesma conta, nem logue manualmente durante a execucao.
- Estado salvo expira. O auxiliar de navegacao deve detectar a tela de login e refazer a
  autenticacao, em vez de falhar.
- Sessao guardada por aba nao vale para outra aba. Confira onde a aplicacao guarda o token antes de
  injetar.

## Espera

- Espere por **condicao** (elemento visivel, resposta recebida, URL mudou), nunca por tempo fixo.
- Espera por "rede ociosa" nao funciona em tela com consulta periodica ou conexao persistente:
  espere o carregamento da pagina mais a condicao especifica.
- Espera fixa curta esconde lentidao do primeiro acesso: use espera por URL com tempo limite
  generoso.

## Resultado e evidencia

Grave **uma linha por caso em arquivo** (formato de uma entrada por linha) assim que o caso
termina. O executor reinicia o processo depois de um caso que falha e perde tudo o que estava em
memoria — acumular resultado em variavel para montar o relatorio no fim nao sobrevive.

Capture nome da captura de tela a partir do nome do caso, nao de um contador em memoria.

Gere o relatorio **depois** de uma execucao completa: qualquer execucao parcial posterior
sobrescreve o arquivo de resultado.

## Armadilhas

- Elemento "visivel" mas fora da area visivel trava o clique ate o tempo limite: em telas
  estreitas, navegue pelo auxiliar; em barra animada, dispare o evento de clique diretamente.
- Botao desabilitado por formulario invalido trava o clique: confira o estado antes.
- Aviso de consentimento cobre a tela e bloqueia cliques: dispense logo depois do login.
- Requisicao simulada para outra origem precisa responder a verificacao previa com os cabecalhos
  de origem, senao o teste falha sem nenhuma resposta.
- Captura de tela com atraso fixo nao e confiavel para conteudo que carrega de forma assincrona.

## Suite longa

Execucao que passa do tempo limite da ferramenta roda **desacoplada**, gravando um marcador de
conclusao; a espera acontece por verificacao do marcador. Nunca encerre processos por nome parcial:
isso derruba programas do usuario.

## Checklist

- [ ] Seletor estavel, sem classe de estilo
- [ ] Casos independentes da ordem
- [ ] Um login no preparo, estado reutilizado e revalidado
- [ ] Espera por condicao, nunca por tempo fixo
- [ ] Resultado gravado por caso, em arquivo
- [ ] Limpeza da massa criada
- [ ] Teste provado contra o codigo antigo (falha onde deveria)

## Pronto quando

A suite roda duas vezes seguidas com o mesmo resultado, cada caso tem evidencia, e um defeito
reintroduzido faz a suite falhar.
