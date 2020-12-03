# GraphQL

**Vulnérabilités courantes et comment les exploiter**
https://the-bilal-rizwan.medium.com/graphql-common-vulnerabilities-how-to-exploit-them-464f9fdce696

De quoi parle ce post?
Beaucoup d'entre vous ont peut-être vu GraphQL utilisé dans de nombreuses applications Web, 
certains d'entre vous ont peut-être reconnu tout de suite que son graphql et ont probablement essayé de rechercher ce que vous pouvez en faire, 
certains n'ont peut-être pas réalisé que la requête s'appelle Requête GraphQL.
Dans cet article, je vais essayer de mettre en évidence les erreurs de configuration courantes, 
dans l'utilisation de GraphQL et comment elles peuvent être exploitées.

Pour ceux qui ne savent pas ce qu'est GraphQL, sa demande ressemble à ceci.
![image1.png](https://raw.githubusercontent.com/TeePee/GraphQL/main/Image1.png)

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






