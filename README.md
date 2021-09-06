# sql запросы 

## заполнение таблицы productType:

```sql
USE [mloginov]
GO

INSERT INTO [dbo].[ProductType]
           ([Title], [DefectedPercent])
SELECT 
 	distinct [Тип продукции], 0  
FROM [dbo].[products_k_import$]
```

## заполнение таблицы продукты:

```sql
USE [mloginov]
GO
INSERT INTO [dbo].[Product]
           ([Title] ,[ProductTypeID],[ArticleNumber ,[Image],[ProductionPersonCount]  ,[ProductionWorkshopNumber]  ,[MinCostForAgent])
SELECT 
 [Наименование продукции],pt.ID,[Артикул],[Изображение],[Количество человек для производства],[Номер цеха для производства],[Минимальная стоимость для агента]
 FROM [dbo].[products_k_import$] ki, ProductType pt
 where ki.[Тип продукции]=pt.Title
 ```
  
## заполнение таблицы materialType:

```sql
USE [mloginov]
GO
INSERT INTO [dbo].[MaterialType]
	([Title],[DefectedPercent]) 
SELECT
     	distinct [ Тип материала],0
     
FROM [dbo].[materials_short_k_import$]
```

## заполнение таблицы material:  

```sql
USE [mloginov]
GO
INSERT INTO [dbo].[Material]
           ([Title],[CountInPack],[Unit],[CountInStock],[MinCount],[Cost],[MaterialTypeID])
SELECT 
	[Наименование материала]  ,[ Количество в упаковке],[ Единица измерения],[ Количество на складе],[ Минимальный возможный остаток],[ Стоимость],mt.ID
FROM [dbo].[materials_short_k_import$]mi,MaterialType mt
where mi.[ Тип материала]=mt.Title
```


## заполнение таблицы productmaterial:  

```sql
USE [mloginov]
GO

INSERT INTO [dbo].[ProductMaterial]
           ([ProductID],[MaterialID],[Count])
SELECT 
	p.ID,m.ID,l.[Необходимое количество материала] 
FROM [dbo].[Product] p,Material m, Лист1$ l
where p.Title=l.Продукция and m.[Title]=l.[Наименование материала]
```
