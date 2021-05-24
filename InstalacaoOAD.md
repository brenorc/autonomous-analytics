# Instruções de Instalação - Oracle Analytics Desktop

O Oracle Analytics Desktop é o software de Visualização de Dados complementar à solução Oracle Analytics Cloud, que pode ser baixado e instalado por qualquer pessoa que possui uma conta no site Oracle.com

- Acessar [esse link](https://www.oracle.com/solutions/business-analytics/analytics-desktop/oracle-analytics-desktop.html) e fazer o download da versão mais atual do Oracle Data Visualization Desktop (no momento da criação desse guia era a 6.0.0). 

![img](https://i.imgur.com/FCy7RT2.png)

- Será pedido um login e senha para o site oficial da Oracle. Caso você não tenha acesso, clique em **Create Account** e siga os passos para poder ter acesso ao download do software.



![img](https://i.imgur.com/E6ojwPp.png)

 

- Após criar um usuário e realizar login, você encontrará a página abaixo. **Escolha seu sistema operacional (Windows ou MacOS)** e **aceite o Oracle License Agreement**. 

- O próximo passo é clicar diretamente no nome do arquivo (no meu caso, **“V1008856-01.zip”**). 

  

![image-20210524095727991](https://i.imgur.com/7JRBodp.png)



- Se você clicar no botão **Download**, o processo vai ser gerenciado por uma aplicação intermediária. Isso o torna um pouco mais longo, mas pode ser útil caso sua conexão com a internet seja instável e você tenha problemas em baixar um arquivo de quase 900MB diretamente sem interrupções.

- Uma vez terminado o download, abra o arquivo **V1008856-01.zip,** extraia o executável  **Oracle_Analytics_Desktop_6.0.0_Windows.exe** para qualquer pasta no seu computador e execute esse arquivo.

 

![img](https://i.imgur.com/zfhkvwR.png)

 

- Aqui é importante se atentar a um detalhe: O endereço padrão de instalação é **C:\Program Files\Oracle Analytics Desktop\.** Já encontrei casos em que ocorreram erros devido à presença de espaços nesse endereço. Se acontecer algum problema na instalação, tente substituir o endereço para **C:\Oracle\ **ou algo similar.

 

![img](https://i.imgur.com/uTRijce.png)

-  Após o término da instação, é necessário também instalar os pacotes de Machine Learning da solução. A maneira mais simples é executar o seguinte arquivo:

- “C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Oracle\Install DVML”

 

![img](https://i.imgur.com/qz3Jm5J.png)

 

- Também dá para simplesmente buscar por “Install DVML”

 

![img](https://i.imgur.com/zloZOMA.png)

 

- O instalador busca alguns pacotes na Internet baseados em Python e os instala na máquina. Isso possibilita ao usuário utilizar todas as funcionalidades da solução, até mesmo as mais avançadas.

 

![img](https://i.imgur.com/rHbjbZH.png)

 

- Finalizada a instalação do DVML, o Oracle Data Visualization já está completamente configurado para toda e qualquer prática utilizando todas as suas funcionalidades.

 

![img](https://i.imgur.com/mBGkecX.png)
