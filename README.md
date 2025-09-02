# ENEM-NORDESTE-2023
![image](https://github.com/user-attachments/assets/6788c5ed-28f8-4ab2-a6c7-9e2b2bed6f81)

Projeto desenvolvido na disciplina IMD1151 - Ciência de Dados.

# Objetivo
- Analisar as desigualdades educacionais na região Nordeste do Brasil usando como base os microdados do ENEM 2023;
- Identificar as combinações de variáveis (sociais, econômicas, regionais, etc.) que mais impactam o desempenho dos participantes;
- Compreender quais fatores estão associados às maiores e menores notas no exame.
  
# Metodologia
O projeto de análise de dados do ENEM foi feito seguindo a metodologia KDD *(Knowledge Discovery in Databases)*. Confira como cada etapa foi aplicada:

### Seleção
A primeira ação de seleção foi filtrarmos a base de dados original para criar um subconjunto focado apenas nos estados da Região Nordeste (df_nordeste), justificando o interesse em analisar as particularidades e desigualdades dessa região. Devido ao grande volume de dados, aplicamos uma amostragem aleatória de 30% (df_amostra) para otimizar o custo computacional sem perder a representatividade estatística.
### Pré-processamento 
Fizemos uma análise de valores faltantes, identificando que as ausências em colunas de notas estavam ligadas à ausência dos participantes nas provas. Com base nisso, criamos o dataframe df_presentes, removendo todos os registros de estudantes que faltaram a um dos dias de prova, pois seus dados de desempenho eram cruciais para a nossa análise.
### Transformação
Criamos novas colunas para enriquecer a análise. A mais importante foi a renda_per_capita, calculada a partir da combinação das questões Q005 (pessoas na residência) e Q006 (renda familiar). Também criamos a coluna MEDIA_OBJETIVAS para facilitar a comparação do desempenho geral dos estudantes nas provas objetivas.
### Mineração de Dados
Aplicamos algumas técnicas de mineração, como agrupamentos (.groupby()) para calcular as médias de notas por estado, tipo de escola e cor/raça. Geramos, também, gráficos de dispersão (.plot.scatter()) para identificar a correlação entre a renda per capita e as notas de Redação, Matemática e Ciências da Natureza, revelando tendências de desempenho. Por fim, fizemos a identificação de "ilhas de excelência" através de um processo de mineração que buscou padrões de municípios com desempenho muito acima da média de seus respectivos estados.
### Interpretação e Avaliação
Todo o código foi preenchido por células de texto que incluem interpretações, os gráficos e tabelas gerados. Por exemplo, ao analisar o ranking de notas, o projeto não apenas mostra o gráfico, mas discute por que o Ceará, apesar da fama, ficou abaixo de outros estados, levantando a hipótese da influência do grande número de participantes. Da mesma forma, as diferenças de desempenho entre escolas públicas e privadas são quantificadas e explicadas.
# Descrição
- Nesta análise, optamos por restringir o escopo geográfico aos dados da região Nordeste do Brasil. Essa escolha se justifica por diversos fatores. Primeiramente, ao limitar a análise a uma única região, conseguimos reduzir a complexidade dos dados e focar em comparações mais consistentes entre os estados que a compõem.
- O Nordeste é a segunda região mais populosa do país e apresenta uma grande diversidade social, econômica e cultural. Essa heterogeneidade interna torna possível identificar padrões, desigualdades e características específicas que poderiam passar despercebidos em uma análise nacional mais ampla.
- Além disso, por ser uma região extensa e com desafios históricos relevantes, o Nordeste oferece um campo fértil para investigações que buscam compreender dinâmicas regionais, o impacto de políticas públicas e disparidades socioeconômicas. Com isso, acreditamos que a análise focada nessa região pode gerar insights significativos e contribuir para uma compreensão mais aprofundada da realidade brasileira.
# Pré-processamento de Dados
O ponto de partida foi o dataset completo dos Microdados do ENEM 2023. A primeira etapa de pré-processamento consistiu em uma filtragem geográfica para isolar apenas os registros da Região Nordeste, utilizando a coluna SG_UF_PROVA. 

![image](https://github.com/user-attachments/assets/aa61de28-4ce9-415d-bc66-98e1bea94165)

Para viabilizar a análise computacional, aplicamos uma amostragem aleatória de 30% sobre o subconjunto do Nordeste, uma técnica que reduziu a dimensionalidade do dataset mantendo, ainda sim, sua representatividade estatística. 

![image](https://github.com/user-attachments/assets/101a020a-821d-4789-9632-d7991c55a036)


A principal decisão de limpeza que tomamos foi a remoção de registros de participantes ausentes. Primeiro, formulamos hipóteses e geramos alguns gráficos para entender o porquê da porcentagem alta de dados faltantes nas varíaveis TX_GABARITO, TX_RESPOSTAS, CO_PROVA e NU_NOTA. Depois, consideramos analisar apenas os registros relativos aos presentes na prova para ver se tinha relação com as taxas supracitadas. Essa ação foi essencial, pois a análise de desempenho de fato dependia diretamente das notas, que estariam nulas para os casos de ausentes na prova.  

A seguir, gráficos de barras considerando os registros relativos aos inscritos presentes e os ausentes e apenas os inscritos presentes na prova, respectivamente:

![image](https://github.com/user-attachments/assets/d128413e-22b0-4c10-8e78-0adeb916ee6f)


![image](https://github.com/user-attachments/assets/8eb67579-83d4-45dd-b263-6184045a9c62)

Durante a análise exploratória, identificamos que as colunas TX_RESPOSTAS (TX_RESPOSTAS_CN, TX_RESPOSTAS_CH, TX_RESPOSTAS_LC e TX_RESPOSTAS_MT) contêm apenas um ID que representa as respostas dadas pelos participantes nas quatro áreas avaliadas no ENEM. Essas informações, por si só, não indicam se as respostas estão corretas, o que limita sua utilidade para análise de desempenho.

Para resolver isso, combinamos cada coluna de respostas com a respectiva coluna de gabarito oficial (TX_GABARITO) e, com o uso de uma função (contar_acertos), calculamos o número total de acertos de cada participante em cada área e de maneira geral na prova. Os resultados foram armazenados em novas colunas: ACERTOS_CN, ACERTOS_CH, ACERTOS_LC, ACERTOS_MTe ACERTOS_GERAL

Essa transformação foi aplicada aos participantes presentes nas provas, e tem como objetivo facilitar a visualização, a comparação e a análise do desempenho individual e coletivo. Com isso, será possível investigar padrões de acerto entre diferentes grupos sociais, regiões ou perfis de estudantes de forma muito mais clara e objetiva.

![image](https://github.com/user-attachments/assets/015f09a2-aec4-4484-b48a-1814a36e59e5)

Também criamos a variável MEDIA_OBJETIVAS com o intuito de facilitar o cálculo das médias entre os estados na criação dos rankings.

![image](https://github.com/user-attachments/assets/d36515b2-fe71-42f9-b048-2cde3e03f7f0)

# Análise
Aqui, iniciamos com uma análise descritiva para entender a distribuição de cada variável chave, como as notas de cada prova, utilizando histogramas e boxplots para visualizar a frequência, média e a presença de outliers. O eixo central da investigação foi a correlação entre a renda_per_capita e o desempenho acadêmico. Cruzamos essa variável com as notas de Redação, Matemática e Ciências da Natureza para identificar tendências e padrões de desigualdade. Também expandimos a análise para investigar o impacto de outras variáveis, como a TP_COR_RACA (raça e etnia) e o TP_DEPENDENCIA_ADM_ESC (tipo de escola), no desempenho geral e por disciplina. Ademais, também realizamos uma análise comparativa entre os estados do Nordeste, criando rankings de desempenho médio e aprofundando a investigação em nível municipal para identificar "ilhas de excelência".
# Principais Insights
Renda como Fator Determinante: Constatamos uma correlação positiva entre a renda familiar per capita e o desempenho dos estudantes, especialmente em Matemática, embora a excelência na Redação tenha se mostrado alcançável por estudantes de diversas faixas de renda. A análise demonstrou uma clara desigualdade de desempenho entre as redes de ensino, seguindo a ordem: Privada > Federal > Estadual. As escolas estaduais, que atendem a maioria dos alunos, apresentaram as menores médias. Identificamos que estudantes autodeclarados brancos obtiveram as maiores médias em todas as áreas, enquanto estudantes indígenas registraram as menores, refletindo uma desigualdade social histórica no acesso à educação de qualidade. A análise municipal revelou que grandes centros urbanos e "ilhas de excelência" , como Natal (RN)  e Eusébio (CE), possuem um desempenho muito superior ao de municípios menores, mesmo dentro do mesmo estado.
# Tecnologias
- Python: Linguagem central do projeto;
- Pandas: Manipulação dos dados, incluindo a leitura do CSV, a filtragem da região Nordeste, a criação de novas colunas e os agrupamentos;
- Matplotlib e Seaborn: Usadas para a criação de todas as visualizações do projeto, desde histogramas e boxplots para a análise univariada até os gráficos de barras e de dispersão que fundamentaram a análise bivariada;
- Google Colab/Jupyter Notebook: O projeto foi desenvolvido no formato de notebook, permitindo uma análise iterativa e a integração de código, visualizações e texto.

