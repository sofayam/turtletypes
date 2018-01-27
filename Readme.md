# Using Types in Ontologies

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

has no problem with
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

... so let's see how Hermit deals with it ...

# Hermit download

(pretty old) jar file is available from http://www.hermit-reasoner.com/

`alias  herm="java -jar ~/hermit/HermiT.jar" `

# Hermit checks

`herm meanstreets.1.ttl` led to complaints that 
```

Exception in thread "main" org.semanticweb.HermiT.datatypes.UnsupportedDatatypeException: Literals can only use the datatypes from the OWL 2 datatype map, see
http://www.w3.org/TR/owl2-syntax/#Datatype_Maps.
The datatype 'http://usetypes.plumbing/usetypes#colour' is not part of the OWL 2 datatype map and
HermiT cannot parse this literal.
```

following advice here:
https://stackoverflow.com/questions/38506988/owlapi-hermit-reasoner-thows-unsupporteddatatypeexception-for-data-type-from
I changed the datatype definition to

```
:colour rdf:type rdfs:Datatype ;
        owl:equivalentClass xsd:string .
```

but still got the same error from Hermit.

You can suppress the error by using the `--ignoreUnsupportedDatatypes` parameter but this is not an 
acceptable solution. In fact it somewhat negates the point of using Ontologies in the first place.

# This Sucks and Other People Have Noticed

I found someone here 
http://www.amberdown.net/2009/09/faq-using-rdfs-or-owl-as-a-schema-language-for-validating-rdf/ saying:

>To spoil the punch line, there isn’t yet a really good schema solution for semantic web applications but one is needed. OWL does allow you to express some (though not all) of the constraints you might like. However, to use it you may need an OWL processor which makes additional assumptions relevant to your application – a generic processor will not do the sort of validation a schema-language user is expecting.