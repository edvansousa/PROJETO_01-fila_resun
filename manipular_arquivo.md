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

void append(); // implementado
void display(); //implementado
void displayAll(); //implementado
void modify(); //implementado
void del();//implementado
void search();//implmentado
void rname(); //implementado
void rremove(); //implementado
char mygetch(); //implementado

/*Arquivo binário com dados dos alunos utilizado com DB no resun*/
char fnome[]={"BD_resun.dat"};

int main()
{
	int ch; //variável auxiliar

	while(1)
	{
		system("clear"); //clrscr();

		printf("==================Sistema de administração de alunos=============\n\n");

		//Operações possíveis no arquivo.

		printf("1. Incluir\n\n"); //append(): incluir registro no arquivo;

		printf("2. Modificar\n\n"); //modify(): modificar dados dos alunos registrados no arquivo;

		printf("3. Deletar\n\n"); //del(): deletar registro de alunos no arquivo;

		printf("4. Buscar\n\n"); //search(): buscar registro no arquivo;

		printf("5. Exibir\n\n"); //display(): exibir um único registro do arquivo;

		printf("6. Exibir Todos\n\n"); //displayAll() : exibir todos os registros do arquivo;

		printf("7. Renomear\n\n"); //rname(): renomear arquivo;

		printf("8. Excluir Arquivo\n\n"); //rremove() : file remove;

		printf("0. Sair\n\n");

		printf("================================================================\n\n");

			printf("\nQual operação gostaria de executar ?: ");
			scanf("%d",&ch);

			switch(ch)
			{
				case 1: append(); // incluir aluno no arquivo.
				break;

				case 2: modify(); //Modificar o arquivo ...
				break;

				case 3: del(); // Deletar registro do arquivo
				break;

				case 4: search(); // buscar registro no arquivo
				break;

				case 5: display(); //Exibir um registro do arquivo
				break;

				case 6: displayAll(); //Exibir todos os registros do arquivo
				break;

				case 7:	rname(); //renomear arquivo
				break;

				case 8:	rremove(); //remover arquivo
				break;

				case 0: exit(0);
			}

			mygetch();
		}
	return 0;
}

//declaração das funções para manipulação do arquivo

void append()
/*Insere registro na estrutura aluno*/
{
	FILE *fp; //cria arquivo
	struct aluno t1; //Cria nova estrutura

	fp = fopen(fnome,"ab"); // Abre aqrquivo

	printf("\nEntre com a Matricula do aluno: "); /*Atribui valor a matricula*/
	scanf("%d",&t1.Matricula);

	printf("\nEntre com o nome do aluno: ");/*Atribui o nome*/
	scanf("%s",t1.Nome);

	printf("\nEntre com a quantidade de créditos para o resun:");/*Atribui a quantidade de créditos inicial*/
	scanf("%d",&t1.Creditos);

	printf("\nA matrícula do aluno é ativa?"); /*Atribui o status da matrícula do aluno*/
	scanf("%d", &t1.MatriculaAtiva);

	fwrite(&t1,sizeof(t1),1,fp); // escreve no arquivo

	fclose(fp); //fecha arquivo
}

void rname()
/*Método renomear arquivo (opcional)*/
{
	char Nome[20]; //string auxiliar

	printf("\nEntre com o nome do arquivo: ");
	fflush(stdin); // limpar buffer de entrada.
	scanf("%[^\n]",Nome); //atribui a variável name a entrada de usuário.

	rename(fnome,Nome); // função que renomea o arquivo antigo com um novo nome

	strcpy(fnome,Nome); //copia uma string.
}

void rremove()
/* Remove o arquivo, podendo realizar o backup opcional do arquivo*/
{
	FILE *fp,*fp1; // cria duas variáveis ponteiro do tipo FILE;
	struct aluno t; // Cria registro aluno t;

	char nome[20]; //string auxiliar;
	char val[20]; //string auxiliar;

	printf("\nDeseja realizar o backup do arquivo?: (S/N):");
	scanf("%s",val);

	if(strcmp(val, "S") == 0)
	{

		printf("\nEntre com o nome do arquivo: "); //solicita do usuário o nome do arquivo
		fflush(stdin); // limpa o buffer de saída
		scanf("%[^\n]", nome); //captura nome

		fp = fopen(nome, "wb"); //abre arquivo para escrita
		fp1 = fopen(fnome, "rb"); //abre arquivo para leitura

		while(1)
		{
			fread(&t, sizeof(t), 1, fp1); //lê os objetos no arquivo

			if(feof(fp1)) //se está no fim do arquivo
			{
				break;
			}
			fwrite(&t, sizeof(t), 1, fp);//escreve no fim do arquivo
		}

		//encerra arquivos
		fclose(fp);
		fclose(fp1);

		remove(fnome); // função que remove um arquivo

		strcpy(fnome, nome); //copia strings: nome para fnome
	}
	else
	{
		remove(fnome); //remove arquivo
	}
}

