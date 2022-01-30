#OpenCV

Open Source Computer Vision Library é uma biblioteca que inclui diversos algoritmos de visão computacional. Atualmente possui mais de 500 funções, pode ser utilizada em diversas linguagens de programação (C++, Python, Ruby, Java…) e é usada para diversos tipos de análise em imagens e vídeos, como  detecção, tracking e reconhecimento facial, edição de fotos e vídeos, detecção e análise de textos, etc.

OpenCV é liberado sob uma licença de BSD e daqui é livre para o uso acadêmico e comercial. Possui interfaces C++, C, Python e Java e suporta Windows, Linux, Mac OS, iOS e Android. OpenCV foi projetado para eficiência computacional e com um forte foco em aplicações em tempo real. Escrito em C/C++ otimizado, a biblioteca pode aproveitar o processamento multi-core. Adotado em todo o mundo, OpenCV tem mais de 47 mil pessoas da comunidade de usuários e número estimado de downloads superior a 6 milhões. O uso varia de arte interativa, a inspeção de minas, costura mapas na web ou através de robótica avançada.

O OpenCV é escrito em C++ e sua interface principal é em C++, mas ainda mantém uma interface C mais antiga, menos abrangente, embora extensa. Todos os novos desenvolvimentos e algoritmos aparecem na interface C++. Existem ligações em Python (por exemplo, métodos OpenCV cv.line, OpenCV cv2.cvtcolor, OpenCV cv2.circle), Java e MATLAB/OCTAVE. 

Um dos únicos pontos de atenção ao se escolher entre usar ou não o OpenCV está nos recursos de hardware do projeto. Em geral, algoritmos que lidam com tratamento e processamento de imagens demandam muita memória e processamento do computador ao qual rodam. Ainda, quanto maiores (com mais resolução e qualidade) as imagens utilizadas forem, mais os algoritmos de tratamento e processamento de imagens vão trabalhar e, portanto, mais recursos computacionais serão exigidos.
Pelo fato de existir uma infinidade de possibilidades de projetos com visão computacional e OpenCV, existe também uma infinidade de casos de uso de recursos computacionais diferentes para projetos com OpenCV. Por essa razão, fica muito difícil (para não dizer impossível) estabelecer uma configuração mínima exigida universal para o OpenCV, uma vez que esta seria altamente dependente do uso desejado. Sendo assim, numa visão mais prática, é altamente aconselhável fazer medições de desempenho enquanto se roda o OpenCV em seu projeto, de forma a se ter noção prática do quanto o OpenCV está demandando do hardware de seu projeto numa dada tarefa e se será necessário um upgrade de hardware ou não.
Para se ter uma referência prática, projetos que envolvam aquisição, tratamento e processamento de imagens (estáticas ou vídeo) para fins de detecção e reconhecimento facial, detecção e obtenção de textos em imagens e buscar e contar objetos numa imagem normalmente podem ser executados com OpenCV em uma Raspberry Pi 3B tranquilamente. Só reforçando: nesses casos, é altamente aconselhável a utilização de um cooler para evitar que os cores do processador cheguem a temperaturas muito altas (o que pode diminuir a vida útil do mesmo).


Para iniciar o código usando openCV em Python, fazemos:

import numpy as np
import cv2 as cv

A seguir temos a parametrização da camera, realizada antes de capturar a imagem.

//comando do sistema (video1, tempo de exposição manual, ganho do sensor manual)

int status = system( "v4l2-ctl -d /dev/video1 -c gain_automatic=0 -c gain=0 -c auto_exposure=1 -c exposure=30 -c power_line_frequency=1" );

Agora, temos como abrir a conexão com a camera para capturar a imagem:

