### Image Stream e Compartilhamento de bibliotecas

O Openshift permite monitorar e realizar algumas ações assim que uma imagem é atualizada no seu registry interno. Se uma aplicação tiver uma falha de segurança crítica, e existirem 200 containers desse sistema, basta que a sua respectiva imagem seja atualizada no registry que todos os 200 containers serão atualizados automaticamente.

Todo esse poder de notificação, só existe graças a um componente chamado Image Stream. Um Image Stream é um repositório virtual que contém as informações das suas imagens.

Nessa demo iremos criar uma imagem base baseado no Httpd e, baseado nela, iremos criar duas aplicações. Logo em seguida simularemos uma atualização da imagem base, o que acarretará na construção \(build\) e atualização das duas aplicações que dependem dela.

#### Preparando o ambiente

Para que essa demo funcione conforme esperado, é necessário liberar a permissão de rodar os containers como root. 

> peça ao instrutor que conceda, temporáriamente, essa permisssão especial no seu projeto!

```
oc adm policy add-scc-to-user anyuid -z default -n myproject
```

Vamos importar o template que iniciará o build das 3 imagens necessárias.

```
oc create -f https://raw.githubusercontent.com/luszczynski/openshift-image-stream-example/master/template.yaml
```

Agora já podemos executar o template

```
oc new-app \
 --template=base-image-example \
 --param=GIT_URL=https://github.com/luszczynski/openshift-image-stream-example.git
```

Espere alguns minutos. Aproveite e tome um café nesse tempo.

Se tudo ocorreu conforme o espero, você terá uma tela com duas aplicações.

![](/assets/Selection_038.png)Se acessarmos os builds, veremos que foram realizados 3 builds. Mas então porque só temos 2 aplicações?

![](/assets/Selection_041.png)

Um dos builds foi somente para criar a nossa imagem base e salvá-la no registry. As aplicações **app-a** e **app-b** são baseadas nessa imagem chamada **httpd-base**.

#### Atualizando a base image

Digamos que descobrimos um bug crítico no apache \(httpd\). Para isso, vamos simular a atualização da imagem base e ver como as aplicações **app-a** e **app-b** se comportam.

Para atualizar a base image, precisamos criar um novo build.

1. No menu lateral esquerdo, acesse **Builds** -&gt; **Builds**
2. Selecione o build de nome **httpd-base **na tabela
3. Depois clique no canto superior direito em **Start build**

![](/assets/new-build-is.gif)

Repare que após o build **httpd-base** o Openshift inicia o build das aplicações **app-a** e **app-b** automaticamente. Ou seja, ele está reconstruindo essas aplicações pois elas dependem da imagem base.

![](/assets/Selection_043.png)

