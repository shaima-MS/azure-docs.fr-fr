---
title: Utiliser des boucles T-SQL
description: Conseils pour utiliser des boucles T-SQL, remplacer des curseurs et développer des solutions connexes avec un pool SQL dans SQL Synapse.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 9db7f2016de01edbedfa9e7d7254561fea957d2a
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495299"
---
# <a name="use-t-sql-loops-in-synapse-sql"></a>Utiliser des boucles T-SQL dans SQL Synapse
Cet article vous fournit des conseils essentiels pour l’utilisation de boucles T-SQL, le remplacement de curseurs et le développement de solutions connexes avec un pool SQL dans SQL Synapse.

## <a name="purpose-of-while-loops"></a>Objectif des boucles WHILE

SQL Synapse prend en charge la boucle [WHILE](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?view=sql-server-ver15) pour les blocs d’instructions dont l’exécution se répète. Cette boucle WHILE se poursuit aussi longtemps que les conditions spécifiées sont vraies, ou jusqu’à ce que le code arrête la boucle de manière spécifique, via le mot clé BREAK. 

Dans un pool SQL, les boucles s’avèrent utiles pour remplacer des curseurs définis dans le code SQL. Heureusement, presque tous les curseurs écrits en code SQL présentent le type à avance rapide, en lecture seule. Par conséquent, les boucles [WHILE] sont une alternative intéressante pour remplacer les curseurs.

## <a name="replace-cursors-in-sql-pool"></a>Remplacer des curseurs dans un pool SQL

Avant de vous lancer, posez-vous la question suivante : « Ce curseur peut-il être réécrit pour utiliser des opérations basées sur les jeux ? » Dans de nombreux cas, la réponse est oui, et cette approche est souvent la meilleure. Une opération basée sur un jeu s’exécute généralement plus rapidement qu’une méthode itérative de type ligne par ligne.

Les curseurs à avance rapide et en lecture sont facilement remplacés par des constructions en boucle. Le code suivant est un exemple simple. Cet exemple de code met à jour les statistiques de chaque table dans la base de données. En effectuant une itération sur les tables dans la boucle, chaque commande s’exécute en séquence.

Tout d’abord, créez une table temporaire contenant un numéro de ligne unique, utilisé pour identifier les instructions individuelles :

```sql
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Deuxièmement, initialisez les variables nécessaires pour exécuter la boucle :

```sql
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Ensuite, effectuez une boucle avec les instructions, en les exécutant l’une après l’autre :

```sql
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Enfin, supprimez la table temporaire créée à la première étape.

```sql
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir des conseils supplémentaires, consultez la [vue d’ensemble du développement](develop-overview.md).
