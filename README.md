# leilao
leilao
1. Bug no Registro de Lances
Problema:
O contrato apenas atualiza maiorLicitante e maiorLance, mas não reembolsa o licitante anterior. Isso pode levar à perda de fundos.

Correção:
Antes de atualizar o maior lance, devemos reembolsar o licitante anterior.

2. Problema de Reentrância na Retirada
Problema:
A função retirarLance transfere o valor antes de atualizar o saldo do usuário, permitindo um ataque de reentrância.

Correção:
A atualização do saldo deve ser feita antes da transferência.

3. Falta de Proteção contra Reentrada no Finalizar Leilão
Problema:
Se um contrato malicioso for o maior licitante, ele pode executar um ataque de reentrância quando o leilão for finalizado.

Correção:
Usar o padrão checks-effects-interactions, garantindo que o estado seja atualizado antes da transferência de fundos.
