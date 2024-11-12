---
title: DEV - 2109 Fazendo config de triggers
allDay: false
startTime: 08:30
endTime: 18:00
date: 2024-11-12
completed: false
tags:
  - AI/GPT
  - TI/Programação/JS-TS
---
[[2109 - Mapa da rede Datacenter + Bases|Link do projeto]]

### Ultimas alterações

Para verificar se há triggers e lidar com múltiplas triggers entre dois hosts, você pode ajustar o código dentro da função `CarregarLinhas`. Aqui está uma solução que verifica se há triggers entre os hosts e modifica a cor da linha com base no número e no status dessas triggers.

### Ajuste na função `CarregarLinhas`:

1. Adicione uma lógica para lidar com múltiplas triggers entre os hosts.
2. Modifique a cor da linha de acordo com as triggers encontradas.
3. Se houver uma ou mais triggers ativas (valor `1`), a linha ficará vermelha. Caso contrário, ficará na cor padrão ou em outra cor se definida.

Aqui está o código modificado com o verificador para triggers:

```javascript
// Função para carregar e desenhar as linhas (links) entre os hosts
async CarregarLinhas(ListaLinks) {
    const linesGroup = document.getElementById("linesGroup");  // Obtém o grupo de linhas no SVG

    // Para cada link na lista de links
    for (const link of ListaLinks) {
        let origem = this.IMGContent[link.selementid1];  // Obtém a posição do host de origem
        let destino = this.IMGContent[link.selementid2];  // Obtém a posição do host de destino

        if (origem && destino) {
            // Obtém as triggers (alertas) entre o host de origem e o de destino
            let triggers = await this.ObterTriggers(link.selementid1, link.selementid2);

            let cor = "white";  // Cor padrão das linhas

            // Verifica se há triggers e altera a cor conforme a severidade
            if (triggers.length > 0) {  // Se houver uma ou mais triggers
                // Verifica se alguma trigger está ativa (value == 1)
                let triggerAtiva = triggers.some(trigger => trigger.value == 1);

                if (triggerAtiva) {
                    // Se há triggers ativas, a linha será vermelha
                    cor = 'red';
                } else {
                    // Caso não haja triggers ativas, verifica se há uma cor definida no link
                    if (link.color) {
                        cor = `#${link.color}`;  // Cor definida para o link
                    }
                }

                // Exibe uma mensagem no console com as triggers encontradas entre os hosts
                console.log(`Triggers entre ${link.selementid1} e ${link.selementid2}:`, triggers);
            }

            // Se não houver triggers, mantém a cor padrão ou definida no link
            let deltaX = Math.abs(destino.centerX - origem.centerX);  // Diferença em X entre origem e destino
            let deltaY = Math.abs(destino.centerY - origem.centerY);  // Diferença em Y entre origem e destino

            let pathData;
            if (deltaX > 50 && deltaY > 50) {
                // Desenha uma curva Bézier se a distância entre os hosts for grande o suficiente
                let controle1X = origem.centerX + (destino.centerX - origem.centerX) / 4;
                let controle1Y = origem.centerY - 50;

                let controle2X = origem.centerX + (destino.centerX - origem.centerX) * (3 / 4);
                let controle2Y = destino.centerY + 50;

                pathData = `M ${origem.centerX},${origem.centerY} C ${controle1X},${controle1Y} ${controle2X},${controle2Y} ${destino.centerX},${destino.centerY}`;
            } else {
                // Se a distância for pequena, desenha uma linha reta
                pathData = `M ${origem.centerX},${origem.centerY} L ${destino.centerX},${destino.centerY}`;
            }

            // Cria o elemento de caminho (linha) no SVG
            let pathElement = document.createElementNS("http://www.w3.org/2000/svg", 'path');
            pathElement.setAttribute("d", pathData);  // Define os dados da linha (origem e destino)
            pathElement.setAttribute("stroke", cor);  // Define a cor da linha com base nas triggers
            pathElement.setAttribute("stroke-width", "2");  // Define a espessura da linha
            pathElement.setAttribute("fill", "none");  // Nenhum preenchimento, apenas o traçado

            linesGroup.appendChild(pathElement);  // Adiciona a linha ao grupo de linhas no SVG
        } else {
            console.warn(`Host de origem ou destino não encontrado para o link entre ${link.selementid1} e ${link.selementid2}`);
        }
    }
}
```

### Explicações dos ajustes:
- **Verificação de múltiplas triggers:** O código agora verifica se existem várias triggers entre dois hosts. O método `some()` é usado para verificar se **alguma** trigger está ativa (`value == 1`), e se sim, a cor da linha será definida como vermelha.
- **Triggers desativadas:** Caso existam triggers, mas nenhuma esteja ativa, a linha manterá a cor padrão (branca) ou a cor definida no próprio link (`link.color`).
- **Mensagens de debug:** O código exibe no console as triggers encontradas para que você possa monitorar o que está sendo verificado durante a execução.

### Verificação em tempo real:
Se um link entre dois hosts tiver múltiplas triggers, o sistema irá:
- **Verificar todas as triggers** e se qualquer uma estiver ativa, a linha será vermelha.
- Caso contrário, seguirá as regras de cor padrão ou a cor fornecida.

Essa abordagem garante que o status das triggers seja verificado adequadamente e reflete visualmente no mapa se houver problemas entre hosts.