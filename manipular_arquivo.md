```c

#include<stdio.h>
#include<stdlib.h>
#include<string.h>

struct aluno
{
/*
* Registro do restaurante contendo informações dos alunos.
* Nome do aluno, Matricula do aluno e Creditos (créditos para refeição)
* e MatriculaAtiva (Se o aluno ainda está matriculado ou não)
*/
	char Nome[20];
	int Matricula;//identificador utilizado nas operações de busca do aluno
	int Creditos;
	int MatriculaAtiva;
};

```
