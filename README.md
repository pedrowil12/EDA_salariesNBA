# EDA_salariesNBA
Análise de dados dos jogadores da NBA desde 1990 até atualmente.


# Exploração de dados dos salários da NBA na década de 90 até atualmente.

- Pedro Willian de Paula Ferreira 6ºADS


```python
import pandas as pd
import locale
import matplotlib.pyplot as plt
```


```python
base = pd.read_csv('NBA Salaries(1990-2023).csv')
locale.setlocale(locale.LC_ALL, 'pt_BR.UTF-8')
```




    'pt_BR.UTF-8'




```python
display(base)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>playerName</th>
      <th>seasonStartYear</th>
      <th>salary</th>
      <th>inflationAdjSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Patrick Ewing</td>
      <td>1990</td>
      <td>$4,250,000</td>
      <td>$9,694,547</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Hot Rod Williams</td>
      <td>1990</td>
      <td>$3,785,000</td>
      <td>$8,633,850</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Hakeem Olajuwon</td>
      <td>1990</td>
      <td>$3,175,000</td>
      <td>$7,242,397</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Charles Barkley</td>
      <td>1990</td>
      <td>$2,900,000</td>
      <td>$6,615,103</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Chris Mullin</td>
      <td>1990</td>
      <td>$2,850,000</td>
      <td>$6,501,049</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>15852</th>
      <td>648</td>
      <td>Jaime Echenique</td>
      <td>2021</td>
      <td>$53,176</td>
      <td>$57,993</td>
    </tr>
    <tr>
      <th>15853</th>
      <td>649</td>
      <td>Luca Vildoza</td>
      <td>2021</td>
      <td>$42,789</td>
      <td>$46,665</td>
    </tr>
    <tr>
      <th>15854</th>
      <td>650</td>
      <td>Zavier Simpson</td>
      <td>2021</td>
      <td>$37,223</td>
      <td>$40,595</td>
    </tr>
    <tr>
      <th>15855</th>
      <td>651</td>
      <td>Mfiondu Kabengele</td>
      <td>2021</td>
      <td>$19,186</td>
      <td>$20,924</td>
    </tr>
    <tr>
      <th>15856</th>
      <td>652</td>
      <td>Melvin Frazier</td>
      <td>2021</td>
      <td>$13,294</td>
      <td>$14,498</td>
    </tr>
  </tbody>
</table>
<p>15857 rows × 5 columns</p>
</div>


- Formatação dos dados.


```python
base['salary'] = base['salary'].str.replace('$', '', regex=False).str.replace(',', '', regex=False)
base['inflationAdjSalary'] = base['inflationAdjSalary'].str.replace('$', '', regex=False).str.replace(',', '', regex=False)
```


```python
base['salary'] = base['salary'].astype(int)
base['inflationAdjSalary'] = base['inflationAdjSalary'].astype(int)
```


```python
base['decade'] = base['seasonStartYear'] // 10 * 10
```

- Maiores salários por década.


```python
maioresSalariosDecada = base.loc[base.groupby('decade')['salary'].idxmax()]
```


```python
display(maioresSalariosDecada[['decade', 'playerName', 'salary']])
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>decade</th>
      <th>playerName</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2805</th>
      <td>1990</td>
      <td>Michael Jordan</td>
      <td>33140000</td>
    </tr>
    <tr>
      <th>5547</th>
      <td>2000</td>
      <td>Kevin Garnett</td>
      <td>28000000</td>
    </tr>
    <tr>
      <th>13458</th>
      <td>2010</td>
      <td>Stephen Curry</td>
      <td>40231758</td>
    </tr>
    <tr>
      <th>14551</th>
      <td>2020</td>
      <td>Stephen Curry</td>
      <td>45780966</td>
    </tr>
  </tbody>
</table>
</div>



```python
fig, ax = plt.subplots()
maioresSalariosDecada.plot.line(x='decade', y='salary', ax=ax, marker='o')

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

# Configurar o título e os rótulos dos eixos
plt.xlabel('Década')
plt.ylabel('Salário')
plt.title('Maiores salários de cada década')
plt.tight_layout()
plt.show()
```


    
![png](output_11_0.png)
    


- Menores salários por década.


```python
menoresSalarioDecada = base.loc[base.groupby('decade')['salary'].idxmin()]
```


