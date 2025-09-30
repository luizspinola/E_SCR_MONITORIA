# PROJETO DE MONITORIA DE EXPERIMENTAL DE SISTEMAS DE CONTROLE REALIMENTADO

## OBJETIVO: Realizar o controle de temperatura e vazão de ar de um secador de cabelo convencional.

# Mudanças necessárias
Para realizar o controle de um secador de cabelo convencional, foi necessário realizar alterações em aspectos básicos do projeto atual, que consiste em controlar o fluxo de ar e temperatura atráves de controle de potência de, respectivamente, um cooler e uma resistência de chuveiro (ou lâmpada) via PWM.

O primeiro aspecto que deve ser pensado e, intuitivamente já se supõe, é sobre a alimentação do sistema. No projeto original, todas as fontes de alimentação são DC. Se resumem a alimentação proveniente do prórpio microcontrolador e das fontes de bancada do laboratório. Um secador de cabelo é um item utilizado comumente em tomadas, assim, decidimos alimentá-lo a partir da própria tomada (uma decisaão que futuramente foi repensada). O outro aspecto a ser pensado é a modelagem e a manufatura de estruturas para acoplar os sensores ao secador, como mostra a figura abaixo.

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/fd7203f281a3de32cfbce53ebf565b8b0dd9bbd7/images/secador.png" alt="secador">
</p>

Foram impressos um bocal, para conectar a saída de ar ao sensor de fluxo de ar, além de possuir um furo para que o sensor de temperatura seja inserido. Além disso, imprimiu-se uma base para que o secador ficasse em pé (as peças modeladas no Fusion podem ser encontradas na pasta CAD deste repositório).

# Desenvolvimento do projeto eletrônico

Diante das mudanças dos requerimentos do projeto, o esquemático abaixo foi desenvolvido, e verificado em _protoboard_ antes da confecção da PCB.

<p align="center">
  <img src="https://github.com/luizspinola/E_SCR_MONITORIA/blob/5854220135213a4ee05ba36546c0e48b903f04f7/images/Schematic_Secador_Scr.png" alt="secador">
</p>

Vamos por partes. O primeiro elemento a ser exposto são as cargas as quais devemos controlar a potência, isto é, precisamos realizar o controle de tensão sobre o motor do secador de cabelo (diretamente proporcional ao volume de ar que pode ser gerado) e sua resistência (diretamente proporcional a temperatura que o sistema pode atingir). A resistência não pode **sob hipótese nenhuma**, ser ligada quando o motor estiver desligado. Ao fazer isso, retira-se o principal e único dispositivo capaz de resfriar o secador de cabelo de maneira eficaz, podendo acometer na queima da resistência e perda deste componente.

Dessa forma, o desenvolvimento do esquemático foi dedicado a desenvolver uma placa que impeça a conexão e comunicação entre as malhas responsáveis pela alimentação destas cargas (maior potência) e pelo controle das mesmas cargas (menor potência), utilizando de componentes para isolar e separar o máximo possível estas fontes de tensão.



