# &#x20;               **Zepto Retail Analytics DashBoard**

# 



#### 1] CREATING THE TABLE



create table zepto(

sku\_id SERIAL primary key,

Category varchar(120),

Name varchar(150) not null,

MRP numeric(8,2),

DiscountPercent numeric(5,2),

AvailableQuantity integer,

DiscountedSellingPrice numeric(8,2),

WeightInGms integer,

OutOfStock boolean,

Quantity integer

);



select \* from zepto



#### 2] NULL VALUES



&#x20;select \* from zepto

&#x20;where

&#x20;name is null

&#x20;or category is null

&#x20;or mrp is null

&#x20;or discountpercent is null

&#x20;or availablequantity is null

&#x20;or discountedsellingprice is null

&#x20;or weightingms is null

&#x20;or outofstock is null

&#x20;or quantity is null;



#### &#x20;3] DIFFERENT PRODUCT CATEGORIES



&#x20;select distinct category from zepto

&#x20;order by category;



#### &#x20;4] PRODUCTS IN STOCK VS OUTOFF STOCK



&#x20;select outofstock, count(sku\_id)

&#x20;from zepto

&#x20;group by outofstock;



#### &#x20;5] PRODUCT NAME MORE THAN 1



&#x20;select name ,count(sku\_id) as Number\_of\_sku

&#x20;from zepto

&#x20;group by name

&#x20;having count(sku\_id)>1

&#x20;order by count(sku\_id) desc;



#### &#x20;6] DATA CLEANING



###### &#x20;1> deleting the record



product with price=0

select \* from zepto

where mrp= 0;



delete from zepto

where mrp=0;



###### 2> convert paise into rupees



update zepto

set mrp=mrp/100.0,

discountedsellingprice=discountedsellingprice/100.0;



\-----------------------------------------------------------------------------------------------------------------------------------------------------------------------



* ### BUSINESS INSIGHTS



###### Q.1] find the top 10 best value products based on the discount percentage



select distinct name,mrp, discountpercent

from zepto

order by discountpercent desc

limit 10;



###### Q.2] what are the products with high mrp but outof stock



select distinct name, mrp, outofstock

from zepto

where outofstock='true'

order by mrp desc;



###### Q.3] calculate estimated revenue for each category



select category, sum(discountedsellingprice \* availableQuantity) as revenue

from zepto

group by category

order by revenue;

###### 

###### Q.4] find all products where mrp is greater than 500 and discount is less than 10%



select distinct name , mrp, discountpercent

from zepto

where mrp>500 and discountpercent<10.00

order by mrp desc, discountpercent desc;



###### Q.5] identify the top 5 categories offering the highest average discount percentage



select category, round(avg(discountpercent),2) Avg\_discount

from zepto

group by category

order by avg\_discount desc

limit 5;



###### Q.6] find the price per gram for product above 100g and sort by best value



select distinct name, weightingms, discountedsellingprice,

round(discountedsellingprice/weightingms,2) as Price\_per\_gram

from zepto

where weightingms >= 100

order by price\_per\_gram ;



###### Q.7] group the products into categories like low, medium, bulk



select distinct name, weightingms,

case

when weightingms <1000 then 'Low'

when weightingms <5000 then 'Medium'

else 'Bulk'

end as weight\_category

from zepto;



###### Q.8] what is the total inventory weight per category



&#x20;select category, sum(weightingms \* availablequantity) as Total\_weight

&#x20;from zepto

group by category

order by total\_weight;









select \* from zepto;

