![webstorage](img/webstorage.png)

Armazenamento do Lado do Cliente
===========

O armazenamento local permite que vários aplicativos web utilizem APIs de navegadores para fazer o armazenamento local de dados dos usuários no computador, e um bom exemplo disso são preferências do usuário.

O armazenamento é separado por origem do documento, dessa forma as páginas de um site não podem ler os dados armazenados pelas páginas de outro, mas duas páginas do mesmo site podem compartilhar o armazenamento.

As aplicações web podem escolher quanto tempo os dados ficarão armazenados, dados estes que podem ser salvos de forma temporária até que a janela seja fechada ou o navegador encerrado, como também pode ficar alocado no disco rígido de forma permanente para que após meses e anos possamos ter acesso.

Existem várias formas de armazenamento do lado do cliente. Uma das que vamos abordar é a Web Storage. Vale lembrar que ela é uma API que é utilizada para armazenar grande volume de dados, mas não enormes. Vamos ao que interessa.

## **O que é Web Storage?**

Podemos definir como uma API que se originou como parte do HTML5, porém desmembrada com outras especificações implementadas para rodar também no IE8. Também possui dois objetos que são muito importantes, os quais são: ```localStorage``` e ```sessionStorage```.

>OBS: Se tentarmos abrir o arquivo direto no navegador (via file://) o objeto Storage no IE vai ser ```undefined```, então o armazenamento só vai funcionar se iniciarmos um servidor local por exemplo com Python: [link](https://gist.github.com/fdaciuk/10000330).

Os navegadores que implementam as especificações da Web Storage definem duas propriedades no objeto ```window```: *localStorage* e *sessionStorage*, elas referem-se a um objeto storage que é um array associativo que mapeia chaves de string em valores de string.
O funcionamento do objeto Storage é bem semelhante ao funcionamento de objetos no JavaScript.

Para utilizar ele basta configurar uma propriedade do objeto com uma string e o navegador armazena essa string.

## **Qual a diferença entre localStorage e sessionStorage?**

As principais diferenças são vida útil e o escopo.


## **localStorage**

**Vida Útil:** Os dados que são armazenados por localStorage são permanentes, eles não expiram e continuam armazenados até que algum aplicativo ou o usuário os delete através de configurações de preferências do navegador.

**Escopo:** Tem como escopo a origem do documento que é definida por seu protocolo, nome de host e porta.
Todos os documentos com a mesma origem compartilham os mesmos dados de localStorage. Mas documentos com outras origens nunca podem ler, escrever nem sobrescrever os dados uns dos outros, mesmo que ambos sejam executados a partir de um script do mesmo servidor externo.

**Exemplo de diferentes urls que não podem compartilhar o mesmo armazenamento** 

```http://www.exemplo.com```    		*Protocolo: http;  Nome do Host: www.exemplo.com*

**Os hosts abaixo não vão poder compartilhar o mesmo armazenamento, pois contém diferentes configurações de escopo**

```https://www.exemplo.com```        	*Podemos ver que o procotolo https é diferente do http* 

```http://outrohost.exemplo.com``` 	 	*Podemos ver que o host é diferente*

```http://www.exemplo.com:8080```  		*Podemos ver que a porta utilizada é diferente da que www.exemplo.com utiliza que é por default 80*  


**Caso utilizemos um navegador e salvarmos um dado em localStorage, ele armazena em outros navegadores que possuo instalado em minha máquina?**

Não. Estes dados são armazenados de acordo com cada navegador. Mesmo que eu visite um site utilizando o Firefox e depois visito novamente usando o Chrome, os dados armazenados durante a visita do Firefox não estão acessíveis durante a visita no Chrome.

## **sessionStorage**

**Vida Útil:** Os dados armazenados só existem enquanto a janela ou uma guia do navegador estiver aberta. Quando a janela ou guia é fechada permanentemente, os dados armazenados são excluídos.

**Particularidade:** 
Alguns navegadores modernos possuem o recurso de restaurar a última sessão de navegação, desta forma a vida útil desses dados é recuperada com a última sessão.

**Escopo:** 
É a origem do documento, com este escopo os documentos com origens diferentes nunca vão compartilhar sessionStorage. Mas o escopo é definido de acordo com a janela. Se um usuário tem duas guias do navegador mostrando documentos da mesma origem, essa duas guias têm dados de sessionStorage separados: os script que são executados em uma guia não podem ler nem sobrescrever os dados gravados por scripts na outra guia mesmo que as duas guias estejam visitando exatamente a mesma página e executando os mesmos scripts.

##**Como Podemos configurar um localStorage e uma sessionStorage?**

Para configurar uma variável de localStorage ou sessionStorage podemos usar a notação por ponto ```localStorage.nomeDaVariavel``` ou a notação por colchetes ```localStorage['nomeDaVariavel']```.

- *Notação por Pontos* 

```javascript
//1ª Uma forma Alternativa -> Objeto.nomeDaVariavel = 'valor';
localStorage.jogador   = 'Leonardo';
sessionStorage.jogador = 'Leonardo';
```

```javascript
/* 2ª Forma mais Utilizada Usando o método setItem do objeto localStorage para configurar uma variável ou getItem para recuperar o valor guardado */

// O Primeiro argumento é o nome da variável, o segundo é o valor atribuído.

// setItem serve para configurarmos um localStorage ou sessionStorage
localStorage.setItem('jogador','leonardo');
sessionStorage.setItem('jogador','leonardo');

// getItem serve para acessar o valor guardado na variável jogador
localStorage.getItem('jogador');   // retorna: 'Leonardo'
sessionStorage.getItem('jogador'); // retorna: 'Leonardo'
```

- *Notação por Colchetes*

```javascript
// Objeto.nomeDaVariavel = 'valor'
localStorage['jogador']   = 'Leonardo';
sessionStorage['jogador'] = 'Leonardo';
```

**Podemos fazer uma iteração utilizando o metodo key() para percorrer todas as variáveis armazenadas no localStorage.**

```javascript
for( var i = 0; i < localStorage.length; i++ ) {
	var key   = localStorage.key(i);			// Obtém o nome da variável
	var value = localStorage.getItem(key);     // Recupera o valor
}
```

**Limpando o armazenamento localStorage ou sessionStorage?**

```javascript
localStorage.clear();                // remove todas as variáveis salvas no objeto localStorage.

localStorage.removeItem('jogador'); // remove uma variável específica no localStorage.

delete localStorage.jogador;       // podemos utilizar o operador delete para remover a variável jogador (exceto no IE8).
```

### **Como reconhecer se o navegador possui suporte ao localStorage?** 

- Uma das alternativas amplamente utilizadas pela comunidade é a biblioteca [modernizr](http://modernizr.com). Essa biblioteca JavaScript tem como uma de suas funcionalidades, verificar se alguma propriedade HTML5 é suportada no browser utilizado. Saiba mais neste link do blog Tableless: [Utilizando a Biblioteca Modernizr] (http://tableless.com.br/utilizando-a-biblioteca-modernizr/) 

Após baixar e instalar a biblioteca em seu projeto, utilize o script abaixo para verificar se o navegador suporta o recurso de localStorage:

```javascript
if (Modernizr.localstorage) {
  console.log('localStorage is available!');
  // faça coisas legais utilizando localStorage aqui...
} else {
  // o navegador não suporta localStorage =(
  // avise o usuário, e forneça alternativas aqui...
}
```

###Por que usar getItem e setItem?
- Um dos principais motivos é que quando os navegadores implementarem todas as especificações e permitirem que objetos e arrays sejam armazenados em um *Objeto Storage*, você poderá tirar proveito de eventos disparados também evitará confusões em seu código.

- Outro motivo é que por usar a API Storage baseada em métodos, podemos simular uma API em cima de outras propriedades nos navegadores que não suportam web storage.

- Se usarmos a API baseada em métodos, vamos poder escrever um script que vai utilizar localStorage quando o navegador suportar. Caso ele não possua suporte, podemos utilizar outros mecanismos como ```cookies```. Mesmo sendo uma alternativa em casos extremos, cookies ou userData não são aconselhados, logo abaixo tem um link que vai te ensinar a utilizar o armazenamento local de forma cross-browser.

```javascript
// Uma solução para descobir qual memória está sendo usada
var memoria = window.localStorage || (window.UserDataStorage && new UserDataStorage()) || new CookieStorage();

// seta um valor para jogador 
memoria.setItem('jogador', 'leo');

// Em seguida, pesquisa a variável jogador com a memória utilizada pelo browser;
var usuario = memoria.getItem('jogador');
```

### **Uma coisa que sempre nos atormenta é o velho IE mais pra isso vi um post no jQuery Brasil.org sobre como utilizar localStorage cross-browser com cookies como fallback**
[Utilizando localStorage cross-browser com cookie fallback](http://jquerybrasil.org/jquery-storage-utilizando-localstorage-cross-browser-com-cookie-fallback/) 

- Pesquisando encontrei um polyfill que pode ser uma alternativa para quem deseja trabalhar com JavaScript puro: [polyfill](https://github.com/marcuswestin/store.js)


**Veja a Tabela de Compatilibidade**

[Tabela de Compatibilidade](http://www.quirksmode.org/html5/storage.html)

*Leia Mais artigos interessantes sobre esse assunto: 

[Web Storage – HTML5 @tableless](http://tableless.com.br/web-storage-html5/)

[O PASSADO, PRESENTE & FUTURO DE ARMAZENAMENTO LOCAL PARA APLICAÇÕES WEB](http://diveintohtml5.com.br/storage.html)

### **Bom este é um artigo curto e simples, indicado para iniciantes. Espero ter lhe ajudado a ter uma boa compreensão sobre Web Storage. Faça sua contribuição ou indique este artigo para um amigo, pois você estará compartilhando conhecimento.**

**Referências:**

[O Guia Definitivo JavaScript](http://www.buscape.com.br/javascript-o-guia-definitivo-david-flanagan-856583719x.html#precos)

[O mestre @zenorocha](http://zenorocha.com/html5-local-storage/)

[MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage)
