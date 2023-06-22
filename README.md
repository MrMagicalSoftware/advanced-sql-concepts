# advanced-sql-concepts


![Schermata 2023-06-20 alle 12 33 09](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/c91fd770-db5a-4d35-b767-753b04f3f471)


Window function chiamate anche analytical function , sono un tipo di funzioni di sql che permettono di eseguire calcoli sui dati in differenti gruppi di righe.


In SQL, la termine "window function" si riferisce a una categoria di funzioni che eseguono un calcolo su un set di righe, chiamato finestra, che sono in qualche modo correlate alla riga corrente. Queste funzioni possono essere utilizzate per eseguire operazioni che sono normalmente troppo complesse per essere realizzate con aggregazioni standard o operazioni di raggruppamento.

Le window functions non restituiscono un singolo valore per l'intero set di righe, ma piuttosto un valore per ogni riga, basato sulla finestra di righe correlata. Le righe incluse nella finestra possono essere definite in vari modi, ad esempio, possono essere tutte le righe in una partizione, o un range di righe che circondano la riga corrente.

Per utilizzare una window function in SQL, si utilizza la sintassi OVER, che può includere le clausole PARTITION BY e ORDER BY. PARTITION BY è utilizzato per dividere il set di righe in partizioni sulle quali la window function opera, mentre ORDER BY determina l'ordine delle righe all'interno di ciascuna partizione.

Esempio di una window function in SQL:


```
SELECT
    name,
    salary,
    AVG(salary) OVER (PARTITION BY department) AS avg_salary_by_department
FROM
    employees;


```



In questo esempio, la funzione AVG è una window function che calcola la media degli stipendi per dipartimento. La clausola OVER con PARTITION BY department indica che il calcolo deve essere fatto separatamente per ciascun dipartimento.


Esempio :


![Schermata 2023-06-20 alle 12 45 04](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/91757bff-d80b-4a3a-a7c7-f68135484241)



![Schermata 2023-06-20 alle 12 45 54](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/43016333-2bc1-448a-bd56-19f4a2f1236c)



![Schermata 2023-06-20 alle 12 47 38](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/2b3e9dc8-8124-4cf3-aba3-f7fb2cfd59e9)



![Schermata 2023-06-20 alle 12 49 35](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/39253ff2-1a77-434b-a52f-5623c3052a73)


![Schermata 2023-06-20 alle 12 50 16](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/443a0fff-044a-496f-a5ab-9a488915317b)


Creiamo la nostra window function :

```
SELECT
order_id,
order_date,
order_total.

SUM ( order_total ) OVER (
  ORDER BY order_id ASC
) AS running_total
FROM orders
ORDER BY Order_id ASC;
```

![Schermata 2023-06-20 alle 12 57 52](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/2ff2a0e6-1953-4c83-8272-925e6d8f21e4)


FUNZIONE DI RIPARTIZIONE NELLE WINDOW FUNCTION :

DEFINIZIONE DI PARTIZIONE :

"partizione" implica una suddivisione o segmentazione di un insieme o di una struttura in parti più piccole o in sottoinsiemi.

una partizione di un insieme è una suddivisione di quell'insieme in sottoinsiemi non vuoti, in modo tale che ogni elemento dell'insieme appartenga esattamente a uno dei sottoinsiemi. Per esempio, una partizione dell'insieme {1, 2, 3} 
potrebbe essere {{1, 2}, {3}}.

una partizione è un sottoinsieme di righe all'interno di un set di dati su cui una window function può operare. La clausola PARTITION BY viene utilizzata per dividere il set di righe in gruppi o partizioni in base ai valori in una o più colonne. Per esempio, se si hanno dati su dipendenti e si vuole calcolare la media dello stipendio per dipartimento, si può partizionare i dati in base al dipartimento e poi applicare la window function ad ogni partizione separatamente.


![Schermata 2023-06-20 alle 13 02 28](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/dae5472d-46fe-475b-b269-81b4534360f5)


![Schermata 2023-06-20 alle 13 04 35](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/1964631a-55cf-4b75-a21d-46d5aca0d9d5)



ANDIAMO A PARTIZIONARE SECONDO UN CRITERIO:




```
SELECT
order_id,
order_date,
order_total.

SUM ( order_total ) OVER (
  PARTITION BY order_date
  ORDER BY order_id ASC
) AS running_total
FROM orders
ORDER BY Order_id ASC;
```

