# biblioteca-streaming

#include <stdio.h>
#include <string.h>

#define MAX_FILMES 100
#define TAM_MAX 100
#define TAM_SINOPSE 500

// Estrutura dos filmes/séries
typedef struct {
    char titulo[TAM_MAX];
    char autor[TAM_MAX];
    int ano;
    char sinopse[TAM_SINOPSE];
} FilmeSerie;

// chamando as funções
void cadastrarFilmeSerie(FilmeSerie filmes[], int *total);
void listarTitulosPorAno(FilmeSerie filmes[], int total);
void exibirDetalhes(FilmeSerie filmes[], int total, char titulo[]);

int main() {
    FilmeSerie filmes[MAX_FILMES];
    int total = 0;
    int opcao;
    char tituloEscolhido[TAM_MAX];

    do {
        printf("\n--- Biblioteca de Streaming ---\n");
        printf("1. Cadastrar Filme/Série\n");
        printf("2. Listar Títulos por Ano\n");
        printf("3. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        getchar();

        switch (opcao) {
            case 1:
                cadastrarFilmeSerie(filmes, &total);
                break;
            case 2:
                listarTitulosPorAno(filmes, total);
                printf("Digite o título para ver os detalhes (ou pressione Enter para voltar): ");
                scanf(" %[^\n]", tituloEscolhido);
                getchar(); // Limpa o buffer novamente

                if (strlen(tituloEscolhido) > 0) {
                    exibirDetalhes(filmes, total, tituloEscolhido);
                }
                break;
            case 3:
                printf("Encerrando o sistema...\n");
                break;
            default:
                printf("Opção inválida. Tente novamente.\n");
        }
    } while (opcao != 3);

    return 0;
}

// cadastrar um novo filme/série
void cadastrarFilmeSerie(FilmeSerie filmes[], int *total) {
    if (*total >= MAX_FILMES) {
        printf("Limite de filmes/séries atingido.\n");
        return;
    }

    printf("\n--- Cadastro de Filme/Série ---\n");
    printf("Título: ");
    scanf(" %[^\n]", filmes[*total].titulo);
    getchar(); 

    printf("Autor: ");
    scanf(" %[^\n]", filmes[*total].autor);
    getchar();

    printf("Ano: ");
    scanf("%d", &filmes[*total].ano);
    getchar(); 

    printf("Sinopse: ");
    scanf(" %[^\n]", filmes[*total].sinopse);
    getchar();

    (*total)++;
    printf("Filme/Série cadastrado com sucesso!\n");
}

// listar os Filmes/Séries por ano 
void listarTitulosPorAno(FilmeSerie filmes[], int total) {
    if (total == 0) {
        printf("\nNenhum filme/série cadastrado.\n");
        return;
    }

    int anoEscolhido;
    printf("\nDigite o ano para listar os títulos: ");
    scanf("%d", &anoEscolhido);
    getchar();

    printf("\n--- Títulos dos Filmes/Séries de %d ---\n", anoEscolhido);
    int encontrados = 0;

    for (int i = 0; i < total; i++) {
        if (filmes[i].ano == anoEscolhido) {
            printf("%d. %s\n", i + 1, filmes[i].titulo);
            encontrados++;
        }
    }

    if (encontrados == 0) {
        printf("Nenhum título encontrado para o ano %d.\n", anoEscolhido);
    }
}

// exibir os detalhes do filme/série
void exibirDetalhes(FilmeSerie filmes[], int total, char titulo[]) {
    for (int i = 0; i < total; i++) {
        if (strcmp(filmes[i].titulo, titulo) == 0) {
            printf("\n--- Detalhes do Filme/Série ---\n");
            printf("Título: %s\n", filmes[i].titulo);
            printf("Autor: %s\n", filmes[i].autor);
            printf("Ano: %d\n", filmes[i].ano);
            printf("Sinopse: %s\n", filmes[i].sinopse);
            return;
        }
    }
    printf("\nTítulo não encontrado.\n");
}
