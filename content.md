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

Em 2015, o _Facebook_ publica em formato _Open Source_ o GraphQL, definido
como uma _query language_ (linguagem de consulta) para _API_'s. Permite
que o cliente obtenha os dados de forma declarativa e hierarquica \cite{graphql:2015}.
É um formato que adapta-se a qualquer banco de dados ou mecanismo de
armazenamento, ou seja, é possível usá-lo para busca de arquivos em um
sistema de arquivos remoto. Possui 3 tipos de operações básicas: _Query_
para obtenção de dados; _Mutation_ para alteração de dados;
e _Subscriptions_ para notificação em tempo real.

# Estilo Arquitetônico e Arquitetura

## REST

Em sistemas REST há série de restrições arquiteturais, tais como:

* **Cliente-Servidor**: separar a interface de usuário da lógica da aplicação.
* **_Stateless_**: a requisição contém informações suficientes sobre o cliente
	para que o servidor entenda a requisição.
* **_Cacheable_**: requisições devem ser passíveis de serem colocadas em _cache_.
* **Interface Uniforme**: identificação de recursos é feita através de URI
	e sua implementação é feita de forma abstrata da definição da interface.
* **Sistema em Camadas**: um cliente não deverá distinguir se está conectado
	ao servidor final ou a um intermediário.

Seu estilo arquitetônico se encaixa em uma arquitetura em camadas.

\begin{figure}[h]
	\includegraphics[scale=1.5]{img/rest.jpg}
	\caption{REST}
\end{figure}

## GraphQL

Em sistemas GraphQL podem haver diversos tipos de arquiteturas diferentes,
entre elas:

* Servidor GraphQL conectado a um banco de dados, como visto na Figura
	\ref{fig:gql-dbgql-db}.
* Servidor GraphQL atuando como camada intermediária entre sistemas legados
	ou ainda de terceiros integrando-os através de uma únida API GraphQL,
    como visto na Figura \ref{fig:gql-3th-legacy}.
* Uma abordagem híbrida com a junção das 2 abordagem supracitadas,
	como visto na Figura \ref{fig:gql-hybrid}.

Seu estilo arquitetônico é híbrido entre arquitetura em camadas e baseada
em eventos, esta última utilizada para notificações em tempo real.

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
e deletar, como é mostrado a seguir:

* Para criar um recurso no servidor, utilize o método `POST`;
* Para resgatar um recurso, use `GET`;
* Para modificar um recurso, use `PUT`;
* Para deletar um recurso, use `DELETE`.

<!-- cite https://www.ibm.com/developerworks/library/ws-restful/index.html -->

## GraphQL

A comunicação pode ser feita por qualquer protocolo, basta que para
isso seja implementado no servidor. A única restrição é que haja
somente um _endpoint_ para a conversa entre cliente e servidor.

# Nomeação

# Dificuldades e Soluções

# Considerações Finais



