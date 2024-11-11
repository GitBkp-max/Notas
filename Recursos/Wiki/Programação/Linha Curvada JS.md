---
tags:
  - TI/Programação/JS-TS
---
# `<path>`

O [`<path>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path) element é o elemento mais poderoso na biblioteca SVG de [formas básicas](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes). Ele pode ser usado para criar linhas, curvas, arcos e muito mais.

Caminhos criam formas complexas combinando várias linhas retas ou linhas curvas. Formas complexas compostas apenas por linhas retas podem ser criadas como [`<polyline>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes#polyline) elementos. Enquanto `<polyline>` e `<path>` os elementos podem criar formas semelhantes `<polyline>` os elementos exigem muitas pequenas linhas retas para simular curvas e não dimensionam bem para tamanhos maiores.

Uma boa compreensão dos caminhos é importante ao desenhar SVGs. Embora a criação de caminhos complexos usando um editor XML ou editor de texto não seja recomendada, entender como eles funcionam permitirá identificar e reparar problemas de exibição em SVGs.

A forma de um `<path>` o elemento é definido por um parâmetro: `[d](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d)`. (Veja mais em [formas básicas](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes).) O `d` atributo contém uma série de comandos e parâmetros usados por esses comandos.

Cada um dos comandos é instanciado (por exemplo, criar uma classe, nomeá-la e localizá-la) por uma letra específica. Por exemplo, vamos passar para as coordenadas x e y (`10` `10`). O comando "Mover para" é chamado com a letra `M`. Quando o analisador encontra essa letra, ele sabe que precisa passar para um ponto. Então, para passar para (`10` `10`) o comando a ser usado seria `M 10 10`. Depois disso, o analisador começa a ler para o próximo comando.

Todos os comandos também vêm em duas variantes. Um **letra maiúscula** especifica coordenadas absolutas na página, e a **letra minúscula** especifica coordenadas relativas (por exemplo, _mova 10px para cima e 7px para a esquerda a partir do último ponto_).

Coordenadas no `d` parâmetro são **sempre sem unidade** e, portanto, no sistema de coordenadas do usuário. Mais tarde, aprenderemos como os caminhos podem ser transformados para atender a outras necessidades.

## [Comandos de linha](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#line_commands)

Existem cinco comandos de linha para [`<path>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path) nós. O primeiro comando é o "Move To" ou `M`, que foi descrito acima. São necessários dois parâmetros, uma coordenada (`x`) e coordenar (`y`) para mover para. Se o cursor já estiver em algum lugar da página, nenhuma linha será desenhada para conectar as duas posições. O comando "Mover para" aparece no início dos caminhos para especificar onde o desenho deve começar. Por exemplo:

> [!NOTE]
> M x y
> (or)
> m dx dy

No exemplo a seguir, há apenas um ponto em (`10` `10`). Observe, porém, que não apareceria se um caminho fosse desenhado normalmente. Por exemplo:

![Um ponto vermelho é desenhado em um quadrado branco 10 pixels para baixo e 10 pixels para a direita. Este ponto normalmente não mostra, mas é usado como um exemplo de onde o cursor começará após o comando "Move To"](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/blank_path_area.png)

```xml
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">

  <path d="M10 10"/>

  <!-- Points -->
  <circle cx="10" cy="10" r="2" fill="red"/>

</svg>
```

Existem três comandos que desenham linhas. O mais genérico é o comando "Line To", chamado com `L`. `L` toma dois parâmetros—x e coordenadas y—and desenha uma linha da posição atual para uma nova posição.

> [!NOTE]
> L x y
> (or)
> l dx dy

Existem duas formas abreviadas para desenhar linhas horizontais e verticais. `H` traça uma linha horizontal e `V` desenha uma linha vertical. Ambos os comandos tomam apenas um parâmetro, uma vez que eles só se movem em uma direção.

> [!NOTE]
> H x
> (or)
> h dx
> 
> V y
> (or)
> v dy

Um lugar fácil para começar é desenhando uma forma. Vamos começar com um retângulo (o mesmo tipo que poderia ser mais facilmente feito com um [`<rect>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/rect) elemento). É composto apenas por linhas horizontais e verticais.

