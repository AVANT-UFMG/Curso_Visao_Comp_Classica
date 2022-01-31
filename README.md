O que é visão computacional clássica

Para que um computador possa enxergar, precisamos de uma forma para representar as imagens. A forma escolhida para tal representação é a de matrizes, de forma que cada número em uma matriz representa uma cor diferente, o que permite o computador recriar as imagens.
A visão computacional clássica não passa do processamento e extração de informações dessas matrizes, de modo que o resultado possua algum significado para os seres humanos.
O grande problema é que quando as imagens ficam mais e mais complexas é necessário um grande processamento para que características de interesse sejam identificadas, e geralmente o programador precisa implementar códigos específicos para realizar certa extração de certa característica na imagem.
Na detecção de objetos, por exemplo, é razoavelmente fácil identificar objetos simples e que destoam do ambiente em que a imagem está inserida.

![alt text](https://github.com/Eletronica-AVANT-UFMG/Curso_Visao_Comp_Classica/blob/sp/Visao-Computacional-Classica-x-Deep-Learning/.github/images/readmeProject/RedBallDeepLearning.png)

Tomando essa imagem como modelo, observamos que o contraste entre o objeto de interesse e o fundo é alto, então é fácil isolar o objeto filtrando as cores da imagem e as formas presentes nela, o que for um círculo, é o nosso alvo.
Contudo, à medida que as imagens ficam um pouco mais complexas ou quando são acrescentados ruídos grandes nas imagens de referência, temos que, muitas vezes, implementar códigos novos que tratam esses ruídos, e, geralmente, modificar a lógica de identificação.

![alt text](https://github.com/Eletronica-AVANT-UFMG/Curso_Visao_Comp_Classica/blob/sp/Visao-Computacional-Classica-x-Deep-Learning/.github/images/readmeProject/RedBall2DeepLearning.png)

Nesta outra imagem, há um grande ruído azul, mas não conseguimos identificar a bola vermelha da mesma forma com que conseguíamos anteriormente, uma vez que quando filtramos as cores, não conseguimos recuperar o formato de círculo que antes estava visível. Precisamos adaptar o nosso código para tratar esse ruído, mesmo que para o cérebro humano seja óbvio que o círculo vermelho esteja atrás do objeto azul. É aí que o deep learning entra para tornar as coisas mais fáceis.

O que é visão com Deep Learning?

	Deep Learning é um paradigma de computação baseado no cérebro humano, que utiliza Redes Neurais Artificiais para extrair características e ao mesmo tempo classificar as entradas recebidas.
	Com a utilização de redes neurais artificiais a visão computacional dá um grande salto, já que agora é possível treinar um modelo a partir de um conjunto de dados e obter resultados de forma bastante precisa.
	Voltando ao exemplo do círculo vermelho, poderíamos pegar um grande conjunto de imagens em que o círculo vermelho está inserido e passar essas imagens para a rede neural, que depois de tentar classificar o círculo muitas vezes, finalmente iria aprender a identificar o círculo nos mais diversos ruídos, uma vez que o funcionamento dela se assemelha muito ao do cérebro humano, com uma grande quantidade de neurônios artificiais conectados entre si processando todas as informações captadas na imagem para gerar uma resposta.
