# GraphQL

**Vulnérabilités courantes et comment les exploiter**
[Site Original](https://the-bilal-rizwan.medium.com/graphql-common-vulnerabilities-how-to-exploit-them-464f9fdce696)

De quoi parle ce post?
Beaucoup d'entre vous ont peut-être vu GraphQL utilisé dans de nombreuses applications Web, 
certains d'entre vous ont peut-être reconnu tout de suite que son graphql et ont probablement essayé de rechercher ce que vous pouvez en faire, 
certains n'ont peut-être pas réalisé que la requête s'appelle Requête GraphQL.
Dans cet article, je vais essayer de mettre en évidence les erreurs de configuration courantes, 
dans l'utilisation de GraphQL et comment elles peuvent être exploitées.

Pour ceux qui ne savent pas ce qu'est GraphQL, sa demande ressemble à ceci.

![image1.png](https://user-images.githubusercontent.com/38256925/101089345-5537b900-35b5-11eb-99db-b54152d1028a.png)

Il comporte des caractères **accolades** et **\n**. Si vous voyez quelque chose comme ça, c'est probablement GraphQL.
Commençons par comprendre ce que GraphQL est réellement en sachant que cela aidera à mieux former des exploits.

# Qu'est-ce que GraphQL?

    En bref, GraphQL est un standard API alternatif comme REST et SOAP. 
    Il s'agit essentiellement d'un langage de requête pour les API utilisé pour interagir avec les API et pour récupérer les données du backend via les API. 
    Il peut faire tout ce que l'API REST peut faire, mais d'une manière beaucoup plus efficace et contrôlée.
    GraphQL résout de nombreux problèmes rencontrés lors de l'utilisation de REST, 
    tels que la récupération de plus de données qu'il ne le devrait ou la nécessité d'avoir un nouveau point de terminaison pour chaque appel.
    
L'exemple suivant devrait effacer la différence entre GraphQL et l'API REST.

Dans l'API REST, nous utilisons généralement `/users/<id>` endpoint pour récupérer les données utilisateur. 
Deuxièmement, il y a probablement un point de terminaison `/users/<id>/posts` qui renvoie toutes les publications d'un utilisateur. 
Le troisième point de terminaison sera alors le `/users/<id>/followers` qui renvoie une liste de followers par utilisateur.

![image2.png](https://user-images.githubusercontent.com/38256925/101089348-579a1300-35b5-11eb-92a7-cf4fc1ee6736.png)

Dans GraphQL cependant, il n'y a qu'un seul point de terminaison où nous envoyons une requête qui inclut des exigences de données concrètes, 
le serveur répond ensuite avec les exigences de données.
Supposons que nous voulions récupérer l'ID utilisateur du système, nous pouvons lui faire une requête comme ceci:

![image3.png](https://user-images.githubusercontent.com/38256925/101090125-751bac80-35b6-11eb-97d3-c2a7f76913b0.png)

Maintenant, que faire si nous voulons également les e-mails des utilisateurs? 
et contrairement à l'API REST, nous pouvons simplement spécifier que dans une nouvelle ligne, 
sur le même point de terminaison dans la demande est envoyée au serveur et c'est aussi simple que cela:

![image4.png](https://user-images.githubusercontent.com/38256925/101090128-777e0680-35b6-11eb-9020-613adc1a541f.png)

Et si nous voulions aussi les noms? Je suis sûr que vous êtes maintenant en mesure de le comprendre.

![image5.png](https://user-images.githubusercontent.com/38256925/101090132-78af3380-35b6-11eb-9df9-b99c500d5c0b.png)

Et c'est l'embellissement de Graphql, vous pouvez simplement spécifier ce que vous voulez de manière granulaire.
Maintenant que vous savez ce qu'est GraphQL, passons à autre chose.

# Erreurs de configuration courantes dans GraphQL

Ce qu'il faut comprendre ici, c'est que GraphQL, comme toute autre API REST, 
est vulnérable à de nombreuses attaques auxquelles les mêmes attaques que l'API REST pourrait être sujette. 
Je vais en énumérer quelques-une ci-dessous, 
mais la chose la plus intéressante et la raison pour laquelle tout ce message est rédigé est le tristement célèbre bogue de requête Introspection.

   **Requête d'introspection:** 
`En termes simples, il s'agit d'un moyen d'interroger le serveur pour son schéma d'arrière-plan GraphQL 
et d'obtenir une documentation complète et une liste des appels d'API disponibles dans le back-end.
Ceci est à l'origine destiné à être utilisé en interne.`

La requête d'introspection ne doit être autorisée qu'en interne et ne doit pas être autorisée au grand public. 
Si nous pouvons récupérer toute la documentation de l'API back-end et les appels disponibles sur un serveur, 
cela peut être très dangereux dans de nombreux cas, 
et si nous pouvions mettre la main sur certains appels d'API destinés uniquement à être utilisés en interne, 
peut-être que nous trouvons un appel pour activer le débogage ou peut-être un appel d'API pour supprimer des utilisateurs, 
il y a tellement de dégâts qui peuvent être causés si tout le back-end peut être récupéré.

Voyons comment cela se fait.

    Pour tester un serveur pour une mauvaise configuration d'introspection GraphQL:
    1) Intercepter la requête HTTP envoyée au serveur
    2) Remplacez son contenu/requête par une requête d'introspection générique pour récupérer l'intégralité du schéma de backend
    3) Visualisez le schéma pour collecter des appels API juteux.
    4) Créez tout appel GraphQL potentiel que vous pourriez trouver intéressant et piratez!
    
Supposons que votre application Web cible effectue un appel GraphQL, 
vous pouvez simplement modifier sa requête avec une requête GraphQL Introspection comme suit.

                                            Appel API d'origine

![image6.png](https://user-images.githubusercontent.com/38256925/101090137-7a78f700-35b6-11eb-8ecf-57756d4da842.png)

                                            Requête d'introspection GraphQL Schéma back-end qui fuit

![image7.png](https://user-images.githubusercontent.com/38256925/101090144-7cdb5100-35b6-11eb-8729-53254ede100a.png)

Remplacez simplement le contenu du POST par la requête suivante:

```
{"query":"\n    query IntrospectionQuery {\r\n      __schema {\r\n        queryType { name }\r\n        mutationType { name }\r\n        subscriptionType { name }\r\n        types {\r\n          ...FullType\r\n        }\r\n        directives {\r\n          name\r\n          description\r\n          locations\r\n          args {\r\n            ...InputValue\r\n          }\r\n        }\r\n      }\r\n    }\r\n\r\n    fragment FullType on __Type {\r\n      kind\r\n      name\r\n      description\r\n      fields(includeDeprecated: true) {\r\n        name\r\n        description\r\n        args {\r\n          ...InputValue\r\n        }\r\n        type {\r\n          ...TypeRef\r\n        }\r\n        isDeprecated\r\n        deprecationReason\r\n      }\r\n      inputFields {\r\n        ...InputValue\r\n      }\r\n      interfaces {\r\n        ...TypeRef\r\n      }\r\n      enumValues(includeDeprecated: true) {\r\n        name\r\n        description\r\n        isDeprecated\r\n        deprecationReason\r\n      }\r\n      possibleTypes {\r\n        ...TypeRef\r\n      }\r\n    }\r\n\r\n    fragment InputValue on __InputValue {\r\n      name\r\n      description\r\n      type { ...TypeRef }\r\n      defaultValue\r\n    }\r\n\r\n    fragment TypeRef on __Type {\r\n      kind\r\n      name\r\n      ofType {\r\n        kind\r\n        name\r\n        ofType {\r\n          kind\r\n          name\r\n          ofType {\r\n            kind\r\n            name\r\n            ofType {\r\n              kind\r\n              name\r\n              ofType {\r\n                kind\r\n                name\r\n                ofType {\r\n                  kind\r\n                  name\r\n                  ofType {\r\n                    kind\r\n                    name\r\n                  }\r\n                }\r\n              }\r\n            }\r\n          }\r\n        }\r\n      }\r\n    }\r\n  ","variables":null}
```

Maintenant, quand vous faites cela, la réponse peut être assez grande et difficile à comprendre. 
La meilleure façon de comprendre le schéma est de le visualiser. 
Cela peut être fait en copiant tout le corps de la réponse et en utilisant ce site Web https://apis.guru/graphql-voyager/ 
cliquez sur le bouton Changer de schéma et allez dans l'onglet introspection puis collez-y la requête d'introspection

![image8.png](https://user-images.githubusercontent.com/38256925/101090153-7ea51480-35b6-11eb-8890-09dfb5f247c1.png)

après avoir collé le schéma, cliquez sur Afficher et le tour est joué, 
vous verrez une visualisation de l'ensemble des appels de back-end et d'API disponibles

![image9.png](https://user-images.githubusercontent.com/38256925/101090159-806ed800-35b6-11eb-9a8c-f39a13c00925.png)

Maintenant que nous avons la liste complète des appels d'API, 
nous pouvons la parcourir et essayer facilement de déterminer s'il existe des appels d'API sensibles qui peuvent être abusés. 
C'est le type de bogue le plus répandu dans les back-ends GraphQL qui peut conduire à des scénarios assez critiques.
                                        ____________________________________
                                        
J'ai un peu l'impression que jusqu'à présent, vous n'avez peut-être pas complètement synchronisé le véritable impact de ce bogue, 
alors prenons un exemple.Pour ceux d'entre vous qui savent déjà comment regarder un schéma graphql et créer des requêtes graphQL, 
la lecture plus approfondie ne sera pas assez utile, mais pour les personnes qui ont visualisé le schéma, 
il a repéré quelque chose de sensible et qui veut l'exploiter ou qui veut simplement comprendre le véritable impact de l'introspection, 
puis continuez à lire.

Apprentissage de la formation des requêtes GraphQL:
    GraphQL a 3 types de base de requêtes ou de composants

    Queries/Requêtes
    Semblable à la requête GET dans l'API REST, les requêtes sont utilisées pour récupérer des données.

    Mutation
    Utilisé pour créer, modifier et supprimer des données.

    Subscriptions/Abonnements
    Utilisé pour la communication en temps réel. Nous ne nous concentrerons pas là-dessus.

Toutes les requêtes graphQL sont écrites au format json avec des sauts de ligne et des accolades.
Prenons un exemple et utilisons une application Web qui a 2 fonctionnalités
1) Liste des utilisateurs
2) Ajouter des utilisateurs
Nous utiliserons ensuite une requête d'introspection pour trouver des bogues dans l'application, 
c'est quelque chose que l'on trouve couramment dans les applications Web.

![image10.png](https://user-images.githubusercontent.com/38256925/101090166-82389b80-35b6-11eb-8269-0f01271912b4.png)

L'image ci-dessus est ce que vous verriez normalement maintenant pour améliorer cela, 
vous pouvez utiliser un plugin burp comme JSON Beautifier ou mieux encore un plus spécifique pour GraphQL connu sous le nom de GraphQL Raider. 
Après l'avoir installé, vous aurez un onglet comme JSON beautifer où que ce soit GraphQL

![image11.png](https://user-images.githubusercontent.com/38256925/101090169-84025f00-35b6-11eb-80f3-bad4566e889f.png)

Maintenant, ce qui précède peut être vu d'une bien meilleure manière

![image12.png](https://user-images.githubusercontent.com/38256925/101090189-89f84000-35b6-11eb-9c22-7edb0459543c.png)

À partir des images, vous devriez être en mesure de comprendre facilement la structure de la requête graphQL


![image13.png](https://user-images.githubusercontent.com/38256925/101090194-8c5a9a00-35b6-11eb-9b32-cd9570c8f2bc.png)


![image14.png](https://user-images.githubusercontent.com/38256925/101090198-8e245d80-35b6-11eb-959b-112e25778d22.png)


![image15.png](https://user-images.githubusercontent.com/38256925/101090200-8f558a80-35b6-11eb-8de4-f788f372aebb.png)


![image16.png](https://user-images.githubusercontent.com/38256925/101090207-91b7e480-35b6-11eb-88ce-612df2dd7f6c.png)


![image17.png](https://user-images.githubusercontent.com/38256925/101090211-92e91180-35b6-11eb-91ff-5e68aad75075.png)


![image18.png](https://user-images.githubusercontent.com/38256925/101090215-94b2d500-35b6-11eb-93de-3aea167a885e.png)


![image19.png](https://user-images.githubusercontent.com/38256925/101090219-97152f00-35b6-11eb-8526-7b81e91a1fc1.png)


![image20.png](https://user-images.githubusercontent.com/38256925/101090224-98465c00-35b6-11eb-8556-9416a20c2570.png)


![image21.png](https://user-images.githubusercontent.com/38256925/101090225-9a101f80-35b6-11eb-8358-fb828ffbcf95.png)


![image22.png](https://user-images.githubusercontent.com/38256925/101090232-9b414c80-35b6-11eb-8449-ba0ae58f17e7.png)


![image23.png](https://user-images.githubusercontent.com/38256925/101090236-9d0b1000-35b6-11eb-9225-425f2f805feb.png)


![image24.png](https://user-images.githubusercontent.com/38256925/101090240-9e3c3d00-35b6-11eb-8cd2-7c9b701e110c.png)


![image25.png](https://user-images.githubusercontent.com/38256925/101090244-a09e9700-35b6-11eb-85d2-daec7de2e288.png)


![image26.png](https://user-images.githubusercontent.com/38256925/101090249-a1cfc400-35b6-11eb-8896-d63f79e4e5b9.png)


![image27.png](https://user-images.githubusercontent.com/38256925/101090253-a300f100-35b6-11eb-83d0-8c1fabea5d30.png)


![image28.png]()


![image29.png]()

