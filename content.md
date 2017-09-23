# Introdução

A evolução dos computadores fez com que mais formas diferentes de
acesso a informação sejam possíveis. Hoje em dia é possível, por
exemplo, verificar sua conta de _e-mail_ de seu computador pessoal,
ou de seu _notebook_, de seu aparelho celular ou ainda de seu
_smartwatch_. Cada um desses aparelhos com diferentes tamanhos
físicos e diferentes poderes de processamento. Nesse cenário,
voltamos um pouco da ideia de _Mainframes_ para acesso remoto
de arquivos ou serviços, mas agora com novas estruturas e
arquiteturas, que ficou conhecido como Nuvem.

Para atender a estes diversos tipos de dispositivos citados acima
que acessam os mesmos recursos surge o conceito de Serviços. O que
se propõe é tornar o servidor e o cliente independentes, para que seja possível
ter acesso aos mesmos recursos do servidor de vários clientes
diferentes.

# Definição

Em 2000, surge o conceito do REST (do inglês _Representational State Transfer_)
criado por Roy Thomas Fielding em sua dissertação de Doutorado. Uma dos
pontos-chave deste conceito é desacoplar a interface de usuário do
armazenamento de dados, o que melhoraria a portabilidade da interface de
usuário para múltiplas plataformas \cite{rest:2000}. Esse padrão se
popularizou graças a adoção por empresas como o _Twitter_ em 2006,
que fornecedia uma API (do inglês _Application Programming Interface_)
para que terceiros pudessem fazer algum tipo de integração,
como por exemplo _login_ com a conta do _Twitter_ em seu aplicativo
ao invés de criar um sistema de _login_ próprio.

Em 2016, o _Facebook_ publica em formato _Open Source_ o GraphQL, definido
como uma _query language_ (linguagem de consulta) para _API_'s. Permite
que o cliente obtenha os dados de forma declarativa e hierarquica \cite{graphql:2016}.
É um formato que adapta-se a qualquer banco de dados ou mecanismo de
armazenamento, ou seja, é possível usá-lo para busca de arquivos em um
sistema de arquivos remoto. Possui 3 tipos de operações básicas: _Query_
para obtenção de dados; _Mutation_ para alteração de dados;
e _Subscriptions_ para notificação em tempo real.

O artigo "GraphQL vs REST: Overview" descreve com mais detalhes outras
diferenças e características \cite{sturgeon:2017}.

# Estilo Arquitetônico e Arquitetura

## REST

Seu estilo arquitetônico se encaixa em uma arquitetura em camadas,
como pode ser visto na Figura \ref{fig:rest-arch}.
Em sistemas REST há série de restrições arquiteturais, tais como:

* **Cliente-Servidor**: separar a interface de usuário da lógica da aplicação.
* **_Stateless_**: a requisição contém informações suficientes sobre o cliente
	para que o servidor entenda a requisição.
* **_Cacheable_**: requisições devem ser passíveis de serem colocadas em _cache_.
* **Interface Uniforme**: identificação de recursos é feita através de URI
	e sua implementação é feita de forma abstrata da definição da interface.
* **Sistema em Camadas**: um cliente não deverá distinguir se está conectado
	ao servidor final ou a um intermediário.

\begin{figure}[h]
	\centering
	\includegraphics[scale=1.8]{img/rest.jpg}
	\caption{Arquitetura REST}
    \label{fig:rest-arch}
\end{figure}

## GraphQL

Seu estilo arquitetônico é híbrido entre arquitetura em camadas e baseada
em eventos, esta última utilizada para notificações em tempo real
\cite{graphql-arch:2017}. Em sistemas GraphQL podem haver diversos
tipos de arquiteturas diferentes, entre elas:

* Servidor GraphQL conectado a um banco de dados, como visto na Figura
	\ref{fig:gql-dbgql-db}.
* Servidor GraphQL atuando como camada intermediária entre sistemas legados
	ou ainda de terceiros integrando-os através de uma únida API GraphQL,
    como visto na Figura \ref{fig:gql-3th-legacy}.
* Uma abordagem híbrida com a junção das 2 abordagem supracitadas,
	como visto na Figura \ref{fig:gql-hybrid}.

\begin{figure}[h]
	\centering
	\includegraphics[scale=0.25]{img/gql-db.png}
	\caption{GraphQL com banco de dados}
    \label{fig:gql-dbgql-db}
\end{figure}

\begin{figure}[h]
	\begin{minipage}{.5\textwidth}
      	\includegraphics[scale=0.2]{img/gql-3th-legacy.png}
      	\caption{GraphQL com sistemas de terceiros ou legados}
      	\label{fig:gql-3th-legacy}
    \end{minipage}
    \begin{minipage}{.5\textwidth}
    	\includegraphics[scale=0.25]{img/gql-hybrid.png}
		\caption{GraphQL híbrido}
    	\label{fig:gql-hybrid}
    \end{minipage}
\end{figure}

# Comunicação

## REST

A comunicação é feita através do protocolo HTTP, em que é recomendado o uso
explícito dos métodos do HTTP para operações: criar, ler, atualizar
e deletar \cite{rodriguez:2008}, como é mostrado a seguir:

* Para criar um recurso no servidor, utilize o método `POST`;
* Para resgatar um recurso, use `GET`;
* Para modificar um recurso, use `PUT`;
* Para deletar um recurso, use `DELETE`.

