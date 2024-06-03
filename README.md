# TDE-SISTEMAS-OPERACIONAIS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ARQUIVOS 100
#define TAM_CONTEUDO 1000

typedef struct {
    char nome[50];
    char conteudo[TAM_CONTEUDO];
} Arquivo;

typedef struct {
    Arquivo arquivos[MAX_ARQUIVOS];
    int num_arquivos;
} SistemaArquivos;

void criar_arquivo(SistemaArquivos *sistema, char *nome) {
    if (sistema->num_arquivos >= MAX_ARQUIVOS) {
        printf("Erro: Não é possível criar mais arquivos. Limite atingido.\n");
        return;
    }
    for (int i = 0; i < sistema->num_arquivos; i++) {
        if (strcmp(sistema->arquivos[i].nome, nome) == 0) {
            printf("Erro: Já existe um arquivo com esse nome.\n");
            return;
        }
    }
    strcpy(sistema->arquivos[sistema->num_arquivos].nome, nome);
    sistema->num_arquivos++;
    printf("Arquivo '%s' criado com sucesso.\n", nome);
}

void escrever_arquivo(SistemaArquivos *sistema, char *nome, char *conteudo) {
    for (int i = 0; i < sistema->num_arquivos; i++) {
        if (strcmp(sistema->arquivos[i].nome, nome) == 0) {
            strcpy(sistema->arquivos[i].conteudo, conteudo);
            printf("Conteúdo do arquivo '%s' atualizado.\n", nome);
            return;
        }
    }
    printf("Erro: Arquivo não encontrado.\n");
}

void ler_arquivo(SistemaArquivos *sistema, char *nome) {
    for (int i = 0; i < sistema->num_arquivos; i++) {
        if (strcmp(sistema->arquivos[i].nome, nome) == 0) {
            printf("Conteúdo do arquivo '%s':\n%s\n", nome, sistema->arquivos[i].conteudo);
            return;
        }
    }
    printf("Erro: Arquivo não encontrado.\n");
}

void excluir_arquivo(SistemaArquivos *sistema, char *nome) {
    int encontrado = 0;
    for (int i = 0; i < sistema->num_arquivos; i++) {
        if (strcmp(sistema->arquivos[i].nome, nome) == 0) {
            encontrado = 1;
            for (int j = i; j < sistema->num_arquivos - 1; j++) {
                strcpy(sistema->arquivos[j].nome, sistema->arquivos[j + 1].nome);
                strcpy(sistema->arquivos[j].conteudo, sistema->arquivos[j + 1].conteudo);
            }
            sistema->num_arquivos--;
            printf("Arquivo '%s' excluído com sucesso.\n", nome);
            break;
        }
    }
    if (!encontrado)
        printf("Erro: Arquivo não encontrado.\n");
}

int main() {
    SistemaArquivos sistema;
    sistema.num_arquivos = 0;

    criar_arquivo(&sistema, "documento.txt");
    escrever_arquivo(&sistema, "documento.txt", "Conteúdo do documento.");
    ler_arquivo(&sistema, "documento.txt");

    criar_arquivo(&sistema, "outro_documento.txt");
    escrever_arquivo(&sistema, "outro_documento.txt", "Outro conteúdo.");
    ler_arquivo(&sistema, "outro_documento.txt");

    excluir_arquivo(&sistema, "documento.txt");
    ler_arquivo(&sistema, "documento.txt");

    return 0;

    #CODIGO REFERENTE A ESTRUTURA DE ARMAZENAMENTO EM C
