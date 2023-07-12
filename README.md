<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h1 align="center">Análise de Preço e Mercado de Ecommerce</h1>

  <p align="center">
    Análise dos Dados disponibilizados pela Olist no Kaggle
  </p>
  <p align="center">
    Os dados usados se encontram no <a href="https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=product_category_name_translation.csv">Kaggle</a>.
  </p>
</div>


<!-- TABLE OF CONTENTS -->
<details>
  <summary>Sumário</summary>
  <ol>
    <li>
      <a href="#sobre-o-projeto">Sobre o Projeto</a>
      <ul>
        <li><a href="#ferramentas">Ferramentas</a></li>
      </ul>
    </li>
    <li><a href="#iniciar-o-projeto">Iniciar o Projeto</a></li>
    <li><a href="#case-de-negócio">Case de Negócio</a></li>
    <li><a href="#preparação-dos-dados">Preparação dos Dados</a></li>
    <li>
      <a href="#análise-dos-dados">Análise dos Dados</a>
      <ul>
        <li><a href="#análise-geral-dos-preços">Análise Geral dos Preços</a></li>
        <li><a href="#análise-dos-preços-com-vendas">Análise dos Preços com Vendas</a></li>
        <li><a href="#conclusão-e-recomendação-do-preço">Conclusão e Recomendação do Preço</a></li>
      </ul>  
    </li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- Sobre o Projeto -->
## Sobre o Projeto

Com o objetivo de aprimorar as minhas habilidades em análise de dados para questões financeiras, decidi usar os dados da Olist, referente ao mercado de ecommerce brasileiro de 2016 a 2018.


### Ferramentas

Para realizar este projeto, foi usado as seguintes ferramenta:


* [![Power-BI][Power-BI.pbix]][Power-BI-url]



<!-- Iniciar o Projeto -->
## Iniciar o Projeto

1. Clone este Repositório
   ```sh
   git clone https://github.com/DemikFR/Analise-de-Preco-Mercado.git
   ```


## Case de Negócio
A empresa de e-commerce está interessada em lançar uma nova linha de relógios. Antes de fazer o lançamento, a empresa decidiu conduzir um estudo de mercado e precificação para estabelecer um valor mais lucrativo e competitivo para seus produtos.

Para isso, foi realizada uma análise detalhada do mercado, examinando os preços mais comumente praticados e os produtos mais vendidos. Essa avaliação fornecerá insights valiosos para a definição dos preços da nossa nova linha de relógios.



## Preparação dos Dados
Após importar os dados no Power BI, é crucial estabelecer os relacionamentos entre as tabelas. Essa etapa desempenha um papel fundamental e tem impacto direto nos resultados.

Com base em uma análise e consulta no modelo relacional fornecido pela Olist, foram efetuados os relacionamentos necessários.