void modify()
/*Modifica os campos créditos e status de matricula do registro aluno no arquivo. */
//variável int count e struct aluno t1;
{
	FILE *fp,*fp1; //cria 2 arquivos
	struct aluno t; // cria 2 registros aluno
	int Matricula, found = 0; //variáveis auxiliares

	fp=fopen(fnome, "rb"); //abre arquivo para leitura
	fp1=fopen("temp.dat", "wb"); //abre arquivo temporário para escrita

	printf("\entre com a matricula do aluno que gostaria de modificar: ");
	scanf("%d", &Matricula); //lê matrícula do usuário

	while(1)
	{
		fread(&t, sizeof(t), 1, fp);

		if(feof(fp))//se chegou ao fim do arquivo
		{
			break;
		}
		if(t.Matricula == Matricula) //compara matricula procurada com matricula no registro
		{
			found = 1; // indica que foi encontrado

			fflush(stdin);

			printf("\nEntre com a nova quantidade de créditos: ");
			scanf("%d", &t.Creditos);

			printf("\nEntre com o status de matricula do aluno: Ativa = 1, Inativa = 0");
			scanf("%d",&t.MatriculaAtiva);

			fwrite(&t, sizeof(t), 1, fp1);
		}
		else
		{
			fwrite(&t, sizeof(t), 1, fp1);
		}
	} // fim while

	//fecha arquivos
	fclose(fp);
	fclose(fp1);

	if(found == 0)
	/*matricula não foi encontrada*/
	{
		printf("Registro não encontrado\n\n");
	}
	else
	{
		//Abre arquivos para escrita e leitura, respectivamente
		fp = fopen(fnome, "wb");
		fp1 = fopen("temp.dat", "rb"); //arquivo temporário fp1

		while(1)
		{
			//lê arquivo
			fread(&t, sizeof(t), 1, fp1);

			if(feof(fp1))
			// Chegou ao fim do arquivo?
			{
				break;
			}
			fwrite(&t, sizeof(t), 1, fp); //escreve no arquivo
		}

	}
	//Fecha arquivos
	fclose(fp);
	fclose(fp1);
}

void del()
/*Deleta registro de um aluno no arquivo..*/
{
	FILE *fp, *fp1;
	struct aluno t;
	int Matricula, found = 0;

	fp = fopen(fnome, "rb"); //arquivo original para leitura
	fp1 = fopen("temp.dat", "wb"); //arquivo temporário para escrita

	printf("\nEntre com a matricula do aluno que você quer deletar:");
	scanf("%d", &Matricula);

	while(1)
	{
		fread(&t, sizeof(t), 1, fp);

		if(feof(fp))
		{
			break;
		}
		if(t.Matricula == Matricula)
		{
			found=1;
		}
		else
		{
			fwrite(&t, sizeof(t), 1, fp1);
		}
	}
	fclose(fp);
	fclose(fp1);

	if(found == 0)
	{
		printf("Registro não encontrado\n\n");
	}
	else
	{
		//abrir os arquivos novamente para escrita e leitura
		fp = fopen(fnome, "wb");
		fp1 = fopen("temp.dat", "rb");

		while(1)
		{
			fread(&t, sizeof(t), 1, fp1);

			if(feof(fp1))
			{
				break;
			}
			fwrite(&t, sizeof(t), 1, fp);
		}
	}
	fclose(fp);
	fclose(fp1);
}


void display()
/*Método que exibe um registro do arquivo a partir de sua matrícula*/
{
	FILE *fp;
	struct aluno t;
	int Matricula, found = 0;

	fp = fopen(fnome, "rb");

	printf("\nEntre com a matrícula do aluno:");
	scanf("%d", &Matricula);

	while(1)
	{
		fread(&t, sizeof(t), 1, fp);

		if(feof(fp))
		{
			break;
		}
		if(t.Matricula == Matricula)
		{
			found = 1;

			printf("\n========================================================\n\n");
			printf("\t\t Informações do aluno: %d\n\n", t.Matricula);
			printf("==========================================================\n\n");

			printf("Nome\tMatricula Ativa\tCréditos disponíveis\n\n");

			printf("%s\t", t.Nome);
			printf("%d\t\n\n", t.MatriculaAtiva);
			printf("%d\t\n\n", t.Creditos);

			printf("=========================================================\n\n");
		}
	}
	if(found == 0)
	{
		printf("\nRegistro não encontrado");
	}
	fclose(fp);
}

void search()
/*Busca aluno a partir de */
{
	FILE *fp;
	struct aluno t;

	int found = 0;
	char Nome[20];

	fp = fopen(fnome, "rb");

	printf("\nEntre com o nome do aluno:");
	scanf("%s", Nome);

	while(1)
	{
		fread(&t, sizeof(t), 1, fp);

		if(feof(fp))
		{
			break;
		}
		if(strcmp(Nome, t.Nome) == 0)
		{
			printf("\n========================================================\n\n");
			printf("\t\t Informações sobre o aluno %d\n\n",t.Matricula);
			printf("==========================================================\n\n");

			printf("Nome\tCréditos disponíveis\n\n");

			printf("%s\t",t.Nome);
			printf("%d\t\n\n",t.Creditos);

			printf("==========================================================\n\n");

		}
	}
	if(found == 0)
	{
		printf("\nRegistro não encontrado");
	}
	fclose(fp);
}

void displayAll()
/*Mostra todos os registros no arquivo BD*/
{
	FILE *fp;
	struct aluno t;

	fp = fopen(fnome, "rb");

	printf("\n========================================================\n\n");
	printf("\t\t Informações sobre todos os alunos\n\n");
	printf("==========================================================\n\n");

	printf("Matrícula\tNome\tStatus de matrícula\tCréditos\n\n");

	while(1)
	{
		fread(&t, sizeof(t), 1, fp);

		if(feof(fp))
		{
			break;
		}
		printf("%d\t", t.Matricula);
		printf("%s\t", t.Nome);
		printf("%d\t\n\n", t.MatriculaAtiva);
		printf("%d\t\n\n", t.Creditos);

	}
	printf("========================================================\n\n");

	fclose(fp);
}

char mygetch()
{
	char val;
	char rel;

	scanf("%c", &val);
	scanf("%c", &rel);
	return (val);
}


```
