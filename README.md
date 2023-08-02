# Projeto de Webscraping com Atualização da Base de Dados

Este é um projeto de automação de webscraping usando a biblioteca Selenium para buscar informações de cotação de moedas estrangeiras (Dólar, Euro) e ouro em sites específicos. Em seguida, atualizamos uma planilha Excel com as novas cotações e os preços de compra e venda dos produtos comercializados pela empresa fictícia. Feito com base no projeto idealizado pelo Professor João Paulo Rodrigues de Lira da Hashtag Treinamentos.

## Descrição

Nessa empresa fictícia, os produtos comercializados são diretamente influenciados por moedas estrangeiras e ouro, o que requer atualização diária das cotações para definição dos novos preços de venda. O objetivo deste projeto é automatizar esse processo por meio de webscraping e atualização da base de dados.

## Ferramentas Utilizadas

- Python
- Biblioteca Selenium

## Instalação

Antes de executar o projeto, é necessário ter o Python instalado. Além disso, instale a biblioteca Selenium com o seguinte comando:

```
pip install selenium
```

## Passo 1: Acessar Sites e Buscar Cotações

O projeto utiliza o navegador Chrome para acessar os sites que fornecem as cotações necessárias. São eles:
- https://www.google.com.br/ (para cotação do dólar e euro)
- https://www.melhorcambio.com/ouro-hoje (para cotação do ouro)

O código para obter as cotações é o seguinte:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

# Abrir navegador Chrome
navegador = webdriver.Chrome()

# Cotação do Dólar
navegador.get("https://www.google.com.br/")
navegador.find_element(By.XPATH, '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("cotação dólar")
navegador.find_element(By.XPATH, '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys(Keys.ENTER)
cotacao_dolar = navegador.find_element(By.XPATH, '//*[@id="knowledge-currency__updatable-data-column"]/div[1]/div[2]/span[1]').get_attribute("data-value")

# Cotação do Euro
navegador.get("https://www.google.com.br/")
navegador.find_element(By.XPATH, '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("cotação euro")
navegador.find_element(By.XPATH, '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys(Keys.ENTER)
cotacao_euro = navegador.find_element(By.XPATH, '//*[@id="knowledge-currency__updatable-data-column"]/div[1]/div[2]/span[1]').get_attribute("data-value")

# Cotação do Ouro
navegador.get("https://www.melhorcambio.com/ouro-hoje")
cotacao_ouro = navegador.find_element(By.XPATH, '//*[@id="comercial"]').get_attribute("value")
cotacao_ouro = cotacao_ouro.replace(",", ".")

navegador.quit()
```

## Passo 2: Atualizar Base de Dados

A base de dados é importada usando a biblioteca Pandas, e em seguida, as cotações são atualizadas. Com as novas cotações, calculamos os preços de compra e venda dos produtos.

```python
import pandas as pd

# Importar a base de dados
tabela = pd.read_excel("Produtos.xlsx")

# Atualizar cotações
tabela.loc[tabela["Moeda"] == "Dólar", "Cotação"] = float(cotacao_dolar)
tabela.loc[tabela["Moeda"] == "Euro", "Cotação"] = float(cotacao_euro)
tabela.loc[tabela["Moeda"] == "Ouro", "Cotação"] = float(cotacao_ouro)

# Atualizar Preço de Compra
tabela["Preço de Compra"] = tabela["Preço Original"] * tabela["Cotação"]

# Atualizar Preço de Venda
tabela["Preço de Venda"] = tabela["Preço de Compra"] * tabela["Margem"]
```

## Passo 3: Exportar Base de Dados Atualizada

Por fim, a base de dados atualizada é exportada para um novo arquivo Excel.

```python
tabela.to_excel("Produtos Atualizado.xlsx", index=False)
```

## Executando o Projeto

Para executar o projeto, siga os seguintes passos:

1. Certifique-se de ter o Python ou algum IDE de notebook instalado em seu computador.
2. Instale a biblioteca Selenium com o comando `pip install selenium`.
3. Faça o download do arquivo `Produtos.xlsx` com a base de dados original.
4. Execute o código acima para obter as novas cotações e atualizar a base de dados.
5. O arquivo `Produtos Atualizado.xlsx` será gerado com a base de dados atualizada.

## Contribuição

Contribuições são bem-vindas! Se você tiver sugestões de melhorias ou novas funcionalidades para o projeto, sinta-se à vontade para abrir uma issue neste repositório ou enviar um pull request.

## Agradecimentos

Agradecemos ao Professor João Paulo Rodrigues de Lira da Hashtag Treinamentos pelo projeto idealizado e à comunidade de código aberto por tornar disponíveis as bibliotecas utilizadas neste projeto. 

**Desenvolvido com :heart: e Python.**
