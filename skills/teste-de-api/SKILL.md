---
name: teste-de-api
description: Validar endpoints em status, corpo, autenticacao, limites e divergencia de contrato entre interface e servidor.
---

# Teste de API e contrato

## Quando usar

Endpoint novo ou alterado, suspeita de divergencia de contrato, validacao de autorizacao e limites.

## O que verificar em cada endpoint

| Dimensao | Casos |
|---|---|
| Status | sucesso; entrada invalida (4xx, nao 5xx); nao autenticado; sem permissao; nao encontrado; conflito |
| Corpo | campos esperados presentes; tipos corretos; nenhum campo sensivel exposto |
| Autenticacao | token valido; expirado; ausente; de outro usuario; de perfil insuficiente |
| Limites | tamanho maximo do corpo; texto no comprimento maximo e um acima; numero no limite |
| Idempotencia | mesma operacao duas vezes nao duplica efeito |
| Limite de taxa | rajada de requisicoes devolve o status de excesso com mensagem util |

Erro de validacao devolvendo 500 e defeito: o cliente nao consegue distinguir "corrigi minha
entrada" de "o servidor caiu", e o log de erro fica poluido.

## Contrato: o teste que mais encontra defeito

1. Envie pela interface e leia o registro gravado no banco.
2. Compare campo a campo. Campo enviado que nao chegou e defeito.
3. Verifique nomes divergentes (`uf` x `estado`, `email` x `emailContato`) — o sintoma tipico e
   botao de salvar desabilitado na edicao ou dado que volta ao valor padrao.
4. Verifique se a atualizacao envia **todos** os campos do contrato: atualizacao que omite um
   campo novo o zera em silencio.
5. Mudanca de contrato tem de ser aditiva: o conjunto de campos antigo continua presente. Prove
   isso com teste.

Atencao: serializador configurado para omitir nulos nao devolve a chave. Isso nao e defeito — o
teste deve ler com acesso tolerante, nao exigir a chave.

## Autorizacao por recurso

Teste de acesso indevido exige **duas contas reais**. Para cada endpoint que recebe identificador:
tente ler, listar, alterar e excluir o recurso da outra conta. Inspecao de codigo nao e prova.

Confira tambem a origem da autorizacao: comparar o dono gravado no registro filho em vez do
vinculo canonico faz o sistema negar acesso ao responsavel atual depois de uma transferencia.

## Massa de teste

Crie a massa pelos **proprios fluxos da API** (cadastro, ativacao), nao por insercao manual com
formato adivinhado. Marque com prefixo obviamente descartavel e remova no fim, respeitando a ordem
das dependencias.

Nunca use dado real de cliente. Nunca execute operacao que cobra, envia mensagem a terceiro ou
consome credito pago sem autorizacao explicita.

## Checklist

- [ ] Todos os status mapeados e testados
- [ ] Validacao de entrada devolve 4xx, nao 5xx
- [ ] Nenhum campo sensivel no corpo de resposta
- [ ] Autorizacao testada entre duas contas
- [ ] Limites de tamanho e comprimento testados nas bordas
- [ ] Idempotencia verificada em operacao com efeito
- [ ] Contrato conferido contra o dado gravado
- [ ] Massa criada pelos fluxos e removida no fim

## Pronto quando

Cada endpoint tem caso de sucesso e de falha, o acesso cruzado entre contas e negado, e nenhum
campo enviado se perde no caminho.