![Modelo Relacional no Power BI](https://github.com/DemikFR/Analise-de-Preco-Mercado/assets/102700735/eca546e4-8494-43bb-a982-aaf98d241711)



## Análise dos Dados

Agora que os dados foram modelados e disponibilizados, será possível começar a extrair insights e conhecimento sobre os preços de relógios no mercado em estudo.

Inicialmente, foi realizada uma análise dos preços gerais, identificando os valores mais comuns e as faixas de preço predominantes. Em seguida, foi realizada uma análise com base nas vendas para obter uma compreensão mais aprofundada.

Todos os relatórios foram filtrados com base na Categoria do Produto, usando o filtro "Relógios_presentes" para selecionar os produtos semelhantes àquele que a empresa pretende lançar.


### Análise Geral dos Preços

O objetivo dessa análise é identificar os preços mais populares entre os clientes do mercado. Além disso, foi analisada a distribuição e a média anual das vendas, bem como a tendência do ano seguinte.

Primeiramente, realizou-se uma análise utilizando um gráfico de boxplot com o objetivo de examinar a dispersão dos dados e identificar possíveis valores discrepantes. Além disso, procurou-se obter informações sobre os intervalos de preços nos quais os relógios estão mais concentrados.

![Boxplot](https://github.com/DemikFR/Analise-de-Preco-Mercado/assets/102700735/934dac3c-0104-4664-830c-1ef7c7a84cff)


Durante a análise, foram identificados vários valores discrepantes (outliers) nos preços, variando de menos de 50.000 até mais de 100.000, com uma média de 20.000. A fim de melhorar a precisão da análise, decidiu-se considerar apenas os produtos com preços inferiores a 70.000, uma vez que essa faixa concentra a maior quantidade de produtos e também está alinhada com a linha de produtos que a empresa pretende lançar, cujos valores não ultrapassarão esse limite.

Em seguida, foram calculadas a mediana e a média por meio da fórmula DAX abaixo. Esses cálculos fornecerão insights sobre os valores que despertaram mais interesse no perfil de cliente analisado, permitindo uma melhor compreensão dos seus padrões de preferência.

```julia
mediana_preco = 
CALCULATE(
  MEDIAN(olist_order_items_dataset[price]), olist_order_items_dataset[price] < 70000)

media_preco = 
CALCULATE(
  AVERAGE(olist_order_items_dataset[price]), olist_order_items_dataset[price] < 70000)
```



Por fim, um gráfico de linha foi criado para identificar a média de preços ao longo dos anos, com o objetivo principal de determinar se houve variação ou aumento nos preços dos produtos vendidos. Além disso, esse gráfico permite visualizar tendências e fazer projeções para o próximo ano com base nos dados analisados.

Relatório concluído:

![Relatorio 1](https://github.com/DemikFR/Analise-de-Preco-Mercado/assets/102700735/de01ac9d-af65-4da8-8aa0-a2cffa214e19)



### Análise dos Preços com Vendas

Esta análise tem como objetivo identificar e compreender o comportamento das vendas em relação aos diferentes intervalos de preços, bem como determinar quais faixas de preço são as mais vendidas.

Foi determinado que seriam criadas seis faixas de preços, variando de 1 a 30.000 (sendo que preços acima de 30.000 serão agrupados na faixa "30.000+"), com intervalos de 5.000 entre elas. Para realizar essa tarefa, foi necessário adicionar uma nova coluna que recebe uma fórmula específica, a fim de identificar a qual faixa de preço cada registro pertence. Com a inclusão dessa nova coluna, foi possível gerar um gráfico de barras que apresenta os cinco preços com maior número de produtos vendidos.

```julia
faixa_preco = 
VAR price = olist_order_items_dataset[price]
RETURN
    SWITCH(
        TRUE(),
        INT(price / 5000) = 0, "1-5000",
        INT(price / 5000) = 1, "5001-10000",
        INT(price / 5000) = 2, "10001-15000",
        INT(price / 5000) = 3, "15001-20000",
        INT(price / 5000) = 4, "20001-25000",
        INT(price / 5000) = 5, "25001-30000",
        "30000+"
    )
```

Além disso, a fim de obter uma compreensão mais abrangente dos dados, foram utilizadas duas fórmulas DAX adicionais para determinar a quantidade de valores acima e abaixo de 10 mil dólares (valor base definido durante a análise). 

```julia
% faixa_maior_10_mil = 
DIVIDE(
    CALCULATE(
        COUNT(olist_order_items_dataset[order_id]), 
                olist_order_items_dataset[price] > 10000
        ),
    COUNTROWS(olist_order_items_dataset)
    )

% faixa_menor_10_mil = 
DIVIDE(
    CALCULATE(
        COUNT(olist_order_items_dataset[order_id]), 
                olist_order_items_dataset[price] <= 10000
        ),
    COUNTROWS(olist_order_items_dataset)
    )
```

O relatório concluido:

![Relatorio_2](https://github.com/DemikFR/Analise-de-Preco-Mercado/assets/102700735/d8e4f25a-4611-4836-b9e9-c462c562d978)



### Conclusão e Recomendação do Preço

Para concluir a análise, foi criada uma página final no Power BI, com o intuito de apresentar e comunicar os resultados aos stakeholders envolvidos no projeto. Essa página final serve como um resumo visual das principais descobertas e insights obtidos durante a análise dos dados.

![Relatório 3 - Conclusão](https://github.com/DemikFR/Analise-de-Preco-Mercado/assets/102700735/5f96297a-d49c-4430-8201-7f5dfd31fdda)


Foi criado um arquivo no formato Power Point para realizar a apresentação completa da análise realizada. O objetivo desse arquivo é reunir de forma organizada e visualmente atraente todas as informações relevantes, insights e conclusões obtidas durante o processo de análise de dados.



<!-- LICENSE -->
## License

Distributed under the BSD 2-Clause License. See `LICENSE.txt` for more information.



<!-- CONTACT -->
## Contact

Demik Freitas - [Linkedin](https://www.linkedin.com/in/demik-freitas/) - demik.freitast2d18@gmail.com

Project Link: [https://github.com/DemikFR/Analise-Controle-Zoonoses-SP156](https://github.com/DemikFR/Analise-Controle-Zoonoses-SP156)



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[Python.py]: https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54
[Python-url]: https://www.python.org/
[Power-BI.pbix]: https://img.shields.io/badge/power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black
[Power-BI-url]: https://www.python.org/
