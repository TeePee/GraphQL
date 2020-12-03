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


![image3.png](https://user-images.githubusercontent.com/38256925/101090125-751bac80-35b6-11eb-97d3-c2a7f76913b0.png)

![image4.png](https://user-images.githubusercontent.com/38256925/101090128-777e0680-35b6-11eb-9020-613adc1a541f.png)


![image5.png](https://user-images.githubusercontent.com/38256925/101090132-78af3380-35b6-11eb-9df9-b99c500d5c0b.png)


![image6.png](https://user-images.githubusercontent.com/38256925/101090137-7a78f700-35b6-11eb-8ecf-57756d4da842.png)


![image7.png](https://user-images.githubusercontent.com/38256925/101090144-7cdb5100-35b6-11eb-8729-53254ede100a.png)


![image8.png](https://user-images.githubusercontent.com/38256925/101090153-7ea51480-35b6-11eb-8890-09dfb5f247c1.png)


![image9.png](https://user-images.githubusercontent.com/38256925/101090159-806ed800-35b6-11eb-9a8c-f39a13c00925.png)


![image10.png](https://user-images.githubusercontent.com/38256925/101090166-82389b80-35b6-11eb-8269-0f01271912b4.png)


![image11.png](https://user-images.githubusercontent.com/38256925/101090169-84025f00-35b6-11eb-80f3-bad4566e889f.png)


![image12.png](https://user-images.githubusercontent.com/38256925/101090189-89f84000-35b6-11eb-9c22-7edb0459543c.png)


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

