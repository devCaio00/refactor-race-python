## TITULO 



# Grupo 

- Analista de qualidade: identifica problemas e acompanha métricas. - Caio
- Desenvolvedor: conduz parte das refatorações. - Marlon
- Responsável por testes: cria e amplia a suíte de testes. - Caio
- Revisor técnico: verifica legibilidade, organização e documentação. - Arthur


# Baseline

- Função Principal: Plataforma de vendas online.

- Principais entradas: customer, items, coupon, state, express

- Principais saídas: Pedido processado para " + customer["name"])
    Subtotal
    Desconto
    Frete
    Imposto
    TOTAL

- quais regras de desconto existem; Desconto por tipo de cliente, Cupom e Desconto máximo permitido

- como o frete é calculado; Calculado apartir do estado do cliente, segue o código:

# frete
    frete = 0

    if subtotal >= 500 and express == False:
        frete = 0
    else:
        if state == "MG" or state == "SP" or state == "RJ" or state == "ES":
            frete = 20 + peso * 0.4
        else:
            frete = 35 + peso * 0.6

        if express == True:
            frete = frete * 1.8


- como os impostos são calculados; São calculados dependendo do estado do cliente, segue código:

# impostos
    taxa = 0

    if state == "MG":
        taxa = 0.07
    elif state == "SP":
        taxa = 0.09
    elif state == "RJ":
        taxa = 0.08
    elif state == "ES":
        taxa = 0.07
    else:
        taxa = 0.12

    imposto = valor_com_desconto * taxa

- como produtos repetidos são identificados: Ele identifica apartir de 2 For, comparando o primeiro indice com o segundo e verificando se é igual.

    duplicados = []

    for i in range(len(items)):
        for j in range(len(items)):
            if i != j:
                if items[i]["name"] == items[j]["name"]:
                    if items[i]["name"] not in duplicados:
                        duplicados.append(items[i]["name"])

    total_final = round(valor_com_desconto + frete + imposto, 2)

    resultado = {
        "customer": customer["name"],
        "subtotal": round(subtotal, 2),
        "discount": round(desconto, 2),
        "shipping": round(frete, 2),
        "tax": round(imposto, 2),
        "total": total_final,
        "points": pontos,
        "duplicate_products": duplicados
    }

    # TESTE DE VALIDAÇÃO CÓDIGO NÃO MODIFICADO
    Os testes passaram e fico top

    tests/test_checkout.py::test_regular_customer PASSED                                                [ 25%]
tests/test_checkout.py::test_vip_customer PASSED                                                    [ 50%]
tests/test_checkout.py::test_coupon PASSED                                                          [ 75%]
tests/test_checkout.py::test_duplicate_products PASSED                                              [100%]

# TESTE DE SEGURANÇA

[100%]
4 passed in 0.01s

# MEDINDO COBERTURA

---------- coverage: platform linux, python 3.12.3-final-0 -----------
Name                     Stmts   Miss  Cover   Missing
------------------------------------------------------
src/legacy_checkout.py      72     12    83%   27, 30, 41, 44, 48, 66, 69, 78-83
------------------------------------------------------
TOTAL                       72     12    83%

# Análise de Compĺexidade

F 6:0 process_order - E (34)

# Análise de Manutenibilidade

src/legacy_checkout.py - A (51.69)

# Análise estatística

SIM102 Use a single `if` statement instead of nested `if` statements
  --> src/legacy_checkout.py:32:13
   |
30 |               desconto = subtotal * 0.20
31 |           else:
32 | /             if customer["type"] == "regular":
33 | |                 if subtotal >= 800:
   | |___________________________________^
34 |                       desconto = subtotal * 0.05
   |
help: Combine `if` statements using `and`

PLR1730 [*] Replace `if` statement with `desconto = min(desconto, subtotal * 0.25)`
  --> src/legacy_checkout.py:47:5
   |
46 |       # desconto máximo permitido
47 | /     if desconto > subtotal * 0.25:
48 | |         desconto = subtotal * 0.25
   | |__________________________________^
49 |
50 |       valor_com_desconto = subtotal - desconto
   |
help: Replace with `desconto = min(desconto, subtotal * 0.25)`
   |
46 |     # desconto máximo permitido
   -     if desconto > subtotal * 0.25:
   -         desconto = subtotal * 0.25
47 +     desconto = min(desconto, subtotal * 0.25)
48 |
   |

