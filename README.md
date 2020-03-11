# Advanced React 

# Fragments > divs
Fragments permitem agrupar uma lista de filhos sem adicionar nodes extras na DOM (como o div faz).

> Exemplo:

<code>
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
</code>

Uma syntax mais curta seria <></> mas não suporta keys/atributos.

# Context
Context provê um jeito de compartilhar valores entre componentes sem ter que passar uma prop de modo
explícito para todos os leveis da árvore. Ele compartilha data que pode ser considerada "global" para
uma árvore de components React (usuário autenticado agora, tema, linguagem).

Exemplo:


// Cria um context para usar na árvore de components sem ter que alinhá-lo por todo componente.
const ThemeContext = React.createContext('light'); 

class App extends React.Component {
	render() {
		return(
		<ThemeContext.Provider value="dark">
		  <Toolbar />
		</ThemeContext.Provider>
(...)


function Toolbar() {
	return(
		<div>
		  <ThemedButton />
		</div>
(...)

/* Atribui um contextType para ler o tema atual do context. 
React irá procurar o theme Provider mais próximo acima e usar seu valor. */
*/
class ThemedButton extends React.Component {
	static contextType = ThemeContext;
	render() {
	  return <Button theme={this.context} /> // "dark"
	}
}

> Context é mais usado quando alguma data precisa ser acessível por vários componentes em 
diferentes níveis de aninhamento.

# Higher-Order Components 
Técnica avançada no React para reutilizar componentes. É uma função que pega um componente
e retorna um novo componente.  

# Portals
Portals provêm um modo first-class de renderizar children em um node DOM que existe fora da hierarquia
DOM do componente pai.

/* O primeiro argumento é qualquer elemento filho do React que seja renderizável,
como um elemento, string ou fragmento e o segundo é um elemento DOM */

ReactDOM.createPortal(child, container)

# Profiler 
Um Profiler pode ser adicionado em qualquer lugar da árvore React para mensurar o custo
de renderizar aquela parte da árvore. Requer duas props: um id (string) e uma função callback
onRender que o React chama toda vez que um componente dentro da árvore é atualizado.

# Refs
Refs provêm um modo de acessar nodes DOM ou elementos React criado no método render.
Normalmente, props são o único modo que components pais podem interagir com filhos. 
Assim, para modificar um filho, é necessário re-renderizá-lo com novos props.
Porém, há alguns casos onde é necessário modificar imperativamente uma child fora do fluxo
de data típico. 

# Virtual DOM
Conceito de uma representação ideal virtual da interface do usuário é mantida em memória
e sincronizada com o DOM 'real' por uma biblioteca como o ReactDOM. Esse processo se chama reconciliação.

Você diz ao React qual state deseja a UI para ser, e ele garante que o DOM se encaixe nesse state. 

# Binding
<code>
obj.method()

var method = obj.method
method()
</code>

Esses dois trechos de código não são equivalentes. 
Métodos binding ajudam garantir que o segundo trecho funcione do mesmo jeito que o primeiro.
 
No React, tipicamente só é necessário bindar métodos que você passa para outros componentes.
Exemplo:
<code>
<button onClick={this.handleClick> 
</code>
passa this.handleClick então ele deve ser bindado. 

# Hooks 
Permitem usar state e outras características React sem escrever uma classe.

<code>
import React, {useState} from 'react'

> useState

function Test() {
	const [count, setCount] = useState(0);
	return (
		<div>
			<p>Você clicou {count} vezes</p>
			<button onClick={() => setCount(count + 1)}>Clique</button>
		</div>
	)
}
</code>

Note os brackets <code> [count, setCount] </code>. Essa syntax é chamada array destructuring. 
Significa que estamos fazendo duas novas variáveis count e setCount, onde count é setada pro 
primeiro valor retornado por useState e setCount pro segundo. 

useState(valorInicial) - É similar ao this.setState em uma classe, porém ele não mergeia o state antigo e o novo. 
useState preserva o state entre renderizações.

> useEffect

Data fetching, subscriptions ou mudanças manuais na DOM por componentes React são chamados operações "side effects"
ou só "effects" porque eles podem afetar outros componentes e não pode ser feito durante renderização.

O hook Effect provêm a habilidade de performar side effects por uma função. Serve para o mesmo propósito de 
componentDidMount, componentDidUpdate e componentWillUnmount nas classes React. 

<code>
useEffect(() => {
	document.title = `Você clicou ${count} vezes`;
)}
</code>

Ao chamar useEffect() estamos dizendo ao React para executar sua função effect depois de carregar mudanças
no DOM. Effects são declarados dentro de um componente então têm acesso às suas props e state. Por padrão
o React executa effects depois de cada renderização.

_________________________________________________________
# Library vs Framework

Angular é um framework!
React é uma library!

Library: colecão de definição de classes; Reutilização de códigos. Quando você chama um método
por uma library, você está no controle.

Framework: todo o fluxo de controle já está lá, e há vários espaços em branco pré-definidos
para você preencher com seu código. O controle é invertido: O framework que te chama.

Frameworks normalmente são mais complexos; Definem um esqueleto onde a aplicação define suas
próprias características para preenchê-las. Assim, o seu código será chamado pelo framework
quando for apropriado. 
