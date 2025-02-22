# [SQLZoo - AdvantureWorks](https://sqlzoo.net/wiki/AdventureWorks)

## Table Name (Column1, Column2, Column3,...)

>[!NOTE]
>Primary Key in **Bold** and _Italics_

Customer(**_CustomerID_**, FirstName, MiddleName, LastName, CompanyName, EmailAddress)

CustomerAddress(**_CustomerID_**, AddressID, **_AddressType_**)

Address(**_AddressID_**, AddressLine1, AddressLine2, City, StateProvince, CountyRegion, PostalCode)

SalesOrderHeader(**_SalesOrderID_**, RevisionNumber, OrderDate, CustomerID, BillToAddressID, ShipToAddressID, ShipMethod, SubTotal, TaxAmt, Freight)

SalesOrderDetail(SalesOrderID, **_SalesOrderDetailID_**, OrderQty, ProductID, UnitPrice, UnitPriceDiscount)

Product(**_ProductID_**, Name, Color, ListPrice, Size, Weight, ProductModelID, ProductCategoryID)

ProductModel(**_ProductModelID_**, Name)

ProductCategory(**_ProductCategoryID_**, ParentProductCategoryID, Name)

ProductModelProductDescription(**_ProductModelID_**, ProductDescriptionID, **_Culture_**)

ProductDescription(**_ProductDescriptionID_**, Description)

## Entity Relationship Diagram

![AdventureWorks](https://github.com/user-attachments/assets/845abdec-5e03-4a3c-91dd-83c5ce19a996)



