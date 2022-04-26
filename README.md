# experian

Objetivo: Criar um serviço do tipo API REST, para cadastro de pessoas com score e suas regiões de afinidades 


1.	Para a construção desse serviço, algumas premissas e sugestões

•	Usar linguagem Java (preferência Java 11);
•	Usar maven no build do projeto;
•	Pode criar archetype usando o start.spring.io (adicione as dependências que achar relevantes);
•	Usar framework Spring (incluindo Spring Boot, para iniciar o serviço);
•	Montar um banco de dados em memória (pode usar o H2, HSQLDB ou MongoDB), usando Hibernate na persistência de dados;
•	Necessário pelo menos um teste unitário para cada método da camada Service, usando JUnit e Mockito;
•	Colocar autenticação com token JWT (opcional)

2.	Endpoints do serviço

•	POST /pessoa
o	Informar a seguinte estrutura de dados na inclusão:
{
	“nome”: “Fulano de Tal”,
	“telefone”: “99 99999-9999”,
	“idade”: 99,
	“cidade”: “Cidade de Fulano”,
	“estado”: “XX”,
	“score”: 1000,		// Entre 0 e 1000
	“regiao”: “sudeste”
}
o	Adicionar um atributo id automático e data de inclusão, além dos dados do POST, durante inclusão dos dados no banco;
o	Retornar 201 no sucesso da inclusão;

•	POST /afinidade
o	Informar a seguinte estrutura de dados na inclusão:
{
	“regiao”: “sudeste”,
	“estados”: [
		“SP”, 
		“RJ”, 
		“MG”, 
		“ES”
	]
}
o	Retornar 201 no sucesso da inclusão;

•	POST /score
o	Informar a seguinte estrutura de dados na inclusão:
{
	“scoreDescricao”: “Insuficiente”,
	“inicial”: 0,
	“final”: 200
}
o	Retornar 201 no sucesso da inclusão;

•	GET /pessoa/{id}
o	Se id encontrado no banco, retornar a seguinte estrutura de dados:
{
	“nome”: “Fulano de Tal”,
	“telefone”: “99 99999-9999”,
	“idade”: 99,
	“scoreDescricao”: “Recomendável”,
	“estados”: [
		“SP”, 
		“RJ”, 
		“MG”, 
		“ES”
	]
}
o	Se id encontrado no banco, retornar 200, com a estrutura de dados;
o	Se id não encontrado no banco, retornar 204 (no content);

•	GET /pessoa
o	Retornar uma lista de todo o cadastro, sendo cada item da lista com a seguinte estrutura de dados:
[
	{
		“nome”: “Fulano de Tal”,
		“cidade”: “Cidade de Fulano”,
		“estado”: “XX”,
		“scoreDescricao”: “Recomendável”,
	“estados”: [
		“SP”, 
		“RJ”, 
		“MG”, 
		“ES”
	]
	},
	{
		“nome”: “Sicrano de Tal”,
		“cidade”: “Cidade de Sicrano”,
		“estado”: “XX”,
		“scoreDescricao”: “Insuficiente”,
	“estados”: [
		“RS”, 
		“PR”, 
		“SC”
	]
	}
]
o	Se algum cadastro encontrado no banco, retornar 200, com a estrutura JSON;
o	Se nenhum item encontrado no banco, retornar 204 (no content);

3.	Lógica do serviço

•	Montar lógica na camada Service, para associar a regiao da afinidade, com a regiao da pessoa, e retornar a lista de estados correspondentes à regiao.
•	Montar lógica na camada Service, para retornar o atributo scoreDescricao, correspondente ao score encontrado entre inicial e final;
•	Cadastrar via POST os seguintes dados na estrutura score:
scoreDescricao	inicial	final
Insuficiente	0	200
Inaceitável	201	500
Aceitável	501	700
Recomendável	701	1000


4.	Estrutura do Banco de dados

•	pessoa
o	id – numérico
o	dataInclusao – data
o	nome – texto
o	telefone – texto
o	idade – numérico
o	cidade – texto
o	estado – texto
o	regiao  – texto

•	afinidade
o	regiao  – texto
o	estados – lista

•	score
o	descricao  – texto
o	inicial – numérico
o	final – numérico
