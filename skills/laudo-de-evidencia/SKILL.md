---
name: laudo-de-evidencia
description: Produzir relatorio autossuficiente em HTML com capturas embutidas, resultado por caso e ambiente validado.
---

# Laudo de evidencia

## Quando usar

Fechamento de qualquer ciclo de validacao que precise ser auditavel depois.

## O que e um laudo util

Arquivo **unico**, que abre sem servidor, com capturas embutidas no proprio arquivo. Quem receber
precisa conseguir abrir seis meses depois, sem nenhuma dependencia.

## Estrutura

```
Cabecalho
  titulo, data e hora, ambiente (URL real), versao verificada
Resumo
  total de casos, aprovados, reprovados, pulados, taxa sobre executados
Tabela de casos
  numero | titulo | resultado | tempo | evidencia
Por caso
  passos executados, resultado esperado, resultado obtido, captura embutida
Defeitos
  gravidade, passos de reproducao, esperado, obtido, captura
Pulados
  caso e motivo (insumo ausente)
Rodape
  quem executou, como reproduzir a execucao
```

## Como montar

1. Durante a execucao, grave **uma linha por caso** num arquivo de resultado (uma entrada por
   linha). Nunca acumule em memoria: o executor reinicia o processo apos falha e perde tudo.
2. Guarde as capturas em disco, nomeadas pelo caso.
3. Gere o laudo num passo separado, lendo o arquivo de resultado e embutindo as imagens.
4. Deduplique por nome de caso: nova tentativa grava linha nova.

Gere o laudo apenas depois de uma execucao **completa**. Execucao parcial sobrescreve o resultado.

## Captura de tela

- Capture **depois** de a tela estabilizar, por condicao e nao por atraso fixo.
- Em tela com conteudo carregado de forma assincrona, espere o conteudo aparecer.
- Aba automatizada pode ficar em segundo plano: recurso que depende de permissao ou de aba visivel
  nao funciona ali. Registre isso como limitacao, nao como defeito.
- Capture a area relevante: captura inteira de pagina longa esconde o que importa.

## Honestidade do laudo

- Caso pulado aparece como pulado, com o motivo.
- Caso instavel (passou na segunda tentativa) aparece como instavel, nao como aprovado.
- Verificacao feita apenas por API, sem a interface, e declarada como tal — resposta de servidor
  nao prova que a tela funciona.
- Resposta 200 em aplicacao de pagina unica **nao** prova que a rota existe: o retorno padrao
  devolve a pagina inicial com status 200. Confira o tipo de conteudo e o corpo.

## Checklist

- [ ] Arquivo unico, abre sem servidor
- [ ] Capturas embutidas
- [ ] Ambiente e versao registrados
- [ ] Resultado por caso com esperado e obtido
- [ ] Defeitos com passos de reproducao
- [ ] Pulados e instaveis declarados
- [ ] Instrucao de como reproduzir a execucao

## Pronto quando

Alguem que nao acompanhou a execucao abre o laudo e entende o que foi testado, o que falhou e como
reproduzir.
