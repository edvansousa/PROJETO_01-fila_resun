# IDENTIFICANDO A SITUAÇÃO PROBLEMA
                        
## OBJETIVO

Codificar a fila de um restaurante universitário.

## CONTEXTUALIZAÇÃO

Um restaurante universitário ofere 3 refeições diárias a preços módicos. 
Os interessados necessitam se cadastrar na instituição e comprar créditos que ficam habilitados em seu perfil, 
esses perfis são mantidos num bando de dados pelo restaurante e consultados no momento que o usuário tenta usar o serviço (fazer a refeição).

## SOLUÇÃO IDEAL

- Utilizaremos uma lista linear encadeada para representar a fila de alunos, pois não sabemos quantos alunos farão uso do restaurante naquele turno, sendo assim, uma lista linear sequencial se torna inadequada. 
- Haverá um registro (arquivo binário) das matrículas dos usuários cadastrados e da quantidade de créditos comprados por cada usuário. Esses dados são mantidos pelo restaurante.
- Cada elemento da fila representará um aluno na forma de um registro que constará apenas do seu número de matrícula. 
- O tamanho da fila é estipulado num momento. A fila decrescerá de uma unidade de tamanho a cada estudante atendido. 
- No momento da refeição, será feita uma consulta ao arquivo binário (representando o BD do restaurante). 
- Havendo créditos, o perfil do usuário é atualizado e o número de créditos é decrescido de um. 
- Acaso o número de créditos seja nulo, o usuário fica impedido de usar o restaurante e o próximo na fila é atendido. 
- Acaso não haja fila e haja créditos, o usuário é atendido imediatamente. 
- Acaso haja fila e haja créditos, o usuário entra na fila e espera a sua vez de ser atendido, sendo que o primeiro usuário a chegar é o primeiro a sair.

## OPERAÇÕES

- Adicionar usuários ao final da fila. 
- Adicionar créditos ao perfil dos usuários. 
- Atendimento ao usuário, verificando se ele consta do registro, se ele possuí créditos e permitindo o acesso dele ao restaurante ou não. 
- exclusão do usuário do BD do restaurante, acaso ele tenha se formado. 