![Um quadrado com preenchimento preto é desenhado dentro de um quadrado branco. As bordas do quadrado preto começam na posição (10,10), movem-se horizontalmente para a posição (90,10), movem-se verticalmente para a posição (90,90), movem-se horizontalmente de volta para a posição (10,90) e, finalmente, voltam para a posição original (10, 10).](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/path_line_commands.png)


```xml
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">

  <path d="M 10 10 H 90 V 90 H 10 L 10 10"/>

  <!-- Points -->
  <circle cx="10" cy="10" r="2" fill="red"/>
  <circle cx="90" cy="90" r="2" fill="red"/>
  <circle cx="90" cy="10" r="2" fill="red"/>
  <circle cx="10" cy="90" r="2" fill="red"/>

</svg>
```

Podemos encurtar um pouco a declaração de caminho acima usando o comando "Close Path", chamado com `Z`. Este comando traça uma linha reta da posição atual de volta ao primeiro ponto do caminho. Muitas vezes é colocado no final de um nó de caminho, embora nem sempre. Não há diferença entre o comando maiúsculo e minúsculo.

> [!NOTE]
> Z
> (or)
> z

Então nosso caminho acima poderia ser encurtado para:

```xml
 <path d="M 10 10 H 90 V 90 H 10 Z" fill="transparent" stroke="black"/>
```

As formas relativas desses comandos também podem ser usadas para desenhar a mesma imagem. Comandos relativos são chamados usando letras minúsculas e, em vez de mover o cursor para uma coordenada exata, eles o movem em relação à sua última posição. Por exemplo, uma vez que o nosso retângulo é 80×80, o `<path>` o elemento poderia ter sido escrito como:

```xml
 <path d="M 10 10 h 80 v 80 h -80 Z" fill="transparent" stroke="black"/>
```

O caminho vai se mover para o ponto (`10` `10`) e, em seguida, mover horizontalmente 80 pontos para a direita, em seguida, 80 pontos para baixo, em seguida, 80 pontos para a esquerda e, em seguida, de volta para o início.

Nesses exemplos, provavelmente seria mais simples usar o [`<polygon>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/polygon) ou [`<polyline>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/polyline) elementos. No entanto, os caminhos são usados com tanta frequência no desenho de SVG que os desenvolvedores podem se sentir mais confortáveis em usá-los. Não há penalidade de desempenho real ou bônus por usar um ou outro.

## [Comandos de curva](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#curve_commands)

