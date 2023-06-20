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









