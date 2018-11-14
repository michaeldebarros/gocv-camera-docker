 # **Camera Golang Docker** pt-BR #

## **broken** ##

#### **Dockerfiles** #### 

No intuito de facilitar o deploy este repositório é uma tentativa de conteinerização com o Docker.

A primeira imagem é a "completa", que pode ser construída com a Dockerfile em `./docker/full-image`. Ela parte de uma imagem golang:1.11.1-alpine passando pela instalação de todo OpenCV e depois faz o build do projeto em si.

Esta imagem é pesada, com tamanho de 1.29 GB.

Já a segunda foi feita com um multi-stage build, fazendo um build da aplicação Go, instalando OpenCV em uma nova imagem alpine, e copiando o binário Go para essa nova imagem.  O tamanho da imagem passou para 924MB. Está em `./docker/go-binary`.

Para fazer o build da imagem é necessário passar a flag -f com o path da imagem dessa forma: `docker build -t go-binary  -f ./docker/go-binary/Dockerfile .`

**Apesar de o build estar ok, o streaming do video I/O não está funcionando.**

Aparentemente o API backend que o OpenCV está usando é o GStreamer, mas sem sucesso. De acordo com a [documentação](https://www.docs.opencv.org/3.2.0/d0/da7/videoio_overview.html)  
 o OpenCV seleciona automaticamente o primeira API backend disponível.  Se o Gstreamer não está funcionando, provavelmente seja possível alterar para algo como FFMPEG, sendo necessário ou escolher uma imagem com ele já instalado ou instalar manualmente.
