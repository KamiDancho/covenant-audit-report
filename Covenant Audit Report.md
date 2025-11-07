# 01_missing_validation_redeem

**Título:** Falta de validação no resgate (Missing validation on redeem)
**Severidade:** Alta
**Status:** Draft

## Resumo
O contrato permite que operações de resgate (redeem/claim) sejam executadas sem validação adequada dos parâmetros e permissões, o que pode levar a resgates não autorizados e perda de fundos.

## Arquivo / Função afetada
- <substituir pelo caminho real do contrato e função>
- Função: redeem / claim

## Descrição
A função de resgate não verifica corretamente se o chamador tem permissão para resgatar ou se os parâmetros estão dentro dos limites esperados. Como resultado, um invasor pode chamar a função com parâmetros manipulados para resgatar tokens/ativos indevidamente.

## Passos para reproduzir
1. Chamar redeem(...) com parâmetros forjados (ex.: destinatário diferente, valores fora de intervalo).
2. Observar que a operação completa sem rejeição e fundos são transferidos.

> Substitua estes passos por um PoC específico assim que o caminho/função real forem identificados.

## Impacto
Resgate não autorizado de fundos, perda de saldo dos usuários, comprometimento financeiro.

## Recomendação / Correção
- Validar msg.sender contra lista de autorizados (ou usar access control adequado).
- Verificar saldos antes de transferir.
- Validar ranges e limites dos parâmetros de entrada.
- Usar modifiers e revisar lógica de transferência/contabilidade.

## PoC (exemplo genérico)
// Exemplo: chamar redeem(to, amount) com to = atacante e amount > 0

## Referências
- [Checklist de auditoria interna]

## Observações
- Prioridade: Alta
- Atualizar o campo "Arquivo / Função afetada" com o caminho real do contrato após busca no repositório.
