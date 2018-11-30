# Laboratorni-roboty
1) Вибрати самий дорогий заказ з підзапросами і без підзапросів

1.1  SELECT max(TotalDue)
    FROM SalesLT.SalesOrderHeader

   1.2  SELECT TotalDue
     FROM SalesLT.SalesOrderHeader
     WHERE TotalDue IN (SELECT max(TotalDue)
                         FROM SalesLT.SalesOrderHeader)

2)Выбрать покупателей, которые купили менее 20 единиц товара с подзапросом и без подзапроса

2.1) SELECT FirstName,MiddleName,LastName,OrderQty
    FROM (SalesLT.Customer a join SalesLT.SalesOrderHeader c
    ON (a.CustomerID=c.CustomerID) join SalesLT.SalesOrderDetail s ON
    (c.SalesOrderID=s.SalesOrderID)
    WHERE s.OrderQty < 20


2.2)  SELECT a.FirstName,a.MiddleName,a.LastName
    FROM  (SalesLT.Customer a join SalesLT.SalesOrderHeader c
     ON a.CustomerID=c.CustomerID) join SalesLT.SalesOrderDetail s ON
     (c.SalesOrderID=s.SalesOrderID)
     WHERE s.OrderQty IN (SELECT OrderQty
                          FROM SalesLT.SalesOrderDetail
                          WHERE OrderQty < 20)


3) Сформировать отчет по продажам в разрезе городов и месяцев(Город, Год, Месяц, Сумма продаж)

    SELECT a.City,YEAR(h.DueDate),MONTH(h.DueDate),sum(h.TotalDue)
  FROM SalesLT.SalesOrderHeader h inner join SalesLT.Address a
  ON (h.BillToAddressID = a.AddressID)
  WHERE TotalDue IN (SELECT max(TotalDue)
                      FROM SalesLT.SalesOrderHeader)
   GROUP BY a.City,h.TotalDue,h.DueDate




4)  Сформировать отчет по самым дорогим заказам в разрезе служб доставки(Служба доставки,  № заказа, Дата Заказа, Сумма)

    SELECT ShipMethod,PurchaseOrderNumber,DueDate,TotalDue
   FROM SalesLT.SalesOrderHeader
   WHERE TotalDue IN (SELECT max(TotalDue)
                      FROM SalesLT.SalesOrderHeader)
   GROUP BY ShipMethod,PurchaseOrderNumber,DueDate,TotalDue

