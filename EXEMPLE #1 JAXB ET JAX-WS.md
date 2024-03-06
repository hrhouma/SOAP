JAXB (Java Architecture for XML Binding) et JAX-WS (Java API for XML Web Services) sont deux technologies importantes dans l'écosystème Java pour travailler avec des services web et XML. Voici un exemple simple qui illustre l'utilisation de JAXB pour sérialiser un objet Java en XML, et de JAX-WS pour créer un service web SOAP.

### Exemple JAXB

JAXB permet de convertir des objets Java en XML et vice-versa. Voici comment vous pourriez définir une classe Java simple et l'annoter pour la sérialisation JAXB.

```java
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement
public class Personne {
    private String nom;
    private int age;

    // Constructeur par défaut est requis pour JAXB
    public Personne() {
    }

    public String getNom() {
        return nom;
    }

    public void setNom(String nom) {
        this.nom = nom;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

Pour sérialiser l'objet `Personne` en XML, vous pouvez utiliser le code suivant :

```java
import javax.xml.bind.JAXBContext;
import javax.xml.bind.Marshaller;

public class JAXBExample {
    public static void main(String[] args) throws Exception {
        JAXBContext context = JAXBContext.newInstance(Personne.class);
        Marshaller marshaller = context.createMarshaller();
        marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);

        Personne personne = new Personne();
        personne.setNom("Jean Dupont");
        personne.setAge(30);

        // Sérialiser en XML
        marshaller.marshal(personne, System.out);
    }
}
```

### Exemple JAX-WS

JAX-WS est une API pour créer des services web SOAP. Voici un exemple simple de service web qui utilise JAX-WS pour exposer une opération web.

D'abord, définissez l'interface du service web :

```java
import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public interface GreetingService {
    @WebMethod
    String sayHello(String name);
}
```

Ensuite, implémentez cette interface :

```java
import javax.jws.WebService;

@WebService(endpointInterface = "exemple.GreetingService")
public class GreetingServiceImpl implements GreetingService {
    @Override
    public String sayHello(String name) {
        return "Bonjour, " + name + "!";
    }
}
```

Pour publier le service web :

```java
import javax.xml.ws.Endpoint;

public class Publisher {
    public static void main(String[] args) {
        Endpoint.publish("http://localhost:8080/ws/hello", new GreetingServiceImpl());
        System.out.println("Service publié à l'adresse http://localhost:8080/ws/hello");
    }
}
```

Ce code démarre un serveur de service web simple qui écoute sur `http://localhost:8080/ws/hello` et retourne un message de salutation.

Ces exemples montrent les bases de JAXB pour la conversion entre objets Java et XML, et de JAX-WS pour la création et la publication de services web SOAP. Ces technologies sont puissantes pour le développement d'applications basées sur des services web en Java.
