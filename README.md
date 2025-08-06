# Ontologia Alergia Alimentar
Ontologia de referência de alergênicos alimentares, usando a linguagem OntoUML como um plugin no programa Visual Paradigm (.vpp), e com a ajuda do plugin transformada em .ttl para utilização no Protégé na linguagem OWL2.

Padrões de projeto utilizados: Role Pattern, Relator Pattern, Subkind Pattern, Category Pattern

Alergia Alimentar.vpp pode ser aberto no Visual Paradigm.

Alergia Alimentar.rdf pode ser aberto no Protégé.

A pasta imagens são o resultado da modelagem para fácil acesso e visualização sem a necessidade do Visual Paradigm para ver a ontologia.


Comandos para linguagem de consulta SPARQL:

Lista de pacientes e profissionais de saúde
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX : 	<http://example.com#>
PREFIX gufo: <http://purl.org/nemo/gufo#>

#pacientes e profissionais de saúde
SELECT    ?paciente ?profissional_da_saude
	Where
{
	 ?paciente :TratadoPor ?profissional_da_saude
}
```
Lista com todos os alimentos
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX : 	<http://example.com#>
PREFIX gufo: <http://purl.org/nemo/gufo#>

SELECT ?paciente ?alergia ?reação  ?alimento
	WHERE 
{
	?alimentos rdfs:subClassOf+ :Alimento
}
```
A quais alimentos os pacientes tem alergia?
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX : 	<http://example.com#>
PREFIX gufo: <http://purl.org/nemo/gufo#>

SELECT ?paciente ?alergia ?reação  ?alimento
	WHERE 
{
 	?paciente 	rdf:type 			:Paciente;
		:pacienteHasAlergiaAlimentar 	?alergia .

	?reação	gufo:mediates 		?alergia;
		gufo:mediates 		?alimento.

	?alimento 	rdfs:subClassOf 		:Alimento	
}
```
Quais alternativas seguras existem ao alimento K para o paciente X? (note que se deve alterar onde existe ':Paciente1' para o paciente que deseja retornar a lista )
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX : 	<http://example.com#>
PREFIX gufo: <http://purl.org/nemo/gufo#>

SELECT    ?alimentos
	WHERE 
{
	?alimentos rdfs:subClassOf+ :Alimento .
 	

MINUS{	:Paciente1 	rdf:type 			:Paciente;
		:pacienteHasAlergiaAlimentar 	?alergia .

	?reação	gufo:mediates 		?alergia;
		gufo:mediates 		?alimentos .
	
	 ?alimentos	rdfs:subClassOf 		:Alimento	}
}
```
Quais alimentos comumente causam reações alérgicas entre pacientes com perfil semelhante ao de X?
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX : 	<http://example.com#>
PREFIX gufo: <http://purl.org/nemo/gufo#>

SELECT ?paciente  ?alergia  ?reaçãoCruzada ?alimento ?PorcentagemReaçãoCruzada
	WHERE 
{
 	?paciente 	rdf:type 			:Paciente;
		:pacienteHasAlergiaAlimentar 	?alergia .

	?reaçãoCruzada	gufo:historicallyDependsOn 	?alergia;
	 		gufo:mediates		?alimento;
			:PorcentagemReaçãoCruzada	?PorcentagemReaçãoCruzada
}
```
