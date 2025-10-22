# PROJETO DE MONITORIA DE EXPERIMENTAL DE SISTEMAS DE CONTROLE REALIMENTADO

🧠: Texto escrito sem o auxílio de inteligência artificial

🤖: Texto escrito com o auxílio de inteligência artificial

## OBJETIVO: Realizar o controle de temperatura e vazão de ar de um secador de cabelo convencional.

# Mudanças necessárias
🧠 Para realizar o controle de um secador de cabelo convencional, foi necessário realizar alterações em aspectos básicos do projeto atual, que consiste em controlar o fluxo de ar e temperatura atráves de controle de potência de, respectivamente, um cooler e uma resistência de chuveiro (ou lâmpada) via PWM.

O primeiro aspecto que deve ser pensado e, intuitivamente já se supõe, é sobre a alimentação do sistema. No projeto original, todas as fontes de alimentação são DC. Se resumem a alimentação proveniente do prórpio microcontrolador e das fontes de bancada do laboratório. Um secador de cabelo é um item utilizado comumente em tomadas, assim, decidimos alimentá-lo a partir da própria tomada (uma decisaão que futuramente foi repensada). O outro aspecto a ser pensado é a modelagem e a manufatura de estruturas para acoplar os sensores ao secador, como mostra a figura abaixo.

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/fd7203f281a3de32cfbce53ebf565b8b0dd9bbd7/images/secador.png" alt="secador">
</p>

Foram impressos um bocal, para conectar a saída de ar ao sensor de fluxo de ar, além de possuir um furo para que o sensor de temperatura seja inserido. Além disso, imprimiu-se uma base para que o secador ficasse em pé (as peças modeladas no Fusion podem ser encontradas na pasta CAD deste repositório).

# Desenvolvimento do projeto eletrônico

🧠 Diante das mudanças dos requerimentos do projeto, o esquemático abaixo foi desenvolvido, e verificado em _protoboard_ antes da confecção da PCB.

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/5854220135213a4ee05ba36546c0e48b903f04f7/images/Schematic_Secador_Scr.png" alt="secador">
</p>

Vamos por partes. O primeiro elemento a ser exposto são as cargas as quais devemos controlar a potência, isto é, precisamos realizar o controle de tensão sobre o motor do secador de cabelo (diretamente proporcional ao volume de ar que pode ser gerado) e sua resistência (diretamente proporcional a temperatura que o sistema pode atingir). A resistência não pode **sob hipótese nenhuma**, ser ligada quando o motor estiver desligado. Ao fazer isso, retira-se o principal e único dispositivo capaz de resfriar o secador de cabelo de maneira eficaz, podendo acometer na queima da resistência e perda deste componente.

Dessa forma, o desenvolvimento do esquemático foi dedicado a desenvolver uma placa que impeça a conexão e comunicação entre as malhas responsáveis pela alimentação destas cargas (maior potência) e pelo controle das mesmas cargas (menor potência), utilizando de componentes para isolar e separar o máximo possível estas fontes de tensão.

Note que, em basicamente cada zona do esquemático, temos duas cópias de um mesmo circuito, cada uma referente a cada uma das cargas que devemos controlar (motor e resistência do secador). A explicação de cada estágio do circuito vale para as duas cargas.

