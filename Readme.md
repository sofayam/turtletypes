An attempt to use open source ontology tools to get stuff done

meanstreets.ttl produced with Protege with intentional failure (shade of myTruck is typed as a speed rather than a colour)

pellet installed next door from https://github.com/stardog-union/pellet

# Pellet build and run

Lots of  warnings  during both build "mvn package install" and run.


Use this alias for something that will actually work as
opposed to the top level shell script which crashes)

`alias pele="sh ~/repos/pellet/cli/target/pelletcli/bin/pellet" `

# Pellet consistency checks

`pele consistency meanstreets.ttl`

will complain about the intentional bad setting of attribute of the wrong type here

```
:myTruck rdf:type owl:NamedIndividual ,
                  :Car ;
         :shade "green"^^:kmh ;
         :speed "50"^^:kmh .
```
BUT

`pele consistency meanstreets.1.ttl` 

has not problem with
```
:myTruck rdf:type owl:NamedIndividual ,
                  :Car ;
         :shade "green"^^:colour ;
         :speed "blue"^^:kmh .
```

although the value of blue cannot be a value of kmh which has previously been specified to be a subclass of integer thusly
```
:kmh rdf:type rdfs:Datatype ;
        rdfs:subClassOf xsd:integer .
```