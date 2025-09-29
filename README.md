# PROJETO DE MONITORIA DE EXPERIMENTAL DE SISTEMAS DE CONTROLE REALIMENTADO

## OBJETIVO: Realizar o controle de temperatura e vazão de ar de um secador de cabelo convencional.

### 🤖: Texto gerado com auxílio de IA
### 🧠: Texto gerado por conta própria

# Mudanças necessárias
🧠 Para realizar o controle de um secador de cabelo convencional, foi necessário realizar alterações em aspectos básicos do projeto atual, que consiste em controlar o fluxo de ar e temperatura atráves de controle de potência de, respectivamente, um cooler e uma resistência de chuveiro (ou lâmpada) via PWM.

O primeiro aspecto que deve ser pensado e, intuitivamente já se supõe, é sobre a alimentação do sistema. No projeto original, todas as fontes de alimentação são DC. Se resumem a alimentação proveniente do prórpio microcontrolador e das fontes de bancada do laboratório. Um secador de cabelo é um item utilizado comumente em tomadas, assim, decidimos alimentá-lo a partir da própria tomada (uma decisaão que futuramente foi repensada). O outro aspecto a ser pensado é a modelagem e a manufatura de estruturas para acoplar os sensores ao secador, como mostra a figura abaixo.

![Secador com peças imprimidas](https://github.com/luizspinola/E_SCR_MONITORIA/tree/main/images/secador.png)



