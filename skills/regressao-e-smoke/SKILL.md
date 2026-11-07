---
name: regressao-e-smoke
description: Manter duas suites complementares: verificacao rapida pos-publicacao e regressao completa pre-entrega.
---

# Regressao e verificacao rapida

## Quando usar

Depois de toda publicacao (rapida) e antes de toda entrega relevante (completa).

## Duas suites

### Verificacao rapida — menos de 2 minutos

Roda a cada publicacao. Cobre apenas o que, se quebrar, significa indisponibilidade:

- [ ] Pagina inicial responde
- [ ] Login funciona
- [ ] Navegacao principal abre as telas
- [ ] Uma leitura e uma escrita no fluxo central
- [ ] Verificacao de saude da API responde
- [ ] Nenhum erro no console

Falha aqui interrompe a publicacao e dispara reversao.

### Regressao completa — antes da entrega

Cobre todos os fluxos criticos, todos os perfis de acesso, casos de borda conhecidos e todo defeito
corrigido no passado (defeito corrigido vira caso permanente de regressao).

## Regra de ouro do caso de regressao

Todo defeito corrigido gera um caso que **falha** contra o codigo antigo. Antes de incorporar o
caso, prove: volte a versao anterior do trecho corrigido e veja o caso falhar. Caso que passa nas
duas versoes nao testa nada.

## Comparacao visual

Util para detectar quebra de layout, exige disciplina:

- Tolerancia configurada: fonte e animacao causam diferenca minima legitima.
- Imagem de referencia versionada e atualizada **conscientemente**, com revisao da diferenca.
- Mascarar regiao com conteudo variavel (data, contador, imagem aleatoria).

## Verificacao de desempenho

Mantenha uma linha de base dos tempos do fluxo principal e compare a cada entrega. Degradacao de
mais de 30% em relacao a linha de base e defeito, mesmo dentro do limite aceitavel.

Em tela lenta apenas dentro da suite (rapida quando aberta isolada), suspeite de limite de taxa
por origem: aumente o tempo limite e registre a suspeita — nao afirme a causa sem medir.

## Varredura responsiva automatizada

Vale mais que inspecao manual: percorra todas as paginas publicas em tres larguras verificando
rolagem horizontal, texto sob cabecalho fixo, botao deformado e elemento circular com largura
diferente da altura. Guarde o roteiro e rode a cada mudanca de estilo.

## Manutencao da suite

- Caso instavel e defeito da suite: conserte ou remova. Caso que falha as vezes treina a equipe a
  ignorar falha.
- Caso lento sem valor proporcional sai da verificacao rapida e vai para a completa.
- Seletor quebrado por mudanca de interface se conserta no mesmo dia.

## Checklist

- [ ] Verificacao rapida abaixo de 2 minutos, rodando a cada publicacao
- [ ] Regressao completa cobrindo fluxos criticos e todos os defeitos passados
- [ ] Todo caso novo provado contra o codigo antigo
- [ ] Comparacao visual com tolerancia e regiao variavel mascarada
- [ ] Linha de base de desempenho registrada
- [ ] Nenhum caso instavel tolerado

## Pronto quando

A verificacao rapida roda a cada publicacao e barra indisponibilidade, e a regressao completa passa
duas vezes seguidas com o mesmo resultado.