SIM102 Use a single `if` statement instead of nested `if` statements
   --> src/legacy_checkout.py:100:13
    |
 98 |       for i in range(len(items)):
 99 |           for j in range(len(items)):
100 | /             if i != j:
101 | |                 if items[i]["name"] == items[j]["name"]:
102 | |                     if items[i]["name"] not in duplicados:
    | |__________________________________________________________^
103 |                           duplicados.append(items[i]["name"])
    |
help: Combine `if` statements using `and`

SIM102 Use a single `if` statement instead of nested `if` statements
   --> src/legacy_checkout.py:101:17
    |
 99 |           for j in range(len(items)):
100 |               if i != j:
101 | /                 if items[i]["name"] == items[j]["name"]:
102 | |                     if items[i]["name"] not in duplicados:
    | |__________________________________________________________^
103 |                           duplicados.append(items[i]["name"])
    |
help: Combine `if` statements using `and`

Found 4 errors.
[*] 1 fixable with the `--fix` option (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
kayy@Prateado:~/Documents/refactor-race-python$ ruff check src/legacy_checkout.py --statistics
3       SIM102  [ ] collapsible-if
1       PLR1730 [*] if-stmt-min-max
Found 4 errors.
[*] 1 fixable with the `--fix` option (2 hidden fixes can be enabled with the `--unsafe-fixes` option).

# 17. REGISTRE A BASELINE
============================================================
Crie um novo arquivo markdown
METRICS.md
Registre:
------------------------------------------------------------
Indicador Antes Depois
------------------------------------------------------------
Testes passando
Cobertura de testes
Complexidade da função principal
Complexidade média
Índice de manutenibilidade
Problemas identificados pelo Ruff
Quantidade de testes
------------------------------------------------------------
Neste momento, preencha apenas os valores da coluna "Antes".

# Identificando problemas

- funções muito grandes;
- responsabilidades demais;
- código duplicado;
- condicionais complexas;
- condicionais excessivamente aninhadas;
- números mágicos;
- strings mágicas;
- nomes pouco descritivos;
- variáveis desnecessárias;
- estado global;
- baixa testabilidade;
- regras de negócio espalhadas;
- baixa coesão;
- alto acoplamento;
- loops desnecessariamente complexos;
- estruturas de dados inadequadas;
- comentários utilizados para explicar código confuso.

## DIAGNÓSTICO INICIAL

1. **Função muito grande e responsabilidades demais (Baixa coesão):** A função `process_order` atua como um "faz-tudo". Ela calcula o subtotal, aplica regras de desconto, calcula frete, impostos, pontos de fidelidade, busca duplicatas, atualiza uma variável global e ainda imprime os resultados no console. Isso fere o Princípio de Responsabilidade Única (SRP).
2. **Código duplicado e variáveis desnecessárias:** O laço de repetição para calcular o subtotal dos itens é escrito duas vezes consecutivas (primeiro para a variável `total1`, que sequer é utilizada depois, e em seguida para a variável `subtotal`).
3. **Estado global:** A modificação da lista `ORDERS_PROCESSED` diretamente dentro da função cria efeitos colaterais indesejados. Isso gera alto acoplamento e resulta em **baixa testabilidade**, pois a função deixa de ser pura.
4. **Condicionais excessivamente aninhadas e complexas:** O aninhamento de múltiplos `else: if` (nas linhas de tipo de cliente) em vez da utilização de `elif`, além de condicionais extensas como `state == "MG" or state == "SP"...`, aumentam drasticamente a complexidade ciclomática do código.
5. **Loops desnecessariamente complexos:** A busca por produtos duplicados utiliza dois laços `for` aninhados acompanhados de três blocos `if`, resultando em uma complexidade de tempo $O(N^2)$. O uso de estruturas de dados mais adequadas, como um `set`, resolveria o problema de forma direta.
6. **Números e strings mágicas:** O código está repleto de valores literais não documentados espalhados pela regra de negócio, como taxas de desconto (`0.15`, `0.20`), impostos (`0.07`, `0.09`), códigos de cupons (`"PROMO10"`) e strings de estados, dificultando futuras manutenções.
7. **Comentários utilizados para explicar código confuso:** A presença do comentário `# procura produtos repetidos de forma bem pouco elegante` evidencia que a estrutura do código não é legível ou expressiva por si só.



