# Atividade - Lógica e Algoritmos: Controle de Acesso

## Descrição do Problema

O objetivo desta atividade é desenvolver um algoritmo capaz de controlar o acesso de alunos a uma sala de aula, com base em uma lista oficial de presença.

Para cada aluno na fila de entrada, o sistema deve:

- Verificar se o nome do aluno está na lista oficial
- Permitir a entrada caso esteja presente
- Negar a entrada caso não esteja
- Repetir o processo até que todos os alunos da fila sejam verificados

---

## Abordagem Utilizada

A solução foi construída com base em três pilares fundamentais da lógica de programação:

### 1. Estrutura de Dados (Vetor)

Foi utilizado um vetor (`listaAlunos`) para armazenar os nomes dos alunos autorizados.  
Isso permite simular uma lista fixa e realizar buscas sobre ela.

### 2. Estruturas de Repetição

- Um laço `enquanto` controla o processamento da fila de alunos  
- Um laço `para` percorre o vetor em busca do nome informado  

### 3. Estruturas Condicionais

- Um `se` interno verifica se o nome digitado corresponde a algum elemento do vetor  
- Um `se` externo decide se a entrada será permitida ou negada  

### Estratégia de Busca

O algoritmo implementa uma **busca sequencial (linear search)**, comparando o nome informado com cada elemento do vetor até encontrar uma correspondência ou esgotar a lista.

---

## Pseudocódigo (Portugol)

```portugol
Algoritmo "controle_acesso_alunos"

Var
   quantidadeAlunos : inteiro
   nomeAluno : caractere
   foiEncontrado : logico
   listaAlunos : vetor[0..2] de caractere
   i : inteiro

Inicio

   listaAlunos[0] <- "Joao"
   listaAlunos[1] <- "Maria"
   listaAlunos[2] <- "Ana"

   escreva("Digite a quantidade de alunos na fila: ")
   leia(quantidadeAlunos)

   enquanto quantidadeAlunos > 0 faca

      escreva("Digite o nome do aluno: ")
      leia(nomeAluno)

      foiEncontrado <- falso

      para i de 0 ate 2 faca
         se nomeAluno = listaAlunos[i] entao
            foiEncontrado <- verdadeiro
         fimse
      fimpara

      se foiEncontrado entao
         escreva("Entrada permitida")
      senao
         escreva("Entrada negada")
      fimse

      quantidadeAlunos <- quantidadeAlunos - 1

   fimenquanto

Fimalgoritmo
```
<img width="1700" height="2200" alt="fluxograma-1" src="https://github.com/user-attachments/assets/6c41e286-13ad-45ba-b059-48382606916b" />
