![webstorage](img/webstorage.png)

Armazenamento do Lado do Cliente
===========

O armazenamento local permite que vários aplicativos web utilizem APIs de navegadores para fazer o armazenamento local de dados dos usuários no computador e um bom exemplo disso são preferências do usuário.

O armazenamento é separado por origem do documento, dessa forma as páginas de um site não podem ler os dados armazenados pelas páginas do outro, mas duas páginas do mesmo site podem compartilhar o armazenamento.

As aplicações web podem escolher quanto tempo os dados ficam armazenados, dados estes que podem ser salvos de forma temporária até que a janela seja fechada ou o navegador encerrado, como também pode ficar alocado no disco rígido de forma permanente para que após meses e anos possamos ter acesso.

Existem várias formas de armazenamento do lado do cliente. Uma das que vamos abordar é a Web Storage. Vale lembrar que ela é uma API que é utilizada para armazenar grande volume de dados, mas não enormes. Vamos ao que interessa.

## **O que é Web Storage?**

Podemos definir como uma API que possui sua origem que é definida como parte do HTML5, porém desmembrada com outras especificações implementadas para rodar também no IE8 *OBS: no só roda em um servidor web, caso você crie uma página html5 com javascript incluso e armazene algo se você rodar diretamente no navegador o objeto Storage no IE vai ser ```undefined```, sendo que em um servidor local vai armazenar perfeitamente*. Também possui dois objetos que são muito importantes são: ``` localStorage``` e ``` sessionStorage ```.

Os navegadores que implementão as especificações da Web Storage definiram duas propriedades no objeto  ``` window ``` : *localStorage* e *sessionStorage*,  elas referem-se a um objeto storage que é um array associativo que mapeia chaves de string em valores de string.
O funcionamento do objeto Storage é bem semelhante ao funcionamento de objetos no JavaScript.

Para utilizar ele basta configurar uma propriedade do objeto com uma string e o navegador armazena essa string.

## **Qual a diferença entre localStorage e sessionStorage?**

A principais diferenças são vida útil e o escopo.


## **localStorage**

**Vida Útil:** Os dados que são armazenados por localStorage são permanente eles não expiram e continuam armazenados até que algum aplicativo ou  usuário delete preferências do navegador.

**Escopo:** Tem como escopo a origem do documento que é definida por seu protocolo, nome de host e porta.
Todos os documentos com a mesma origem compartilha os mesmo dados de localStorage. Mas documentos com outras origens nunca podem ler escrever nem sobrescrever os dados uns dos outros. Mesmo que ambos sejam executados a partir de um script do mesmo servidor externo.

**Exemplo de diferentes urls que não podem compartilhar o mesmo armazenamento** 

```http://www.exemplo.com```    		*Protocolo: http;  Nome do Host: www.exemplo.com*

**Os hosts abaixo não vão poder compartilhar o mesmo armazenamento, pois contem diferentes configurações de escopo**

```https://www.exemplo.com```        	*Podemos ver que o procotolo https é diferente do http* 

```http://outroHost.exemplo.com``` 	 	*Podemos ver que o host é diferente*

```http://www.exemplo.com:8080```  		*Podemos ver que a porta utilizada é diferente da que www.exemplo.com utiliza que é por deafault 80*  


**Caso utilizemos um navegador e salvarmos um dado localStorage ele armazena em outros navegadores que possuo instalado em minha máquina?**

Não. Estes dados são armazenados de acordo com cada navegador. Mesmo que eu visite um site utilizando o Firefox e depois visito novamente usando o Chrome, os dados armazenados durante a visita do Firefox não estão acessíveis durante a visita no Chrome.

## **sessionStorage**

**Vida Útil:** Os dados armazenados só existem enquanto o a janela do navegador está aberta ou uma janela por meio de guia. Quando a janela ou guia é fechada permanentemente, os dados armazenados são excluídos.

**Particularidade:** 
Alguns navegadores modernos possuem o recurso de restaurar a última sessão de navegação desta forma a vida útil desses dados é recuperada com a última sessão

**Escopo:** 
É a origem do documento, com este escopo os documentos com origens diferentes nunca vão compartilhar sessionStorage. Mas o escopo é definido de acordo com a janela. Se um usuário tem duas guias do navegador mostrando documentos da mesma origem, essa duas guias têm dados de sessionStorage separados: os script que são executados em uma guia não podem ler nem sobrescrever os dados gravados por scripts na outra guia mesmo que as duas guias estejam visitando exatamente a mesma página e executando os mesmos scripts.

##**Como Podemos configurar um localStorage e uma sessionStorage?**

Para configurar uma variável de localStorage ou sessionStorage podemos usar a notação por ponto ```localStorage.nomeDaVariavel``` ou a notação por colchetes ```localStorage['nomeDaVariavel']```.

- *Notação por Pontos* 