Primeiramente, antes de explicarmos os estágios do circuito de controle, é necessário deixar claro o que é necessário para construir um sistema de controle. Em suma, um elemento físico precisa de um dispositivo capaz de regular sua potência a partir de um certo acionamento. Em um carro, a velocidade é regulada pelo quanto de combustível é permitido acessar o motor, a partir do acionamento de uma válvula conectada aos cilindros de potência do carro. De forma que quando o carro está abaixo da velocidade, há um fluxo de óleo no cilindro de tal forma que a válvula suba e permita a passagem de um maior volume de combustível, aumentando a valocidade real (a recíproca é verdadeira). No caso de sistemas eletrônicos (como é o nosso), o combustível do motor pode ser tanto uma tensão quanto corrente elétrica aplicada, e cabe a nós encontrar ferramentas que permitam diminuir ou aumentar o fornecimento de combustível" a partir de um acionamento. No nosso caso, ao invés de conectar a alimentação diretamente sobre a carga, adiciona-se um transistor de tal maneira que, para que haja uma tensão na carga, a base do transitor deve estar polarizada e permitindo a passagem de corrente elétrica entre coletor e emissor. Isto é, O controle é realizado através da utilização de um transistor como um elemento de chaveamento. Caso a base do transistor sempre esteja polarizada, a carga estará "recebendo" o máximo de potência possível do sistema, se a base não for polarizada, a carga "receberá" potência nenhuma. Podemos regular a porcentagem de potência fornecida a carga modulando o sinal responsável pela polarização do transistor. Por isso é utilizado o PWM -_Pulse Width Modulation_. Entenda mais sobre PWM neste [link](https://www.youtube.com/shorts/xqR8hdYKgJs)   



## Estágio 1: Optoacopladores

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/76f16076b71ec76a1ff509dff280be8ef9a71f41/images/opto.png" alt="opto">
</p>

🤖 O optoacoplador é usado para isolar eletricamente dois circuitos, permitindo a transferência de sinais entre eles sem contato elétrico direto. O funcionamento baseia-se na conversão de um sinal elétrico em luz e, em seguida, novamente em um sinal elétrico. Internamente, o PC817A possui um LED emissor de infravermelho e um fototransistor. Quando uma corrente flui entre os pinos 1 (ânodo) e 2 (cátodo), o LED emite luz infravermelha. Essa luz incide sobre a base do fototransistor interno, que está entre os pinos 3 (emissor) e 4 (coletor). Assim, quando o LED está aceso, o fototransistor conduz, permitindo a passagem de corrente entre seus terminais. Quando o LED está apagado, o fototransistor permanece em corte, bloqueando o sinal.

🧠 A natureza do nosso sistema já explica por si só por que este componente é necessário, podemos separar o nosso esquemático em duas malhas de magnitudes distintas, uma malha de potência, em que a principal fonte de tensão é a tomada (AC) de 127V, e uma malha de controle, em que as principais fontes são a fonte de bancada, de 12V DC, além dos 3.3V fornecidos ao microcontrolador pela porta USB do _notebook_. Para evitar que tanto o microcontrolador, amplificador operacional, sensores e o _notebook_ sofram uma sobretensão que possa estragar estes componentes, é necessário isolar eletricamente estas malhas. 

## Estágio 2: Push-Pulls

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/8b4d0cf5edfb41e455b89dd79b8a1ae3c9ea84aa/images/push_pull.png" alt="push">
</p>

🤖🧠 O circuito push-pull opera como um driver de corrente de alta eficiência, utilizando um par de transistores complementares (NPN Q1 e PNP Q2) conectados de forma que suas bases sejam controladas por um sinal de entrada único e seus emissores estejam ligados à saída. Este circuito permite o fornecimento e absorção de corrente ativamente, resultando em uma comutação rápida da tensão de saída entre as fontes de alimentação, sendo ideal para acionar cargas que requerem alta corrente, como a porta de um MOSFET (como é o nosso caso).

🧠 Após a amplificação da corrente de saída do optoacoplador, este sinal vai para a porção do circuito que realmente controla o motor do secador.

## Estágio 3: MOSFET

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/8b4d0cf5edfb41e455b89dd79b8a1ae3c9ea84aa/images/mosfet.png" alt="mosfet">
</p>

Este circuito funciona da mesma maneira como foi explicado no parágrafo antes do estágio 1. A alimentação de meia onda alternada da tomada só é fornecida a carga (motor do secador) quando o _gate_ do mosfet for acionado. Em tese, essa configuração permitiria o controle de potência, porém, não foi o que aconteceu.

## Por que deu errado?




