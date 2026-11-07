---
name: checklist-de-validacao
description: Roteiro completo de verificacao funcional, responsiva, de acesso, de dados e de seguranca antes de aprovar ou reprovar.
---

# Checklist de validacao de entrega

## Quando usar

Toda entrega recebida para validacao, antes de emitir aprovado ou reprovado.

## Roteiro

### 1. Construcao
- [ ] Compila com zero erro
- [ ] Nenhum aviso novo introduzido pela alteracao

### 2. Funcional
- [ ] Faz o que a especificacao diz
- [ ] Cada criterio de aceite verificado individualmente
- [ ] Nenhum marcador, `TODO`, tela vazia ou rota morta no caminho entregue

### 3. Casos de borda
- [ ] Campo obrigatorio vazio
- [ ] Texto no comprimento maximo e um acima
- [ ] Caractere especial e acentuacao
- [ ] Valor numerico zero, negativo, fracionado onde so cabe inteiro
- [ ] Lista vazia e lista com muitos itens
- [ ] Operacao repetida (duplo clique)

### 4. Responsividade
- [ ] 375 px, 768 px e 1440 px
- [ ] Sem rolagem horizontal
- [ ] Alvo de toque com no minimo 44 px em tela pequena
- [ ] Nenhum texto cortado ou sobreposto

### 5. Acesso
- [ ] Rota protegida sem sessao redireciona para o login
- [ ] Perfil sem permissao recebe negativa clara, **sem** ser deslogado
- [ ] Sessao expirada tenta renovar antes de expulsar

### 6. Dados
- [ ] Criar, ler, atualizar e excluir funcionando
- [ ] O que o formulario envia e o que foi gravado conferem
- [ ] Data exibida no fuso correto
- [ ] Exclusao: conferir no banco se foi logica ou fisica, conforme o esperado

### 7. Experiencia
- [ ] Estado de carregamento em toda acao assincrona
- [ ] Estado vazio com orientacao
- [ ] Mensagem de erro util, ao lado do campo, no idioma do usuario
- [ ] Botao desabilitado com motivo visivel

### 8. Seguranca basica
- [ ] Nenhum segredo no pacote servido
- [ ] Nenhum registro de depuracao com dado sensivel
- [ ] Nenhum campo sensivel na resposta da API
- [ ] Cabecalhos de seguranca presentes na resposta

### 9. Ambiente real
- [ ] Fluxo exercitado no ambiente publicado
- [ ] Nenhum erro no console durante o teste
- [ ] Versao servida corresponde a construida

## Veredito

```
Resultado: APROVADO | REPROVADO
Ambiente: <URL testada>
Casos: <executados> executados, <aprovados> aprovados, <pulados> pulados

Defeitos (se reprovado):
- [<gravidade>] <descricao>
  Passos: 1) ... 2) ... 3) ...
  Esperado: ...
  Obtido: ...
  Evidencia: <captura de tela>

Pulados: <caso> — motivo: <insumo ausente>
```

Gravidade: **bloqueante** (impede o uso), **alta** (fluxo central com defeito), **media** (fluxo
secundario), **baixa** (cosmetico).

Defeito bloqueante reprova a entrega inteira, mesmo com todo o resto aprovado.

## Regras

- Nao marcar item sem ter verificado. Checklist preenchido por presuncao e pior que nenhum.
- Caso sem insumo (credencial, chave, dado externo) e **pulado com motivo**, nunca aprovado.
- Taxa de sucesso se calcula sobre os casos executados, e os pulados aparecem no laudo.

## Pronto quando

Todos os itens aplicaveis foram verificados com evidencia, e o veredito tem motivo reproduzivel.
