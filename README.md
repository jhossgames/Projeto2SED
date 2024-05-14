# Projeto Sincronização de semáforos (Onda Verde)

## Descrição

Este projeto busca desenvolver um sistema de onda verde de semáforos gerencie o fluxo de veículos, minimizando o tempo de espera e evitando congestionamentos no software UPPAAL, uma ferramenta para modelagem e análise de funções de controle de eventos discretos com base em modelos baseados em tempo. Como prioridade fundamental, o sistema foi pensado para garantir que , no trecho simulado, o usuário pare no máximo uma vez. 

![Visão Geral](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/GFN%20.png?raw=true)

## Funcionalidades

- Controle de quatro semáforos de veículos.
- Sincronismo no sistema entre os semáforos para evitar paradas excessivas.
- Modelagem da via, onde o carro pode percorrer o trecho e chegar ao final do trajeto.

## Topologia do Cruzamento

![Topologia do Cruzamento](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/Grafico.png?raw=true)

  Para modelagem do sistema, primeiramente foi desenvolvido o diagrama acima, relacionamos os estados de cada semáforo independente com os estados, a fim de minimizar erros durante o desenvolvimento. Para o sincronismo do sistema foi utilizado o método gráfico descrito no Manual Brasileiro de Sinalização de Trânsito - Volume V. No qual apresenta como determinar os tempos e os ajustes para uma via de mão única. Na ausência de uma ferramenta para desenvolvimento específico do gráfico descrito acima, optou-se por desenvolvêlo em um software CAD, com as devidas adaptações.
  Analisando o gráfico, podemos ver que os semáforos 2 e 4, 1 e 3 correspondem aos mesmos estados em função do tempo. Além disso, observa-se também que tanto o carro que chegará ao semáforo assim que o mesmo abrir cumprirá todo o percurso sem paradas. Também podemos ver que o mesmo fenômeno acontece para o veículo que chega ao final do sinal verde e início do amarelo, ao manter a velocidade de 40 km/h da via é observada a onda verde.
  Assim, podemos descrever as transições para a síntese do controlador:
  - Estágio 1-2: Manutenção do estado verde para os semáforos 1 e 3, mudança do amarelo para o vermelho nos semáforos 2 e 4;
  - Estágio 2-3: Mudança do estado verde para amarelo nos semáforos 1 e 3, mudança do vermelho para o verde nos semáforos 2 e 4;
  - Estágio 3-4: Mudança do estado amarelo para o vermelho nos semáforos 1 e 3, manutenção do estado verde nos semáforos 2 e 4;
  - Estágio 4-1: Mudança do estado vermelho para o verde nos semáforos 1 e 3, mudança do estado verde para amarelo nos semáforos 2 e 4.
    
  Podemos extrair do gráfico também os tempos de cada transição, assim tempos:
  - Estágio 1-2: 5s;
  - Estágio 2-3: 26,5s;
  - Estágio 3-4: 5s;
  - Estágio 4-1: 26,5s.
      
## Construção dos Autômatos

 Com a topologia e os eventos definidos podemos utilizar o software UPPAAL para construção dos autômatos de acordo com o funcionamento estabelecido. Criado um novo projeto e adicionando os eventos, podemos criar as 4 templates necessários. Assim temos: 
 
 Autômato do semáforo 1 e 3:
 ![automato S1](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/S1.png?raw=true)
 
 
 Autômato do semáforo 2 e 4:
 ![automato S2](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/S2.png?raw=true)
 
 Controlador dos semáforos:
  ![Controlador dos semáforos](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/controlador.png?raw=true)
  
 Modelo da via:
 ![automato Carro](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/ViaN.png?raw=true)

 Após isso, podemos declarar nossas variáveis Globais:
  ![Variáveis](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/variaveis.png?raw=true)

 Finalmente, podemos declarar nossos automatos e criar o sistema:
   ![Declaração do sistema](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/sistema.png?raw=true)


## Análise com Verifier com lógica temporal
  Além da utilização do simulador simbólico para o desenvolvimento e ajuste dos atributos, uma importante ferramenta do UPPAAL é o uso de lógica temporal para validar condições. Uma vez que com poucos estados um debug ou bugfix pode ser relativamente rápido, com o aumento dos estados e variáveis se faz necessário o uso de uma ferramenta como essa.
  Foram aplicadas as seguintes equações ao modelo:
  - E<> Stop==0;
  - E<> Stop==1;
  - E<> Stop>1;
  - A[] not deadlock;
  - E<>  carro1.Chegada;
  - E<>  carro1.Parado1;
  - E<>  sema1.Verde and sema3.Verde;
  - E<>  sema2.Verde and sema4.Verde;
  - E<>  sema1.Verde and sema2.Verde;
  - E<>  sema1.Verde and carro1.Parado1.

Assim, obtemos o seguinte resultado:
  
  Com todos os template executados executadas e com o correto funcionamento, podemos utilizar o software para criar o supervisor de forma automatizada. Assim temos:
  
![Verifier](https://github.com/jhossgames/Projeto2SED/blob/main/Imagens/Verifier.png?raw=true)

  Podemos utilizar o Software para a simulação sistema, constatando-se o correto funcionamento do sistemas de semáforos, o correto sincronismo e as verificações da lógica temporal.
  Para a simulação, basta abrir o arquivo ZZZ no software UPPAAL, caso seu usuário do sistem operacional tenha carateres do UNICODE, você deve baixar a versão BETA do software.
  Para visualizar a simulação do sistema:

  [![Veja a simulação](https://i.imgur.com/XllV2DE.png)](https://youtu.be/ni8EY2Vam8o)

## Conclusão
  Podemos concluir que a construção do sistema para o controle de um uma via de 4 semáforos através do software UPPAAL, foi concluido com sucesso. Permitindo o tráfego seguro e eficiente para os usuários que precisam passar por esse trecho todod os dias.
## Licença

Este projeto está licenciado sob a [Licença MIT](LICENSE). Consulte o arquivo `LICENSE` para obter detalhes.

## Contribuindo

Contribuições são bem-vindas! Sinta-se á  vontade para abrir uma issue ou enviar um pull request.


## Autores

Este projeto foi desenvolvido por:


Ádson Vital Correia (adson.correia@ee.ufcg.edu.br); 

Francisco Olimpio Ferreira da Silva. (francisco.silva@ee.ufcg.edu.br).