## GraphQL

A comunicação pode ser feita por qualquer protocolo, basta que para
isso seja implementado no servidor. A única restrição é que haja
somente um _endpoint_ para a conversa entre cliente e servidor.

# Nomeação

Ambas tecnologias usam sistema de nomeação estruturada, mais especificamente o DNS
(do inglês _Domain Name System_). Entretanto o REST utiliza _n-endpoint_'s,
em que cada um deles retornam um recurso diferente; e no GraphQL existe apenas
um _endpoint_, em que é passado a _query_ com a definição dos recursos
a serem retornados.

## Exemplo usando REST

Suponhamos que a URL de acesso seja `api.company.com`, e o recurso **produto**.
Teremos:

* [`GET`] `api.company.com/produtos` - Obtem uma lista de produtos
* [`GET`] `api.company.com/produto/:id` - Obtem um produto específico
* [`POST`] `api.company.com/produto` - Salva um novo produto
* [`PUT`] `api.company.com/produto/:id` - Atualiza um produto existente
* [`DELETE`] `api.company.com/produto/:id` - Deleta um produto

## Exemplo usando GraphQL

Suponha a mesma URL do exemplo anterior e o mesmo recurso. Para
obter uma lista de produtos devemos enviar a _query_ abaixo
para o _endpoint_ [`GET` ou `POST`] \newline
`api.company.com/graphql`,
teremos:

~~~
query {
	produtos(first: 10) {
    	edges {
        	node {
            	id
                descricao
                grupo_mercadoria {
                	descricao
                }
            }
        }
        pageInfo {
        	hasNextPage
       		hasPreviousPage
        	startCursor
        	endCursor
        }
    }
}
~~~

# Dificuldades e Soluções

Criar uma API usando REST chega até a ser algo trivial hoje em dia. São
muitas _frameworks_ e exemplos em todas as linguagens de programação,
como JavaScript, Java, PHP, Python, Ruby, etc \cite{awesome-rest:2017}.
Isso se deve a ampla adoção do padrão ao longo dos anos.

Antes do GraphQL outras soluções semelhantes surgiram para otimizar a
busca de recursos estendendo o padrão REST. Entretanto nenhuma delas
chegou a se tornar um padrão ou mesmo ser adotada em larga escala.
Somente com a chegada do GraphQL foi possível enxergar um novo padrão
surgir e que fizesse sentido para ser implementado nos dias atuais,
com o crescente uso de diversos dispositivos de acesso à _Internet_.

Mesmo sendo um padrão relativamente novo o GraphQL teve rápida adoção
por empresas como o GitHub, que trocou sua API REST por GraphQL. Outro
ponto importante é a ótima documentação criada para explicar seu funcionamento.
Tenha em mente que o GraphQL é apenas a especificação da linguagem,
para a execução é preciso ter um servidor e um cliente que conversem
através do GraphQL ou _Graph Query Language_. Nesse sentido uma empresa
que surgiu para atender essa necessidade foi a Apollo Data, que criou
um conjunto de ferramentas para desenvolvimento de servidores e clientes
GraphQL, até mesmo com a possibilidade de trabalhar em conjunto com serviços
REST existentes \cite{apollodata:2017}.

E assim foi feito para este trabalho, foi usado o servidor REST
`restify` \cite{restify:2017} e o `apollo-server` para integração
com GraphQL \cite{apollo-server:2017}. Para a resolução das _queries_
e mapeamento com as colunas do banco de dados foi utilizada a
ferramenta `join-monster`, que traduz GraphQL para SQL de forma
eficiente \cite{join-monster:2017}. Já para o servidor REST foi utilizada
a biblioteca `bookshelfjs`, que é uma ferramenta ORM (do inglês
Object-Relational Mapping) que auxilia na busca de dados do banco
para cada _endpoint_ \cite{bookshelfjs:2017}.

# Considerações Finais

Apesar de, às vezes, ser considerada o substituto do REST, o GraphQL
não deve ser utilizado dessa maneira apenas por ser algo novo e
inovador. Uma de suas características é exatamente ser capaz
de trabalhar em conjunto com tecnologias existentes e assim
deve ser feito.

Tomemos o exemplo do GitHub que substituiu por completo sua
API (ou v3) em REST (que por sinal ainda está em funcionamento)
\cite{github-rest:2017} por uma API GraphQL (ou v4) \cite{github-graphql:2017}.
No caso deles fez muito sentido a mudança para apenas uma tecnologia,
pois é o tipo de API que será usada por terceiros e que podem
ter necessidade de recursos diferentes, por exemplo um aplicativo
que mostre os repositórios favoritos ao redor do mundo com as pessoas
envolvidas detalhadamente; e outro que apenas diz as 10 linguagens
mais usadas nos projetos. É notável que os dois projetos irão
utilizar informações distintas e que um é mais detalhado do que
o outro.

Agora imagine uma API interna de uma empresa. Nesse caso sabemos
de antemão quais serão os recursos necessários e podemos criá-los
de forma otimizada. Em um segundo momento a empresa cria um aplicativo
_mobile_ e que deve consumir o mínimo de banda possível, isso indicaria
a necessidade de criação de um _endpoint_ GraphQL para atender essa
demanda sem ser necessário refazer a API REST existente. A ideia é
utilizar GraphQL em situações em que se busca utilização mínima
ou dinâmica de recursos.








