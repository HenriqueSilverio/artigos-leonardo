![webstorage](img/webstorage.png)

Armazenamento do Lado do Cliente
===========

O armazenamento local permite que vários aplicativos web utilizem APIs de navegadores para fazer o armazenamento local de dados dos usuários no computador e um bom exemplo disso são preferências do usuário.

O armazenamento é separado por origem do documento, dessa forma as páginas de um site não podem ler os dados armazenados pelas páginas do outro, mas duas páginas do mesmo site podem compartilhar o armazenamento.

Os Aplicativos web podem escolher quanto tempo os dados ficam armazenados, dados estes que podem ser salvos de forma temporária até que a janela seja fechada ou o navegador encerrado, como também pode ficar alocado no disco rígido de forma permanente para que após meses e anos possamos ter acesso.

Existem várias formas de armazenamento do lado do cliente. Uma das que vamos abordar é a Web Storage. Vale lembrar que ela é uma API que é utilizada para armazenar grande volume de dados, mas não enormes. Vamos ao que interessa.

**O que é Web Storage?**

Podemos definir como uma API que possui sua origem é definida como parte do HTML5, porém desmembrada com outras especificações e foi implementado para rodar também no IE8. Também possui dois objetos que são muito importantes que vamos conhecer logo abaixo ```javascript localStorage``` e ```javascript sessionStorage ```.

Para utilizar ele basta configurar uma propriedade do objeto com uma string e o navegador armazena essa string.

## **Qual a diferença entre localStorage e sessionStorage?**

A principais diferenças são vida útil e o escopo delas.

Os navegadores que implementão as especificações da Web Storage definiram duas propriedades no objeto  ```javascript window ``` : *localStorage* e *sessionStorage*,  elas referem-se a um objeto storage que é um array associativo que mapeia chaves de string em valores de string.
O funcionamento do objeto Storage é bem semelhante ao funcionamento de objetos no JavaScript.

## **localStorage**

*Vida Útil:* Os dados que são armazenados por localStorage são permanente eles não expiram e continuam armazenados até que algum aplicativo ou  usuário delete as preferências do navegador assim é que são excluídos.

**Escopo:** Tem como escopo a origem do documento que é definida por seu protocolo, nome de host e porta.
Todos os documentos com a mesma origem compartilha os mesmo dados de localStorage. Mas documentos com outras origens nunca podem ler escrever nem sobrescrever os dados uns dos outros. Mesmo que ambos sejam executados a partir de um script do mesmo servidor externo.

*Caso utilizemos um navegador e salvarmos um dado localStorage ele armazena em outros navegadores que possuo instalado em minha máquina?*

Não. Estes dados são armazenados de acordo com cada navegador.Mesmo que eu visite um site utilizando o Firefox e depois visito novamente usando o Chrome, os dados armazenados durante a visita do Firefox não estão acessíveis durante a visita no Chrome.

## **sessionStorage**

*Vida Útil:* Os dados armazenados só existem enquanto o a janela do navegador está aberta ou uma janela por meio de guia. Quando a janela ou guia é fechada permanentemente, os dados armazenados são excluídos.

**Particularidade:** 
Alguns navegadores modernos possuem o recurso de restaurar a última sessão de navegação desta forma a vida útil desses dados é recuperada com a última sessão

**Escopo:** 
É a origem do documento, com este escopo os documentos com origens diferentes nunca vão compartilhar sessionStorage. Mas o escopo é definido de acordo com a janela. Se um usuário tem duas guias do navegador mostrando documentos da mesma origem, essa duas guias têm dados de sessionStorage separados: os script que são executados em uma guia não podem ler nem sobrescrever os dados gravados por scripts na outra guia mesmo que as duas guias estejam visitando exatamente a mesma página e executando os mesmos scripts.

##**Como Podemos configurar um localStorage e uma sessionStorage?**

Para configurar uma variável de localStorage ou sessionStorage podemos usar a notação por ponto ```localStorage.nomeDaVariavel``` ou a notação por colchetes ```localStorage['nomeDaVariavel']```.

- *Notação por Pontos* 

```javascript
//1ª Uma forma Alternativa
Objeto.nomeDaVariavel  = “valor”;
localStorage.jogador   = 'Leonardo';
sessionStorage.jogador = 'Leonardo';
```

```javascript
/* 2ª Forma mais Utilizada
Usando o método setItem do objeto localStorage para atribuir um valor a variável jogador.*/

//O Primeiro argumento é o nome da variável, o segundo é o valor atribuído.
localStorage.setItem('jogador','leonardo');
sessionStorage.setItem('jogador','leonardo');
```

- *Notação por Colchetes*

```javascript
Objeto.nomeDaVariavel     = 'valor'
localStorage['jogador']   = 'Leonardo';
sessionStorage['jogador'] = 'Leonardo';
```
**Limpando o armazenamento localStorage ou sessionStorage?**

```javascript
localStorage.clear();                //remove todas as variáveis salvas no objeto localStorage

localStorage.removeItem('jogador'); //remove uma variável específica no localStorage

delete localStorage.jogador;       //podemos utilizar o operador delete para remover a variável jogador
```

**Como reconhecer se o navegador possui suporte ao localStorage?**

-Uma das alternativas que sempre o pessoal recorre é o modernizr. Que é uma biblioteca JavaScript que tem funcionalidade e uma delas é verificar se alguma propriedade HTML5 é suportada no browser utilizado.

Saiba mais neste link do Tableless: [Utilizando a Biblioteca Modernizr] (http://tableless.com.br/utilizando-a-biblioteca-modernizr/) 

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

Uma coisa que sempre nos atormenta é o velho IE mais pra isso vi um post no jQuery Brasil.org como utilizar localStorage cross-browser com cookie fallback

[Utilizando localStorage cross-browser com cookie fallback](http://jquerybrasil.org/jquery-storage-utilizando-localstorage-cross-browser-com-cookie-fallback/) 

-Pesquisando encontrei polyfill que pode ser uma alternativa para quem deseja trabalhar com JavaScript puro e o legal é que é Cross-browser.: [polyfill](https://github.com/marcuswestin/store.js)

-Caso você queira verificar quais os navegadores possui compatibilidade segue o link.**
[Tabela de Compatibilidade]http://www.quirksmode.org/html5/storage.html

*Leia Mais artigos interessantes do Tableless:* [link](http://tableless.com.br/web-storage-html5/)

### **Bom este é um artigo curto e simples indicado para iniciantes, espero ter ajudado a vocês a ter uma boa compreensão sobre Web Storage.**

**Fontes:**

[O Guia Definitivo JavaScript](http://www.buscape.com.br/javascript-o-guia-definitivo-david-flanagan-856583719x.html#precos)

[O mestre @zenorocha](http://zenorocha.com/html5-local-storage/)

[MDN] (https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage)