```javascript
//1ª Uma forma Alternativa -> Objeto.nomeDaVariavel  = 'valor';
localStorage.jogador   = 'Leonardo';
sessionStorage.jogador = 'Leonardo';
```

```javascript
/* 2ª Forma mais Utilizada Usando o método setItem do objeto localStorage para configurar uma variável ou getItem para recuperar o valor guardado */

//O Primeiro argumento é o nome da variável, o segundo é o valor atribuído.

//setItem serve para configurarmos um localStorage ou sessionStorage
localStorage.setItem('jogador','leonardo');
sessionStorage.setItem('jogador','leonardo');

//getItem serve para acessar o valor guardado na variável jogador
localStorage.getItem('jogador','leonardo'); 
sessionStorage.getItem('jogador','leonardo');
```

- *Notação por Colchetes*

```javascript
//Objeto.nomeDaVariavel     = 'valor'
localStorage['jogador']   = 'Leonardo';
sessionStorage['jogador'] = 'Leonardo';
```

**Podemos fazer uma iteração utilizando o metodo key() para percorrer todas as variáveis armazenadas no localStorage.**

```javascript
for( var i = 0; i < localStorage.length; i++ ){

	var key   = localStorage.key(i);			//Obtém o nome da vaiável
	var value = localStorage.getItem(key);     //Recupera o valor

}
```

**Limpando o armazenamento localStorage ou sessionStorage?**

```javascript
localStorage.clear();                //remove todas as variáveis salvas no objeto localStorage

localStorage.removeItem('jogador'); //remove uma variável específica no localStorage

delete localStorage.jogador;       //podemos utilizar o operador delete para remover a variável jogador exceto no IE8 não funciona
```

### **Como reconhecer se o navegador possui suporte ao localStorage?** 

-Uma das alternativas que sempre o pessoal recorre a ```modernizr```. Que é uma biblioteca JavaScript que tem funcionalidade e uma delas é verificar se alguma propriedade HTML5 é suportada no browser utilizado. Saiba mais neste link do Tableless: [Utilizando a Biblioteca Modernizr] (http://tableless.com.br/utilizando-a-biblioteca-modernizr/) 

Utilize o script abaixo para verificar se o navegador suporta.

```javascript
if (Modernizr.localstorage) {
  alert('localStorage is available!');
  // run localStorage code here...
}
else {
  // no native support for local storage
  alert('This Browser Doesn\'t Support Local Storage.');
}
```

###Porque usar getItem e setItem?
Um dos principais motivos é quando os navegadores implementarem todas as especificações
e permitirem que objetos e array sejam armazenados em um *Objeto Storage*.

-Outro motivo é por usar API Storage baseada em métodos é que podemos simular API em cima de outras propriedades nos navegadores que nao suportam web storage.

-Se usarmos a API baseada em métodos, vamos poder escrever um script que vai utilize localStorage quando o navegador suportar. Caso ele não possua suporte como no IE vamos armazenar utilizanos os mecanismos que o IE nos oferece que são ```cookies```, mesmo sendo uma alternativas utilizar cookies ou userData não aconselho utilize o jQuery, logo abaixo tem um link que vai te ensinar a deixar o armazenamento cross browser.

```javascript
//Uma solução par descobir qual memória está sendo usada
var memoria = window.localStorage || (window.UserDataStorage && new UserDataStorage()) || new CookieStorage();

//seta um valor para jogador 
memoria.setItem("jogador",'leo');

//Em seguida, pesquisa a variável jogador com a memória utilizada pelo browser;
var usuario = memoria.getItem("jogador");
```

### **Uma coisa que sempre nos atormenta é o velho IE mais pra isso vi um post no jQuery Brasil.org como utilizar localStorage cross-browser com cookie fallback**
[Utilizando localStorage cross-browser com cookie fallback](http://jquerybrasil.org/jquery-storage-utilizando-localstorage-cross-browser-com-cookie-fallback/) 

-Pesquisando encontrei polyfill que pode ser uma alternativa para quem deseja trabalhar com JavaScript puro: [polyfill](https://github.com/marcuswestin/store.js)


**Veja a Tabela de Compatilibidade**

[Tabela de Compatibilidade](http://www.quirksmode.org/html5/storage.html)

*Leia Mais artigos interessantes : 
[Web Storage – HTML5 @tableless](http://tableless.com.br/web-storage-html5/)
[O PASSADO, PRESENTE & FUTURO DE ARMAZENAMENTO LOCAL PARA APLICAÇÕES WEB](http://diveintohtml5.com.br/storage.html)

### **Bom este é um artigo curto e simples indicado para iniciantes, espero ter ajudado a vocês a ter uma boa compreensão sobre Web Storage. No próximo artigo abordaremos sobre cookies**

**Fontes:**

[O Guia Definitivo JavaScript](http://www.buscape.com.br/javascript-o-guia-definitivo-david-flanagan-856583719x.html#precos)

[O mestre @zenorocha](http://zenorocha.com/html5-local-storage/)

[MDN] (https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage)
