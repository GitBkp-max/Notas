---
tags:
  - TI/Programação/JS-TS
  - TI/Programação/WEB
---
### Código da geração de mapeamento

```html
<script>
    class Zabbix_Interpretador {
        apiUrl = "http://monitor.netturbosolucoes.com.br:8989/api_jsonrpc.php";
        authToken = "c6dbe55c40c8ae2c057ce64957d9d3452702b80dda302893b4dd2889f949642d";
        SVGInstance;
        IMGContent = {};  // Armazena o tamanho das imagens após carregamento
        // Função para inicializar o elemento SVG
        InicializarSVG() {
            this.SVGInstance = document.getElementById('Mapa_NTT');  // Obtém o elemento SVG
            this.SVGInstance.innerHTML = "";  // Limpa o conteúdo do SVG
            // Define a área visível do SVG
            this.SVGInstance.setAttribute('viewBox', "0 0 1920 1080");
            // Cria dois grupos: um para as linhas e outro para as imagens
            const linesGroup = document.createElementNS("http://www.w3.org/2000/svg", "g");
            linesGroup.setAttribute("id", "linesGroup");
            this.SVGInstance.appendChild(linesGroup);
            const imagesGroup = document.createElementNS("http://www.w3.org/2000/svg", "g");
            imagesGroup.setAttribute("id", "imagesGroup");
            this.SVGInstance.appendChild(imagesGroup);
        }
        // Carrega os hosts no mapa a partir da lista fornecida
        CarregarHosts(ListaHosts) {
            const imagesGroup = document.getElementById("imagesGroup");
            ListaHosts.forEach(element => {
                // Cria um elemento de imagem SVG para cada host
                let imgElement = document.createElementNS("http://www.w3.org/2000/svg", 'image');
                imgElement.setAttributeNS("http://www.w3.org/1999/xlink", 'href', 'http://monitor.netturbosolucoes.com.br:8989/imgstore.php?iconid=' + element.iconid_off);
                imgElement.setAttribute("x", element.x);
                imgElement.setAttribute("y", element.y);
                // Adiciona a imagem ao grupo de imagens
                this.SVGInstance.appendChild(imgElement);
                // Espera até a imagem carregar para obter suas dimensões
                imgElement.onload = () => {
                    // Obtém as dimensões e o centro da imagem
                    let bbox = imgElement.getBBox();
                    this.IMGContent[element.selementid] = {
                        width: bbox.width,
                        height: bbox.height,
                        centerX: parseInt(element.x) + bbox.width / 2,
                        centerY: parseInt(element.y) + bbox.height / 2
                    };
                    // Adiciona um rótulo de texto para o host abaixo da imagem
                    let textElement = document.createElementNS("http://www.w3.org/2000/svg", 'text');
                    textElement.setAttribute("x", this.IMGContent[element.selementid].centerX);
                    textElement.setAttribute("y", parseInt(element.y) + bbox.height + 20);  // Posição abaixo da imagem
                    textElement.setAttribute("fill", "white");
                    textElement.setAttribute("font-size", "16");
                    textElement.setAttribute("font-family", "Arial");
                    textElement.setAttribute("text-anchor", "middle");
                    textElement.textContent = element.label;
                    // Adiciona o texto ao grupo de imagens
                    imagesGroup.appendChild(textElement);
                };
            });
        }
        // Função para carregar e desenhar as linhas entre os hosts
        CarregarLinhas(ListaLinks) {
            const linesGroup = document.getElementById("linesGroup");
            ListaLinks.forEach(link => {
                // Obtém o centro das imagens dos hosts de origem e destino
                let origem = this.IMGContent[link.selementid1];
                let destino = this.IMGContent[link.selementid2];
                if (origem && destino) {
                    // Define a cor da linha, ou usa "white" como padrão
                    let cor = link.color ? `#${link.color}` : 'white';
                    // Calcula as diferenças nas coordenadas X e Y
                    let deltaX = Math.abs(destino.centerX - origem.centerX);
                    let deltaY = Math.abs(destino.centerY - origem.centerY);
                    // Verifica se é necessário desenhar uma curva ou uma linha reta
                    let pathData;
                    if (deltaX > 50 && deltaY > 50) {  // Se a diferença for significativa, cria uma curva
                        // Define os pontos de controle para criar a curva em "S"
                        let controle1X = origem.centerX + (destino.centerX - origem.centerX) / 4;
                        let controle1Y = origem.centerY - 50;  // Desloca para cima
                        let controle2X = origem.centerX + (destino.centerX - origem.centerX) * (3 / 4);
                        let controle2Y = destino.centerY + 50;  // Desloca para baixo
                        // Cria o caminho da curva Bézier cúbica
                        pathData = `M ${origem.centerX},${origem.centerY} C ${controle1X},${controle1Y} ${controle2X},${controle2Y} ${destino.centerX},${destino.centerY}`;
                    } else {  // Caso contrário, desenha uma linha reta
                        pathData = `M ${origem.centerX},${origem.centerY} L ${destino.centerX},${destino.centerY}`;
                    }
                    // Cria o elemento <path> SVG para desenhar a linha ou curva
                    let pathElement = document.createElementNS("http://www.w3.org/2000/svg", 'path');
                    pathElement.setAttribute("d", pathData);  // Define o caminho
                    pathElement.setAttribute("stroke", cor);
                    pathElement.setAttribute("stroke-width", "2");
                    pathElement.setAttribute("fill", "none");  // Remove o preenchimento do caminho
                    // Adiciona o caminho (linha ou curva) ao grupo de linhas
                    linesGroup.appendChild(pathElement);
                }
            });
        }
        // Função para carregar o mapa com base no ID do mapa
        CarregarMapa(IdMapa = 3) {
            fetch(this.apiUrl, {
                method: 'POST',
                mode: "cors",
                headers: {
                    'Content-Type': 'application/json-rpc',
                },
                body: JSON.stringify({
                    jsonrpc: "2.0",
                    method: "map.get",
                    params: {
                        "output": "extend",
                        "selectSelements": "extend",
                        "selectLinks": "extend",
                        "sysmapids": IdMapa
                    },
                    auth: this.authToken,
                    id: 1
                })
            }).then((response) => {
                response.json().then((DadosZabbix) => {
                    this.InicializarSVG();
                    this.SVGInstance.style.width = DadosZabbix.result[0].width + "px";
                    this.SVGInstance.style.height = DadosZabbix.result[0].height + "px";
                    const hosts = DadosZabbix.result[0].selements;
                    // Carrega e desenha os hosts
                    this.CarregarHosts(hosts);
                    // Aguarda o carregamento das imagens para desenhar as linhas
                    setTimeout(() => {
                        this.CarregarLinhas(DadosZabbix.result[0].links);
                    }, 500);  // Timeout para garantir que as imagens carreguem
                    console.log(DadosZabbix.result[0].width, DadosZabbix.result[0].height, this.SVGInstance, DadosZabbix.result[0].links);
                });
            });
        }
    }
</script>
<svg id='Mapa_NTT'></svg>
```

### Código para iniciar o mapemaento

```js
(()=>{      
	let interpretador = new Zabbix_Interpretador();    
	interpretador.CarregarMapa(13);    
})() 
```