```python
display(menoresSalarioDecada[['decade', 'playerName', 'salary']])
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>decade</th>
      <th>playerName</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4190</th>
      <td>1990</td>
      <td>Jason Sasser</td>
      <td>2706</td>
    </tr>
    <tr>
      <th>7444</th>
      <td>2000</td>
      <td>Renaldo Major</td>
      <td>20133</td>
    </tr>
    <tr>
      <th>11750</th>
      <td>2010</td>
      <td>John Holland</td>
      <td>9266</td>
    </tr>
    <tr>
      <th>15203</th>
      <td>2020</td>
      <td>Melvin Frazier</td>
      <td>13294</td>
    </tr>
  </tbody>
</table>
</div>



```python
fig, ax = plt.subplots()
menoresSalarioDecada.plot.line(x='decade', y='salary', ax=ax, marker='o')

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

# Configurar o título e os rótulos dos eixos
plt.xlabel('Década')
plt.ylabel('Salário')
plt.title('Menores salários de cada década')
plt.tight_layout()
plt.show()
```


    
![png](output_15_0.png)
    


- Maiores salários ajustados pela inflação por década.


```python
maioresSalariosDecadaInflacao = base.loc[base.groupby('decade')['inflationAdjSalary'].idxmax()]
```


```python
display(maioresSalariosDecadaInflacao[['decade', 'playerName', 'inflationAdjSalary']])
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>decade</th>
      <th>playerName</th>
      <th>inflationAdjSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2805</th>
      <td>1990</td>
      <td>Michael Jordan</td>
      <td>61258556</td>
    </tr>
    <tr>
      <th>5547</th>
      <td>2000</td>
      <td>Kevin Garnett</td>
      <td>45164442</td>
    </tr>
    <tr>
      <th>13458</th>
      <td>2010</td>
      <td>Stephen Curry</td>
      <td>46540848</td>
    </tr>
    <tr>
      <th>14551</th>
      <td>2020</td>
      <td>Stephen Curry</td>
      <td>49928610</td>
    </tr>
  </tbody>
</table>
</div>



```python
fig, ax = plt.subplots()
maioresSalariosDecadaInflacao.plot.line(x='decade', y='inflationAdjSalary', ax=ax, marker='o')

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

# Configurar o título e os rótulos dos eixos
plt.xlabel('Década')
plt.ylabel('Salário')
plt.title('Maiores salários de cada década ajustado por inflação.')
plt.tight_layout()
plt.show()
```


    
![png](output_19_0.png)
    


- Menores salários ajustados pela inflação por década.


```python
menoresSalarioDecadaInflacao = base.loc[base.groupby('decade')['inflationAdjSalary'].idxmin()]
```


```python
display(menoresSalarioDecadaInflacao[['decade', 'playerName', 'inflationAdjSalary']])
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>decade</th>
      <th>playerName</th>
      <th>inflationAdjSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4190</th>
      <td>1990</td>
      <td>Jason Sasser</td>
      <td>4824</td>
    </tr>
    <tr>
      <th>7444</th>
      <td>2000</td>
      <td>Renaldo Major</td>
      <td>29401</td>
    </tr>
    <tr>
      <th>11750</th>
      <td>2010</td>
      <td>John Holland</td>
      <td>11505</td>
    </tr>
    <tr>
      <th>15203</th>
      <td>2020</td>
      <td>Melvin Frazier</td>
      <td>14498</td>
    </tr>
  </tbody>
</table>
</div>



```python
fig, ax = plt.subplots()
menoresSalarioDecadaInflacao.plot.line(x='decade', y='inflationAdjSalary', ax=ax, marker='o')

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

# Configurar o título e os rótulos dos eixos
plt.xlabel('Década')
plt.ylabel('Salário')
plt.title('Menores salários de cada década ajustado por inflação.')
plt.tight_layout()
plt.show()
```


    
![png](output_23_0.png)
    


- 10 maiores salários.


```python
maiores_salarios = base[['playerName', 'salary', 'seasonStartYear']].nlargest(10, 'salary')
display(maiores_salarios)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>playerName</th>
      <th>salary</th>
      <th>seasonStartYear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14551</th>
      <td>Stephen Curry</td>
      <td>45780966</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>15204</th>
      <td>Stephen Curry</td>
      <td>45780966</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>14552</th>
      <td>John Wall</td>
      <td>44310840</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>14553</th>
      <td>James Harden</td>
      <td>44310840</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>15205</th>
      <td>John Wall</td>
      <td>44310840</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>15206</th>
      <td>James Harden</td>
      <td>44310840</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>14554</th>
      <td>Russell Westbrook</td>
      <td>44211146</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>15207</th>
      <td>Russell Westbrook</td>
      <td>44211146</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>13973</th>
      <td>Stephen Curry</td>
      <td>43006362</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>14555</th>
      <td>Kevin Durant</td>
      <td>42018900</td>
      <td>2021</td>
    </tr>
  </tbody>