int main(int, char**)
{
// video 1, exposure manual, gain manual
int status = system( "v4l2-ctl -d /dev/video1 -c gain_automatic=0 -c gain=0 -c auto_exposure=1 -c exposure=30 -c power_line_frequency=1" );
	

double start=0;
	double end=0;
	//iniciar captura camera 1
capture.open(1);
	
pthread_mutex_init(&frameLocker, NULL);
	pthread_t UpdateThread;
	pthread_create(&UpdateThread, NULL, UpdateFrame, NULL);
    	
//loop principal
	for (;;)
	{(...)}

Para capturar um frame, por outro lado, usamos:

/capture
void *UpdateFrame(void *arg)
{
	for(;;)
	{
    	try
    	{
        	Mat tempFrame = Mat::zeros( frame.size(), CV_8UC3 );
        	capture >> tempFrame;
       	 
        	pthread_mutex_lock(&frameLocker);
        	if (!tempFrame.empty())
            	frame = tempFrame;
        	pthread_mutex_unlock(&frameLocker);
    	}
    	catch(Exception &e)
    	{
        	cout << e.msg << endl;
    	}
	}   
}

##Detecção

uma função simples e rápida que verifica se o objeto existe na frame capturada.
A detecção ou trigger é uma área retangular no centro da imagem que testa a média da cor/tom.
Se a comparação for verdadeira a frame capturada contém o objeto e podemos inspecioná-la.

//Trigger
float trigger(Mat &tframe)
{
try
	{
    	float resmean = 0;
    	int x=200,y=200,w=300,h=110;
    	Rect roi = Rect(x,y,w,h);
    	Mat mask0 = Mat::zeros( tframe.size(), CV_8UC1 );
    	rectangle(mask0,roi,255,1);
    	Mat imRoi = frame(roi);
    	Scalar tempMean = mean(imRoi);
    	resmean = tempMean.val[0];
    	if (resmean < 123)
        		cout << "\n trigger mean: " << resmean << "\n";
    	return resmean;
    
	}
	catch (Exception &e)
	{   
     	cout << e.msg << endl;
     	return 0;
	}
}

##Pré-processamento

onde filtramos a imagem cinza e transformamos em uma imagem binária preto e branco, onde o objeto é branco e o fundo preto.

void processing(Mat &frame)
{
	try
	{
    	vector<Mat> bgr_planes;
    	split( frame, bgr_planes );
    	//Mat b = bgr_planes[0];
    	//Mat g = bgr_planes[1];
    	Mat r = bgr_planes[2];
    	Mat mask   = Mat::zeros( frame.size(), CV_8UC1 );
    	Mat res = Mat::zeros( frame.size(), CV_8UC1 );
    	Mat open   = Mat::zeros( frame.size(), CV_8UC1 );
    	Mat close  = Mat::zeros( frame.size(), CV_8UC1 );
    	Mat thresh = Mat::zeros( frame.size(), CV_8UC1 );
    	resize( r, mask, Size(), 0.5, 0.5, INTER_LINEAR );
    	threshold(mask,thresh, 120, 255, THRESH_BINARY);
	//cleanup
    	Mat kernel1 = getStructuringElement(MORPH_ELLIPSE, Size(3, 3));
    	morphologyEx(thresh,open, MORPH_OPEN, kernel1);
    	morphologyEx(open,close, MORPH_CLOSE, kernel1);
    	GaussianBlur(close,res, Size(3, 3), 3);
    	resize( res, res, Size(), 2, 2, INTER_LINEAR );
    	inspect(res, frame);
	}
	catch (Exception &e)
	{   
     	cout << e.msg << endl;
	}

}

##Canal  
	
  Canal R – Os canais RGB, são imagens em tons de cinza com profundidade de 8 bits cada, neste caso escolhemos o RED que contém a melhor informação dos tons do biscoito, note que ele é cinza mas representa o vermelho de 0 á 100% na imagem colorida.
  Threshold – Função que binariza a imagem, ou seja, transforma uma imagem em tons de cinza em uma imagem preto e branco, note que ainda existem alguns ruídos na imagem.
  Cleanup – Remoção de ruídos (Reduz a imagem na metade do tamanho, passa filtros de transformação morfológica de abrir e fechar e depois amplia a imagem para o tamanho original).

Outros exemplos de funções usadas para o processamento de imagens em OpenCV podem ser vistos no link abaixo:

https://docs.opencv.org/3.1.0/d2/d96/tutorial_py_table_of_contents_imgproc.html
  

##Biografias:

https://www.newtoncbraga.com.br/index.php/microcontroladores/143-tecnologia/17799-opencv-o-que-e-onde-usar-e-como-instalar-na-raspberry-pi-mic412.html
https://www.embarcados.com.br/aplicacao-de-visao-computacional-com-opencv/
https://blog.cedrotech.com/opencv-uma-breve-introducao-visao-computacional-com-python
  
  
  
 #Instalação OpenCV
  
##Ubuntu

https://docs.opencv.org/4.x/d7/d9f/tutorial_linux_install.html
https://learnopencv.com/install-opencv3-on-ubuntu/

##Windows

https://docs.opencv.org/4.x/d3/d52/tutorial_windows_install.html
https://learnopencv.com/install-opencv3-on-windows/

##MacOS

https://docs.opencv.org/4.x/d0/db2/tutorial_macos_install.html