Possiamo notare cge ordinando per id il risultato non è chiaro quindi
modifichiamo il campo nel seguente modo :


```
SELECT
order_id,
order_date,
order_total.

SUM ( order_total ) OVER (
  PARTITION BY order_date
  ORDER BY order_id ASC
) AS running_total
FROM orders
ORDER BY order_date ASC;
```

![Schermata 2023-06-20 alle 13 17 33](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/f5cf07ea-6f71-459c-92af-1cb597d35223)


![Schermata 2023-06-20 alle 13 19 21](https://github.com/MrMagicalSoftware/advanced-sql-concepts/assets/98833112/6860b387-b720-478a-8a54-1a1532148747)




Certamente! Di seguito troverai 10 esercizi con relative soluzioni e codice utilizzando le Window Function con la clausola OVER in SQL Server utilizzando AdventureWorks 2014 come esempio.

Esercizio 1: Calcola la somma totale degli importi degli ordini per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, SUM(TotalDue) OVER (PARTITION BY CustomerID) AS TotalAmount
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione di aggregazione SUM con la clausola OVER per calcolare la somma totale degli importi degli ordini per ogni cliente.

Esercizio 2: Trova il numero totale di ordini effettuati da ogni cliente e mostra anche la somma cumulativa dei totali degli ordini.
Soluzione:

```sql
SELECT CustomerID, COUNT(*) OVER (PARTITION BY CustomerID) AS TotalOrders,
       SUM(TotalDue) OVER (PARTITION BY CustomerID ORDER BY OrderDate) AS CumulativeTotal
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo le funzioni di aggregazione COUNT e SUM con la clausola OVER per calcolare il numero totale di ordini per ogni cliente e la somma cumulativa dei totali degli ordini.

Esercizio 3: Calcola la media degli importi degli ordini per ogni cliente e mostra anche la media cumulativa degli importi.
Soluzione:

```sql
SELECT CustomerID, TotalDue,
       AVG(TotalDue) OVER (PARTITION BY CustomerID) AS AverageAmount,
       AVG(TotalDue) OVER (PARTITION BY CustomerID ORDER BY OrderDate) AS CumulativeAverage
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione di aggregazione AVG con la clausola OVER per calcolare la media degli importi degli ordini per ogni cliente e la media cumulativa degli importi.

Esercizio 4: Trova il valore massimo e minimo degli importi degli ordini per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, TotalDue,
       MAX(TotalDue) OVER (PARTITION BY CustomerID) AS MaxAmount,
       MIN(TotalDue) OVER (PARTITION BY CustomerID) AS MinAmount
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo le funzioni di aggregazione MAX e MIN con la clausola OVER per trovare il valore massimo e minimo degli importi degli ordini per ogni cliente.

Esercizio 5: Calcola la differenza tra l'importo di ciascun ordine e la media degli importi degli ordini per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, TotalDue,
       TotalDue - AVG(TotalDue) OVER (PARTITION BY CustomerID) AS AmountDifference
FROM Sales.SalesOrderHeader;
```
In questo esempio, sottraiamo la media degli importi degli ordini per ogni cliente dall'importo di ciascun ordine utilizzando la funzione AVG con la clausola OVER.

Esercizio 6: Calcola la percentuale dell'importo di ciascun ordine rispetto all'importo totale degli ordini per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, TotalDue,
       (TotalDue / SUM(TotalDue) OVER (PARTITION BY CustomerID)) * 100 AS

 AmountPercentage
FROM Sales.SalesOrderHeader;
```
In questo esempio, calcoliamo la percentuale dell'importo di ciascun ordine rispetto all'importo totale degli ordini per ogni cliente utilizzando la funzione SUM con la clausola OVER.

Esercizio 7: Trova il valore massimo dell'importo di ciascun ordine per ogni anno.
Soluzione:

```sql
SELECT OrderDate, TotalDue,
       MAX(TotalDue) OVER (PARTITION BY YEAR(OrderDate)) AS MaxAmount
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione MAX con la clausola OVER per trovare il valore massimo dell'importo di ciascun ordine per ogni anno utilizzando la funzione YEAR per estrarre l'anno dalla colonna OrderDate.

Esercizio 8: Calcola la differenza tra l'importo di ciascun ordine e l'importo massimo degli ordini per ogni anno.
Soluzione:

```sql
SELECT OrderDate, TotalDue,
       TotalDue - MAX(TotalDue) OVER (PARTITION BY YEAR(OrderDate)) AS AmountDifference
FROM Sales.SalesOrderHeader;
```
In questo esempio, sottraiamo l'importo massimo degli ordini per ogni anno dall'importo di ciascun ordine utilizzando la funzione MAX con la clausola OVER.

Esercizio 9: Trova il numero di ordini effettuati da ogni cliente e ordina i risultati in base al numero di ordini in ordine decrescente.
Soluzione:

```sql
SELECT CustomerID, COUNT(*) OVER (PARTITION BY CustomerID) AS TotalOrders
FROM Sales.SalesOrderHeader
ORDER BY TotalOrders DESC;
```
In questo esempio, utilizziamo la funzione di aggregazione COUNT con la clausola OVER per calcolare il numero di ordini effettuati da ogni cliente e quindi ordiniamo i risultati in base al numero di ordini in ordine decrescente.

Esercizio 10: Calcola la somma cumulativa degli importi degli ordini per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, TotalDue,
       SUM(TotalDue) OVER (PARTITION BY CustomerID ORDER BY OrderDate) AS CumulativeAmount
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione di aggregazione SUM con la clausola OVER per calcolare la somma cumulativa degli importi degli ordini per ogni cliente, ordinata per la colonna OrderDate.

Questi esempi mostrano come utilizzare le Window Function con la clausola OVER per risolvere diversi tipi di problemi analitici utilizzando AdventureWorks 2014 come database di esempio.

________________________




Esercizio 1: Trova il numero di ordini effettuati in ogni anno e calcola la percentuale rispetto al numero totale di ordini.
Soluzione:

```sql
SELECT YEAR(OrderDate) AS OrderYear,
       COUNT(*) OVER (PARTITION BY YEAR(OrderDate)) AS TotalOrders,
       (COUNT(*) OVER (PARTITION BY YEAR(OrderDate)) / CAST(COUNT(*) OVER () AS FLOAT)) * 100 AS Percentage
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione COUNT con la clausola OVER per calcolare il numero di ordini effettuati in ogni anno e la funzione COUNT senza la clausola OVER per calcolare il numero totale di ordini. Quindi calcoliamo la percentuale dividendo il numero di ordini in ogni anno per il numero totale di ordini.

Esercizio 2: Calcola il saldo cumulativo degli ordini per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, OrderDate, TotalDue,
       SUM(TotalDue) OVER (PARTITION BY CustomerID ORDER BY OrderDate ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CumulativeBalance
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione SUM con la clausola OVER e la clausola ROWS BETWEEN per calcolare il saldo cumulativo degli ordini per ogni cliente. La clausola ROWS BETWEEN specifica che la somma deve essere calcolata dalla riga di partenza (UNBOUNDED PRECEDING) fino alla riga corrente (CURRENT ROW).

Esercizio 3: Trova il numero di giorni trascorsi tra l'ordine corrente e l'ultimo ordine effettuato da ogni cliente.
Soluzione:

```sql
SELECT CustomerID, OrderDate,
       DATEDIFF(DAY, LAG(OrderDate) OVER (PARTITION BY CustomerID ORDER BY OrderDate), OrderDate) AS DaysSinceLastOrder
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione LAG con la clausola OVER per ottenere la data dell'ultimo ordine effettuato da ogni cliente. Quindi utilizziamo la funzione DATEDIFF per calcolare il numero di giorni trascorsi tra l'ordine corrente e l'ultimo ordine.

Esercizio 4: Calcola la media mobile degli importi degli ordini per ogni cliente considerando gli ultimi 3 ordini.
Soluzione:

```sql
SELECT CustomerID, OrderDate, TotalDue,
       AVG(TotalDue) OVER (PARTITION BY CustomerID ORDER BY OrderDate ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS MovingAverage
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione AVG con la clausola OVER e la clausola ROWS BETWEEN per calcolare la media mobile degli importi degli ultimi 3 ordini per ogni cliente. La clausola ROWS BETWEEN specifica che la media deve essere calcolata considerando le 2 righe precedenti (2 PRECEDING) oltre alla riga corrente (CURRENT ROW).

Esercizio 5: Trova il cliente con l'import

o totale più alto degli ordini.
Soluzione:

```sql
SELECT CustomerID, SUM(TotalDue) AS TotalAmount
FROM Sales.SalesOrderHeader
GROUP BY CustomerID
HAVING SUM(TotalDue) = (SELECT MAX(TotalAmount) FROM (SELECT SUM(TotalDue) AS TotalAmount FROM Sales.SalesOrderHeader GROUP BY CustomerID) AS subquery);
```
In questo esempio, utilizziamo la funzione SUM con la clausola OVER per calcolare l'importo totale degli ordini per ogni cliente. Quindi utilizziamo una subquery per trovare il massimo importo totale e confrontarlo con l'importo totale di ciascun cliente nella clausola HAVING per selezionare il cliente corrispondente.

Esercizio 6: Trova il cliente con il numero massimo di ordini effettuati.
Soluzione:

```sql
SELECT CustomerID, COUNT(*) AS TotalOrders
FROM Sales.SalesOrderHeader
GROUP BY CustomerID
HAVING COUNT(*) = (SELECT MAX(TotalOrders) FROM (SELECT COUNT(*) AS TotalOrders FROM Sales.SalesOrderHeader GROUP BY CustomerID) AS subquery);
```
In questo esempio, utilizziamo la funzione COUNT con la clausola OVER per calcolare il numero di ordini effettuati per ogni cliente. Quindi utilizziamo una subquery per trovare il massimo numero di ordini e confrontarlo con il numero di ordini di ciascun cliente nella clausola HAVING per selezionare il cliente corrispondente.

Esercizio 7: Trova la data dell'ultimo ordine effettuato per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, MAX(OrderDate) OVER (PARTITION BY CustomerID) AS LastOrderDate
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione MAX con la clausola OVER per trovare la data dell'ultimo ordine effettuato per ogni cliente.

Esercizio 8: Trova la data dell'ordine più vecchio effettuato per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, MIN(OrderDate) OVER (PARTITION BY CustomerID) AS OldestOrderDate
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione MIN con la clausola OVER per trovare la data dell'ordine più vecchio effettuato per ogni cliente.

Esercizio 9: Trova il numero di ordini consecutivi effettuati da ogni cliente.
Soluzione:

```sql
SELECT CustomerID, OrderDate,
       ROW_NUMBER() OVER (PARTITION BY CustomerID ORDER BY OrderDate) AS ConsecutiveOrderNumber
FROM Sales.SalesOrderHeader;
```
In questo esempio, utilizziamo la funzione ROW_NUMBER con la clausola OVER per assegnare un numero sequenziale ai record di ogni cliente, ordinati per data dell'ordine. Questo numero rappresenta il numero di ordine consecutivo per ogni cliente.

Esercizio 10: Calcola la differenza in giorni tra la data dell'ordine corrente e la data dell'ordine successivo per ogni cliente.
Soluzione:

```sql
SELECT CustomerID, OrderDate,
       DATEDIFF(DAY, OrderDate, LEAD(OrderDate) OVER (PARTITION BY CustomerID ORDER BY OrderDate)) AS DaysToNextOrder
FROM Sales.SalesOrderHeader;
```
In questo esempio,

 utilizziamo la funzione LEAD con la clausola OVER per ottenere la data dell'ordine successivo per ogni cliente. Quindi utilizziamo la funzione DATEDIFF per calcolare il numero di giorni tra la data dell'ordine corrente e la data dell'ordine successivo.


## ESERCIZI DI OTTIMIZZAZIONE 


Creare query SQL ottimizzate è molto importante per garantire un utilizzo efficiente delle risorse del database e velocizzare il recupero dei dati.
Si noti che per ottimizzare le query, ci si concentra su diverse strategie come l'uso di indici, evitando l'uso di funzioni negli operatori WHERE, la riduzione delle chiamate a tabelle di grandi dimensioni, l'uso di operatori di confronto più efficienti e l'uso di clausole EXISTS anziché IN quando possibile.

1. Esercizio: 

   Query originale:
    ```sql
    SELECT * FROM Sales.SalesOrderDetail
    WHERE ProductID = (SELECT ProductID FROM Production.Product WHERE Name = 'Road-650 Red, 48');
    ```
   Ottimizzazione:
   
   Questa query può essere ottimizzata eliminando la subquery e utilizzando un JOIN.
   
   Query ottimizzata:
    ```sql
    SELECT sod.* 
    FROM Sales.SalesOrderDetail sod
    JOIN Production.Product p ON sod.ProductID = p.ProductID
    WHERE p.Name = 'Road-650 Red, 48';
    ```

2. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.Customer 
    WHERE TerritoryID IN (SELECT TerritoryID FROM Sales.SalesTerritory WHERE Name = 'Northwest');
    ```
    Ottimizzazione:
    
    Invece di utilizzare la clausola IN, possiamo utilizzare un JOIN per ottimizzare la query.
    
    Query ottimizzata:
    ```sql
    SELECT c.*
    FROM Sales.Customer c
    JOIN Sales.SalesTerritory st ON c.TerritoryID = st.TerritoryID
    WHERE st.Name = 'Northwest';
    ```

3. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Production.Product 
    WHERE ListPrice > (SELECT AVG(ListPrice) FROM Production.Product);
    ```
    Ottimizzazione:
    
    Possiamo migliorare l'efficienza calcolando la media una volta e memorizzandola in una variabile.
    
    Query ottimizzata:
    ```sql
    DECLARE @average_price DECIMAL(18, 2);
    SELECT @average_price = AVG(ListPrice) FROM Production.Product;
    SELECT * FROM Production.Product WHERE ListPrice > @average_price;
    ```

4. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Production.Product
    WHERE Name LIKE '%Road%';
    ```
    Ottimizzazione:
    
    Utilizzare l'operatore LIKE con un jolly all'inizio può essere costoso. Se possibile, limitare l'uso dei jolly.
    
    Query ottimizzata:
    ```sql
    SELECT * FROM Production.Product
    WHERE Name LIKE 'Road%';
    ```

5. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.SalesOrderHeader
    WHERE YEAR(OrderDate) = 2014 AND MONTH(OrderDate) = 7;
    ```
    Ottimizzazione:
    
    Utilizzare funzioni sugli attributi nella clausola WHERE può impedire l'uso degli indici. E' meglio utilizzare una range query.
    
    Query ottimizzata:
    ```sql
    SELECT * FROM Sales.SalesOrderHeader
    WHERE OrderDate >= '2014-07-01' AND OrderDate < '2014-08-01';
    ```

Nota :
l'ottimizzazione delle query può dipendere da vari fattori, tra cui l'architettura del database, gli indici esistenti, le dimensioni delle tabelle, ecc. Questi esempi sono intesi come linee guida generali e potrebbero non applicarsi a tutti i casi o a tutte le configurazioni del database.



Certo, ecco altri dieci esempi di query SQL che potrebbero essere ottimizzate:

6. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.Customer 
    WHERE PersonID IN (SELECT BusinessEntityID FROM Person.Person WHERE FirstName = 'John');
    ```
    Ottimizzazione:
    
    Possiamo eliminare la subquery utilizzando un JOIN.
    
    Query ottimizzata:
    ```sql
    SELECT c.*
    FROM Sales.Customer c
    JOIN Person.Person p ON c.PersonID = p.BusinessEntityID
    WHERE p.FirstName = 'John';
    ```

7. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Production.Product 
    WHERE ProductID = (SELECT ProductID FROM Sales.SalesOrderDetail WHERE OrderQty = (SELECT MAX(OrderQty) FROM Sales.SalesOrderDetail));
    ```
    Ottimizzazione:
    
    Possiamo ridurre la subquery nidificata utilizzando un JOIN.
    
    Query ottimizzata:
    ```sql
    SELECT p.*
    FROM Production.Product p
    JOIN Sales.SalesOrderDetail sod ON p.ProductID = sod.ProductID
    WHERE sod.OrderQty = (SELECT MAX(OrderQty) FROM Sales.SalesOrderDetail);
    ```

8. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.Customer 
    WHERE TerritoryID IN (SELECT TerritoryID FROM Sales.SalesTerritory WHERE CountryRegionCode = 'US' AND Group = 'North America');
    ```
    Ottimizzazione:
    
    Possiamo sostituire la clausola IN con un JOIN.
    
    Query ottimizzata:
    ```sql
    SELECT c.*
    FROM Sales.Customer c
    JOIN Sales.SalesTerritory st ON c.TerritoryID = st.TerritoryID
    WHERE st.CountryRegionCode = 'US' AND st.Group = 'North America';
    ```

9. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Production.Product 
    WHERE ListPrice > (SELECT AVG(ListPrice) FROM Production.Product WHERE Color = 'Black');
    ```
    Ottimizzazione:
    
    Possiamo migliorare l'efficienza calcolando la media una volta e memorizzandola in una variabile.
    
    Query ottimizzata:
    ```sql
    DECLARE @average_black_price DECIMAL(18, 2);
    SELECT @average_black_price = AVG(ListPrice) FROM Production.Product WHERE Color = 'Black';
    SELECT * FROM Production.Product WHERE ListPrice > @average_black_price;
    ```

10. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.SalesOrderHeader
    WHERE YEAR(OrderDate) = 2014 AND DAY(OrderDate) = 15;
    ```
    Ottimizzazione:
    
    Utilizzare funzioni sugli attributi nella clausola WHERE può impedire l'uso degli indici. E' meglio utilizzare una range query.
    
    Query ottimizzata:
    ```sql
    SELECT * FROM Sales.SalesOrderHeader
    WHERE OrderDate >= '2014-01-15' AND OrderDate < '2014-01-16';
    ```

11. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.SalesOrderHeader
    WHERE YEAR(OrderDate) = 2013 AND MONTH(OrderDate) = 6;
    ```
    Ottimizzazione:

    Si può migliorare utilizzando un range di date.

    Query ottimizzata

:
    ```sql
    SELECT * FROM Sales.SalesOrderHeader
    WHERE OrderDate >= '2013-06-01' AND OrderDate < '2013-07-01';
    ```

12. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM HumanResources.Employee 
    WHERE JobTitle LIKE '%Marketing%';
    ```
    Ottimizzazione:
    
    Utilizzare l'operatore LIKE con un jolly all'inizio può essere costoso. Se possibile, limitare l'uso dei jolly.
    
    Query ottimizzata:
    ```sql
    SELECT * FROM HumanResources.Employee
    WHERE JobTitle LIKE 'Marketing%';
    ```

13. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Production.Product 
    WHERE ListPrice > ALL (SELECT ListPrice FROM Production.Product WHERE Color = 'Black');
    ```
    Ottimizzazione:
    
    Si può utilizzare MAX() invece di ALL per migliorare le prestazioni.
    
    Query ottimizzata:
    ```sql
    DECLARE @max_black_price DECIMAL(18, 2);
    SELECT @max_black_price = MAX(ListPrice) FROM Production.Product WHERE Color = 'Black';
    SELECT * FROM Production.Product WHERE ListPrice > @max_black_price;
    ```

14. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Production.Product 
    WHERE ProductID = ANY (SELECT ProductID FROM Sales.SalesOrderDetail WHERE OrderQty > 10);
    ```
    Ottimizzazione:
    
    Possiamo eliminare la subquery utilizzando un JOIN.
    
    Query ottimizzata:
    ```sql
    SELECT p.*
    FROM Production.Product p
    JOIN Sales.SalesOrderDetail sod ON p.ProductID = sod.ProductID
    WHERE sod.OrderQty > 10;
    ```

15. Esercizio:

    Query originale:
    ```sql
    SELECT * FROM Sales.SalesOrderDetail 
    WHERE UnitPrice < ANY (SELECT ListPrice FROM Production.Product WHERE Color = 'Red');
    ```
    Ottimizzazione:
    
    Possiamo migliorare l'efficienza utilizzando MIN() invece di ANY.
    
    Query ottimizzata:
    ```sql
    DECLARE @min_red_price DECIMAL(18, 2);
    SELECT @min_red_price = MIN(ListPrice) FROM Production.Product WHERE Color = 'Red';
    SELECT * FROM Sales.SalesOrderDetail WHERE UnitPrice < @min_red_price;
    ```

Le ottimizzazioni specifiche possono dipendere da molti fattori, come la struttura del database, l'hardware su cui il database è ospitato, la dimensione dei dati e così via. Alcune delle ottimizzazioni proposte potrebbero non essere applicabili in ogni situazione, ma forniscono un punto di partenza per pensare a come migliorare le prestazioni delle query SQL.






