</table>
</div>



```python
maiores_salarios_chart = base.nlargest(10, 'salary')

fig, ax = plt.subplots(figsize=(10,6))
maiores_salarios_chart.plot.bar(x='playerName', y='salary', ax=ax, rot=45, legend=False, width=0.8)

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

plt.xlabel('Nome do Jogador')
plt.ylabel('Salário')
plt.title('Os 10 jogadores com os maiores salários')
plt.tight_layout()
plt.show()
```


    
![png](output_26_0.png)
    


- 10 menores salários.


```python
menores_salarios = base[['playerName', 'salary', 'seasonStartYear']].nsmallest(10, 'salary')
display(menores_salarios)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>playerName</th>
      <th>salary</th>
      <th>seasonStartYear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4190</th>
      <td>Jason Sasser</td>
      <td>2706</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>4187</th>
      <td>Ira Bowman</td>
      <td>4529</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>4188</th>
      <td>Trevor Winter</td>
      <td>4529</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>4189</th>
      <td>Steve Goodrich</td>
      <td>4529</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>4185</th>
      <td>Andy Panko</td>
      <td>5000</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>4186</th>
      <td>Jonathan Kerner</td>
      <td>5000</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>1938</th>
      <td>Tate George</td>
      <td>5260</td>
      <td>1994</td>
    </tr>
    <tr>
      <th>1937</th>
      <td>Tom Hovasse</td>
      <td>6140</td>
      <td>1994</td>
    </tr>
    <tr>
      <th>4184</th>
      <td>John Morton</td>
      <td>6735</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>1134</th>
      <td>Lorenzo Williams</td>
      <td>7000</td>
      <td>1992</td>
    </tr>
  </tbody>
</table>
</div>



```python
menores_salarios_chart = base.nsmallest(10, 'salary')

fig, ax = plt.subplots(figsize=(10,6))
menores_salarios_chart.plot.bar(x='playerName', y='salary', ax=ax, rot=45, legend=False, width=0.8)

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

plt.xlabel('Nome do Jogador')
plt.ylabel('Salário')
plt.title('Os 10 jogadores com os menores salários')
plt.tight_layout()
plt.show()
```


    
![png](output_29_0.png)
    


- Dez menores salários ajustados pela inflação.


```python
menores_salarios_inflacao = base[['playerName', 'salary', 'inflationAdjSalary']].nsmallest(10, 'inflationAdjSalary')
display(menores_salarios_inflacao)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>playerName</th>
      <th>salary</th>
      <th>inflationAdjSalary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4190</th>
      <td>Jason Sasser</td>
      <td>2706</td>
      <td>4824</td>
    </tr>
    <tr>
      <th>4187</th>
      <td>Ira Bowman</td>
      <td>4529</td>
      <td>8074</td>
    </tr>
    <tr>
      <th>4188</th>
      <td>Trevor Winter</td>
      <td>4529</td>
      <td>8074</td>
    </tr>
    <tr>
      <th>4189</th>
      <td>Steve Goodrich</td>
      <td>4529</td>
      <td>8074</td>
    </tr>
    <tr>
      <th>4185</th>
      <td>Andy Panko</td>
      <td>5000</td>
      <td>8914</td>
    </tr>
    <tr>
      <th>4186</th>
      <td>Jonathan Kerner</td>
      <td>5000</td>
      <td>8914</td>
    </tr>
    <tr>
      <th>1938</th>
      <td>Tate George</td>
      <td>5260</td>
      <td>10531</td>
    </tr>
    <tr>
      <th>11750</th>
      <td>John Holland</td>
      <td>9266</td>
      <td>11505</td>
    </tr>
    <tr>
      <th>4184</th>
      <td>John Morton</td>
      <td>6735</td>
      <td>12007</td>
    </tr>
    <tr>
      <th>1937</th>
      <td>Tom Hovasse</td>
      <td>6140</td>
      <td>12292</td>
    </tr>
  </tbody>
