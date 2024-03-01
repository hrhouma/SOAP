WSDL, qui signifie Web Services Description Language, est un langage basé sur XML utilisé pour décrire les services Web. Il définit la manière dont les services peuvent être appelés, quelles sont les entrées nécessaires et le type de réponse attendue. En d'autres termes, il s'agit d'un contrat entre le fournisseur et le consommateur du service Web, détaillant comment la communication entre eux doit se dérouler.

### Composants Clés d'un Document WSDL

Un document WSDL typique contient les éléments suivants :

- **`<wsdl:definitions>`** : L'élément racine qui encapsule toute la définition du service Web.
- **`<wsdl:message>`** : Définit les données échangées. Chaque message peut contenir plusieurs parties (`<wsdl:part>`).
- **`<wsdl:portType>`** (ou interface dans WSDL 2.0) : Abstraction d'un ensemble d'opérations supportées par le service web, où chaque opération correspond à un échange de messages.
- **`<wsdl:operation>`** : Une opération spécifique offerte par le service, définissant les messages d'entrée et de sortie.
- **`<wsdl:binding>`** : Spécifie le protocole et le format des données pour les opérations et les messages définis dans le `portType`.
- **`<wsdl:service>`** : Spécifie les points d'accès (`<wsdl:port>`) aux services décrits dans le WSDL.

### Exemple Expliqué
```xml
<wsdl:definitions xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"
                  xmlns:wth="http://www.webserviceX.NET/"
                  xmlns:s="http://www.w3.org/2001/XMLSchema"
                  targetNamespace="http://www.webserviceX.NET/"
                  xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/">

  <!-- Messages -->
  <wsdl:message name="GetWeatherSoapIn">
    <wsdl:part name="parameters" element="wth:GetWeatherRequest"/>
  </wsdl:message>
  <wsdl:message name="GetWeatherSoapOut">
    <wsdl:part name="parameters" element="wth:GetWeatherResponse"/>
  </wsdl:message>

  <!-- PortType -->
  <wsdl:portType name="WeatherPredictSoap">
    <wsdl:operation name="GetWeather">
      <wsdl:input message="wth:GetWeatherSoapIn" />
      <wsdl:output message="wth:GetWeatherSoapOut" />
    </wsdl:operation>
  </wsdl:portType>

  <!-- Binding -->
  <wsdl:binding name="WeatherPredictSoapBinding" type="wth:WeatherPredictSoap">
    <soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
    <wsdl:operation name="GetWeather">
      <soap:operation soapAction="http://www.webserviceX.NET/GetWeather" style="document"/>
      <wsdl:input>
        <soap:body use="literal"/>
      </wsdl:input>
      <wsdl:output>
        <soap:body use="literal"/>
      </wsdl:output>
    </wsdl:operation>
  </wsdl:binding>

  <!-- Service -->
  <wsdl:service name="WeatherPredictService">
    <wsdl:port name="WeatherPredictSoapPort" binding="wth:WeatherPredictSoapBinding">
      <soap:address location="http://www.webservicex.net/WeatherPredict.asmx"/>
    </wsdl:port>
  </wsdl:service>

</wsdl:definitions>
```

Dans l'exemple ci-haut, nous avons un extrait de contrat WSDL pour un service de prévisions météorologiques :

- **Déclarations de l'espace de noms (`xmlns`) :** Ces attributs définissent les espaces de noms XML utilisés dans le document. Par exemple, `xmlns:soap` définit l'espace de noms pour les éléments liés à SOAP, et `xmlns:wsdl` pour ceux liés à WSDL.

- **`<wsdl:definitions>` :** C'est l'élément racine qui englobe toute la définition du service Web, déclarant les espaces de noms et le namespace cible (`targetNamespace`) du service.

- **`<wsdl:message>` :** Deux messages sont définis ici, `GetWeatherSoapIn` et `GetWeatherSoapOut`, représentant respectivement la requête et la réponse pour l'opération `GetWeather`. Les détails internes (`...`) de ces messages comprendraient typiquement des définitions de structure pour les données d'entrée et de sortie.

- **`<wsdl:portType>` :** Nomme `WeatherPredictSoap` et encapsule l'opération `GetWeather`. Cet élément définit l'interface abstraite du service Web.

  - **`<wsdl:operation>` :** L'opération `GetWeather` spécifiée ici fait référence aux messages pour ses données d'entrée et de sortie à travers les attributs `message`, liant l'opération aux messages `GetWeatherSoapIn` et `GetWeatherSoapOut`.

Cet extrait de WSDL établit donc la base pour un service Web permettant de récupérer des prévisions météorologiques. 
Le client saurait, à partir de ce contrat, comment formuler une requête pour le service (`GetWeatherSoapIn`) et à quoi s'attendre en réponse (`GetWeatherSoapOut`). 
Le document inclurait également des éléments `<wsdl:binding>` et `<wsdl:service>` pour définir concrètement comment communiquer avec le service via HTTP, SOAP, ou un autre protocole.