Existem três comandos diferentes que podem ser usados para criar curvas suaves. Duas dessas curvas são [Curvas bézier](https://developer.mozilla.org/en-US/docs/Glossary/Bezier_curve), e o terceiro é um "arco" ou parte de um círculo. Você já pode ter adquirido experiência prática com curvas Bézier usando ferramentas de caminho no Inkscape, Illustrator ou Photoshop. Há um número infinito de curvas de Bézier, mas apenas duas simples estão disponíveis em `<path>` elementos: um cúbico, chamado com `C`, e um quadrático, chamado com `Q`.

### [Curvas Bézier](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#b%C3%A9zier_curves)

A curva cúbica, `C`é a curva ligeiramente mais complexa. Béziers cúbicos recebem dois pontos de controle para cada ponto. Portanto, para criar um Bézier cúbico, três conjuntos de coordenadas precisam ser especificados.

> [!NOTE]
> C x1 y1, x2 y2, x y
> (or)
> c dx1 dy1, dx2 dy2, dx dy

O último conjunto de coordenadas aqui (`x` `y`) especifique onde a linha deve terminar. Os outros dois são pontos de controle. (`x1` `y1`é o ponto de controle para o início da curva, e (`x2` `y2`) é o ponto de controle para o fim. Os pontos de controle descrevem essencialmente a inclinação da linha começando em cada ponto. A função de Bézier então cria uma curva suave que se transfere da inclinação estabelecida no início da linha, para a inclinação na outra extremidade.

![Curvas Bézier cúbicas com grade](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/cubic_bezier_curves_with_grid.png)

```xml
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">

  <path d="M 10 10 C 20 20, 40 20, 50 10" stroke="black" fill="transparent"/>
  <path d="M 70 10 C 70 20, 110 20, 110 10" stroke="black" fill="transparent"/>
  <path d="M 130 10 C 120 20, 180 20, 170 10" stroke="black" fill="transparent"/>
  <path d="M 10 60 C 20 80, 40 80, 50 60" stroke="black" fill="transparent"/>
  <path d="M 70 60 C 70 80, 110 80, 110 60" stroke="black" fill="transparent"/>
  <path d="M 130 60 C 120 80, 180 80, 170 60" stroke="black" fill="transparent"/>
  <path d="M 10 110 C 20 140, 40 140, 50 110" stroke="black" fill="transparent"/>
  <path d="M 70 110 C 70 140, 110 140, 110 110" stroke="black" fill="transparent"/>
  <path d="M 130 110 C 120 140, 180 140, 170 110" stroke="black" fill="transparent"/>

</svg>
```

O exemplo acima cria nove curvas cúbicas de Bézier. À medida que as curvas se movem para a direita, os pontos de controle se espalham horizontalmente. À medida que as curvas se movem para baixo, elas se tornam mais separadas dos pontos finais. A coisa a notar aqui é que a curva começa na direção do primeiro ponto de controle e depois se dobra para que chegue ao longo da direção do segundo ponto de controle.

Várias curvas de Bézier podem ser amarradas juntas para criar formas estendidas e suaves. Muitas vezes, o ponto de controle de um lado de um ponto será um reflexo do ponto de controle usado no outro lado para manter a inclinação constante. Nesse caso, uma versão de atalho do Bézier cúbico pode ser usada, designada pelo comando `S` (ou `s`).

> [!NOTE]
> S x2 y2, x y
> (or)
> s dx2 dy2, dx dy

`S` produz o mesmo tipo de curva que a anterior—mas se seguir outra `S` comando ou um `C` comando, o primeiro ponto de controle é assumido como sendo um reflexo do usado anteriormente. Se o `S` o comando não segue outro `S` ou `C` comando, então a posição atual do cursor é usada como o primeiro ponto de controle. O resultado não é o mesmo que o `Q` comando teria produzido com os mesmos parâmetros, mas é semelhante.

Um exemplo desta sintaxe é mostrado abaixo, e na figura à esquerda os pontos de controle especificados são mostrados em vermelho, e o ponto de controle inferido em azul.

![Uma curva suave em forma de S é desenhada a partir de duas curvas de Bézier. A segunda curva mantém a mesma inclinação dos pontos de controle que a primeira curva, que é refletida para o outro lado.](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/shortcut_cubic_bezier_with_grid.png)

```xml
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 80 C 40 10, 65 10, 95 80 S 150 150, 180 80" stroke="black" fill="transparent"/>
</svg>
```

O outro tipo de curva de Bézier, a curva quadrática chamada com `Q`é, na verdade, uma curva mais simples do que a cúbica. Requer um ponto de controle que determina a inclinação da curva tanto no ponto inicial quanto no ponto final. É preciso dois parâmetros: o ponto de controle e o ponto final da curva.

**Nota:** Os deltas coordenados para `q` ambos são relativos ao ponto anterior (isto é `dx` e `dy` não são relativos a `dx1` e `dy1`).

> [!NOTE]
> Q x1 y1, x y
> (or)
> q dx1 dy1, dx dy

![Bézier quadrático com grade](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/quadratic_bezier_with_grid.png)

```xml
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 80 Q 95 10 180 80" stroke="black" fill="transparent"/>
</svg>
```

Tal como acontece com a curva cúbica de Bézier, há um atalho para encadear vários Béziers quadráticos, chamados com `T`.

> [!NOTE]
> T x y
> (or)
> t dx dy

Este atalho olha para o ponto de controle anterior usado e infere um novo a partir dele. Isso significa que, após o primeiro ponto de controle, formas bastante complexas podem ser feitas especificando apenas pontos finais.

Isso só funciona se o comando anterior foi um `Q` ou um `T` comando. Se não, então o ponto de controle é assumido como sendo o mesmo que o ponto anterior, e apenas as linhas serão desenhadas.

![Duas curvas quadráticas formam uma curva suave em forma de S. Os pontos de controle da segunda curva são refletidos através do eixo horizontal](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/shortcut_quadratic_bezier_with_grid.png)

```xml
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 80 Q 52.5 10, 95 80 T 180 80" stroke="black" fill="transparent"/>
</svg>
```

Ambas as curvas produzem resultados semelhantes, embora a cúbica permita maior liberdade exatamente como a curva se parece. Decidir qual curva usar é situacional e depende da quantidade de simetria que a linha tem.

### [Arcos](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#arcs)

O outro tipo de linha curva que pode ser criado usando SVG é o arco, chamado com o `A` comando. Arcos são seções de círculos ou elipses.

Para um dado raio-x e raio-y, existem duas elipses que podem conectar quaisquer dois pontos (desde que estejam dentro do raio do círculo). Ao longo de qualquer um desses círculos, existem dois caminhos possíveis que podem ser tomados para conectar os pontos—então, em qualquer situação, existem quatro arcos possíveis disponíveis.

Por causa disso, os arcos exigem alguns parâmetros:

> [!NOTE]
> A rx ry x-axis-rotation large-arc-flag sweep-flag x y
> a rx ry x-axis-rotation large-arc-flag sweep-flag dx dy

No seu início, o elemento de arco leva em dois parâmetros para o x-raio e y-raio. Se necessário, veja [`<ellipse>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/ellipse)s e como eles se comportam. Os dois últimos parâmetros designam as coordenadas x e y para terminar o traçado. Juntos, esses quatro valores definem a estrutura básica do arco.

O terceiro parâmetro descreve a rotação do arco. Isso é melhor explicado com um exemplo:

![SVGArcs_XAxisRotação_com_grid](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/svgarcs_xaxisrotation_with_grid.png)

```xml
<svg width="320" height="320" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 315
           L 110 215
           A 30 50 0 0 1 162.55 162.45
           L 172.55 152.45
           A 30 50 -45 0 1 215.1 109.9
           L 315 10" stroke="black" fill="green" stroke-width="2" fill-opacity="0.5"/>
</svg>
```

O exemplo mostra um `<path>` elemento que vai na diagonal da página. No seu centro, foram recortados dois arcos elípticos (x raio = `30`, y raio = `50`). No primeiro, a rotação do eixo-x foi deixada em `0`, então a elipse que o arco percorre (mostrada em cinza) é orientada para cima e para baixo. Para o segundo arco, porém, a rotação do eixo x é definida como `-45` graus. Isso gira a elipse de modo que ela esteja alinhada com seu eixo menor ao longo da direção do caminho, como mostrado pela segunda elipse na imagem de exemplo.

Para a elipse não rotacionada na imagem acima, existem apenas dois arcos diferentes e não quatro para escolher, porque a linha desenhada do início e do final do arco passa pelo centro da elipse. Em um exemplo ligeiramente modificado, as duas elipses que formam os quatro arcos diferentes podem ser vistas:

![Mostre os 4 arcos no exemplo da Elipse](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/svgarcs_xaxisrotation_with_grid_ellipses.png)


```xml
<svg xmlns="http://www.w3.org/2000/svg" width="320" height="320">
  <path d="M 10 315
           L 110 215
           A 36 60 0 0 1 150.71 170.29
           L 172.55 152.45
           A 30 50 -45 0 1 215.1 109.9
           L 315 10" stroke="black" fill="green" stroke-width="2" fill-opacity="0.5"/>
  <circle cx="150.71" cy="170.29" r="2" fill="red"/>
  <circle cx="110" cy="215" r="2" fill="red"/>
  <ellipse cx="144.931" cy="229.512" rx="36" ry="60" fill="transparent" stroke="blue"/>
  <ellipse cx="115.779" cy="155.778" rx="36" ry="60" fill="transparent" stroke="blue"/>
</svg>
```

Observe que cada uma das elipses azuis é formada por dois arcos, dependendo de viajar no sentido horário ou anti-horário. Cada elipse tem um arco curto e um arco longo. As duas elipses são apenas imagens espelhadas uma da outra. Eles são invertidos ao longo da linha formada a partir dos pontos start→end.

Se os pontos start→end estiverem mais longe do que os da elipse `x` e `y` o raio pode alcançar, os raios da elipse serão minimamente expandidos para que ele possa atingir os pontos start→end. O codepen interativo na parte inferior desta página demonstra isso bem. Para determinar se os raios de uma elipse são grandes o suficiente para exigir expansão, um sistema de equações precisaria ser resolvido, como isto no wolfram alfa. Este cálculo é para a elipse não rotacionada com start→end (`110` `215`)→(`150.71` `170.29`). A solução, (`x` `y`), é o centro da elipse(s). A solução será imaginário se os raios da elipse forem muito pequenos. Este segundo cálculo é para a elipse não rotacionada com start→end (`110` `215`)→(`162.55` `162.45`). A solução tem um pequeno componente imaginário porque a elipse mal foi expandida.

Os quatro caminhos diferentes mencionados acima são determinados pelos próximos dois sinalizadores de parâmetros. Como mencionado anteriormente, ainda existem duas possíveis elipses para o caminho a percorrer e dois caminhos possíveis diferentes em ambas as elipses, dando quatro caminhos possíveis. O primeiro parâmetro é o `large-arc-flag`. Ele determina se o arco deve ser maior ou menor que 180 graus; no final, essa bandeira determina qual direção o arco irá percorrer em torno de um determinado círculo. O segundo parâmetro é o `sweep-flag`. Ele determina se o arco deve começar a se mover em ângulos positivos ou negativos, que essencialmente escolhe qual dos dois círculos será percorrido. O exemplo abaixo mostra todas as quatro combinações possíveis, juntamente com os dois círculos para cada caso.

![Quatro exemplos são mostrados para cada combinação de bandeira de arco grande e bandeira de varredura para dois círculos sobrepostos, um no canto superior direito, o outro no canto inferior esquerdo. Para a bandeira de varrimento = 0, quando a bandeira de arco grande = 0, o arco interior do círculo superior direito é desenhado e, quando a bandeira de arco grande = 1, o arco exterior do círculo inferior esquerdo é desenhado. Para a bandeira de varrimento = 1, quando a bandeira de arco grande = 0, o arco interior do círculo inferior esquerdo é desenhado e, quando a bandeira de arco grande = 1, o arco exterior do círculo superior direito é desenhado.](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/svgarcs_flags.png)

```xml
<svg width="325" height="325" xmlns="http://www.w3.org/2000/svg">
  <path d="M 80 80
           A 45 45, 0, 0, 0, 125 125
           L 125 80 Z" fill="green"/>
  <path d="M 230 80
           A 45 45, 0, 1, 0, 275 125
           L 275 80 Z" fill="red"/>
  <path d="M 80 230
           A 45 45, 0, 0, 1, 125 275
           L 125 230 Z" fill="purple"/>
  <path d="M 230 230
           A 45 45, 0, 1, 1, 275 275
           L 275 230 Z" fill="blue"/>
</svg>
```

Arcos são uma maneira fácil de criar pedaços de círculos ou elipses em desenhos. Por exemplo, um gráfico de pizza exigiria um arco diferente para cada peça.

Se a transição para SVG de [`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas)arcos podem ser a coisa mais difícil de aprender, mas também são muito mais poderosos. Círculos e elipses completos são as únicas formas que os arcos SVG têm problemas para desenhar. Como os pontos de início e fim de qualquer caminho ao redor de um círculo são o mesmo ponto, há um número infinito de círculos que podem ser escolhidos e o caminho real é indefinido. É possível aproximá-los fazendo com que os pontos de início e fim do caminho sejam ligeiramente distorcidos e, em seguida, conectando-os com outro segmento de caminho. Por exemplo, é possível fazer um círculo com um arco para cada semicírculo. Nesse ponto, muitas vezes é mais fácil usar um real [`<circle>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle) ou [`<ellipse>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/ellipse) nó em vez disso. Esta demonstração interativa pode ajudar a entender os conceitos por trás dos arcos SVG: [https://codepen.io/lingtalfi/pen/yaLWJG](https://codepen.io/lingtalfi/pen/yaLWJG)(testado apenas no Chrome e Firefox, pode não funcionar no seu navegador)