</table>
</div>



```python
menores_salarios_chart_inflacao = base.nsmallest(10, 'inflationAdjSalary')
fig, ax = plt.subplots(figsize=(10,6))
menores_salarios_chart_inflacao.plot.bar(x='playerName', y='inflationAdjSalary', ax=ax, rot=45, legend=False, width=0.8)

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

plt.xlabel('Nome do Jogador')
plt.ylabel('Salário ajustados por inflação')
plt.title('Os 10 jogadores com os menores salários ajustados por inflação.')
plt.tight_layout()
plt.show()
```


    
![png](output_32_0.png)
    


- Dez maiores salários ajustados pela inflação.


```python
maiores_salarios_inflacao = base[['playerName','inflationAdjSalary', 'salary']].nlargest(10, 'inflationAdjSalary')
display(maiores_salarios_inflacao)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>playerName</th>
      <th>inflationAdjSalary</th>
      <th>salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2805</th>
      <td>Michael Jordan</td>
      <td>61258556</td>
      <td>33140000</td>
    </tr>
    <tr>
      <th>2390</th>
      <td>Michael Jordan</td>
      <td>56993066</td>
      <td>30140000</td>
    </tr>
    <tr>
      <th>14551</th>
      <td>Stephen Curry</td>
      <td>49928610</td>
      <td>45780966</td>
    </tr>
    <tr>
      <th>15204</th>
      <td>Stephen Curry</td>
      <td>49928610</td>
      <td>45780966</td>
    </tr>
    <tr>
      <th>13973</th>
      <td>Stephen Curry</td>
      <td>49431367</td>
      <td>43006362</td>
    </tr>
    <tr>
      <th>14552</th>
      <td>John Wall</td>
      <td>48325294</td>
      <td>44310840</td>
    </tr>
    <tr>
      <th>14553</th>
      <td>James Harden</td>
      <td>48325294</td>
      <td>44310840</td>
    </tr>
    <tr>
      <th>15205</th>
      <td>John Wall</td>
      <td>48325294</td>
      <td>44310840</td>
    </tr>
    <tr>
      <th>15206</th>
      <td>James Harden</td>
      <td>48325294</td>
      <td>44310840</td>
    </tr>
    <tr>
      <th>14554</th>
      <td>Russell Westbrook</td>
      <td>48216568</td>
      <td>44211146</td>
    </tr>
  </tbody>
</table>
</div>



```python
maiores_salarios_chart_inflacao = base.nlargest(10, 'inflationAdjSalary')
fig, ax = plt.subplots(figsize=(10,6))
maiores_salarios_chart_inflacao.plot.bar(x='playerName', y='inflationAdjSalary', ax=ax, rot=45, legend=False, width=0.8)

# Desativar a notação científica no eixo y
ax.ticklabel_format(useOffset=False, style='plain', axis='y')

plt.xlabel('Nome do Jogador')
plt.ylabel('Salário ajustados por inflação')
plt.title('Os 10 jogadores com os maiores salários ajustados por inflação.')
plt.tight_layout()
plt.show()
```


    
![png](output_35_0.png)
    


- Media salarial por década.


```python
# Definir opção de formatação de ponto flutuante para duas casas decimais
pd.set_option('display.float_format', lambda x: '%.2f' % x)

# Agrupar o dataframe por década e calcular a média da coluna "salary"
media_salarial_decada = base.groupby('decade')[['salary']].mean()

# Imprimir a média salarial por década
display(media_salarial_decada)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>salary</th>
    </tr>
    <tr>
      <th>decade</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990</th>
      <td>1682438.84</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>3955733.67</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>5117500.03</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>6448383.36</td>
    </tr>
  </tbody>
</table>
</div>


- Média salarial por década ajustados pela inflação.


```python
# Definir opção de formatação de ponto flutuante para duas casas decimais
pd.set_option('display.float_format', lambda x: '%.2f' % x)

# Agrupar o dataframe por década e calcular a média da coluna "inflationAdjSalary"
media_salarial_decada = base.groupby('decade')[['inflationAdjSalary']].mean()

# Imprimir a média salarial ajustado por década
display(media_salarial_decada)
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>inflationAdjSalary</th>
    </tr>
    <tr>
      <th>decade</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990</th>
      <td>3238826.87</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>6010267.01</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>6327877.78</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>7154469.95</td>
    </tr>
  </tbody>
</table>
</div>



```python

```
